---
title: "Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen | Microsoft-Dokumentation"
description: "Erstellen Sie einen Linux-Cluster mit virtuellen Computern der Größe H16r, H16mr, A8 oder A9, um das Azure RDMA-Netzwerk für die Ausführung von MPI-Apps zu verwenden."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 4b2ceb64b1737918458f6d5c692fc2bfbc0f12ed
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen
Hier erfahren Sie, wie Sie in Azure einen Linux RDMA-Cluster mit [HPC-VM-Größen (High Performance Computing)](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) einrichten, um parallele MPI-Anwendungen (Message Passing Interface) auszuführen. Dieser Artikel enthält die Schritte, die ausgeführt werden müssen, um ein Linux-HPC-Image für die Ausführung von Intel MPI auf einem Cluster vorzubereiten. Im Anschluss an die Vorbereitung stellen Sie mithilfe dieses Images und einer der RDMA-fähigen Größen für virtuelle Azure-Computer (derzeit H16r, H16mr, A8 oder A9) einen Cluster mit virtuellen Computern bereit. Mit dem Cluster können Sie MPI-Anwendungen ausführen, die effizient mit geringer Latenz und hohem Durchsatz über ein RDMA-Netzwerk (Remote Direct Memory Access, Remotezugriff auf den direkten Speicher) kommunizieren.

> [!IMPORTANT]
> Azure bietet zwei Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager](../../../resource-manager-deployment-model.md) und klassisch. Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## <a name="cluster-deployment-options"></a>Optionen für die Clusterbereitstellung
Mit den folgenden Methoden können Sie einen Linux RDMA-Cluster mit oder ohne Auftragsplanung erstellen.

* **Azure CLI-Skripts:** Erstellen Sie mithilfe der [Azure-Befehlszeilenschnittstelle](../../../cli-install-nodejs.md) (Command-Line Interface, CLI) ein Skript zur Bereitstellung eines Clusters mit RDMA-fähigen virtuellen Computern (wie weiter unten in diesem Artikel gezeigt). Da Clusterknoten im Rahmen des klassischen Bereitstellungsmodells im Dienstverwaltungsmodus der Befehlszeilenschnittstelle seriell erstellt werden, kann die Bereitstellung einer großen Anzahl von Serverknoten mehrere Minuten dauern. Um die RDMA-Netzwerkverbindung im Rahmen des klassischen Bereitstellungsmodells aktivieren zu können, müssen die virtuellen Computer im gleichen Clouddienst bereitgestellt werden.
* **Azure Resource Manager-Vorlagen**: Sie können auch das Resource Manager-Bereitstellungsmodell verwenden, um einen Cluster mit RDMA-fähigen virtuellen Computern bereitzustellen, der eine Verbindung mit dem RDMA-Netzwerk herstellt. Sie können [eine eigene Vorlage erstellen](../../../resource-group-authoring-templates.md) oder in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) nach Vorlagen von Microsoft oder der Community suchen, um die gewünschte Lösung bereitzustellen. Ressourcen-Manager-Vorlagen stellen eine schnelle und zuverlässige Möglichkeit zum Bereitstellen eines Linux-Clusters dar. Um die RDMA-Netzwerkverbindung im Rahmen des Resource Manager-Bereitstellungsmodells aktivieren zu können, müssen die virtuellen Computer in der gleichen Verfügbarkeitsgruppe bereitgestellt werden.
* **HPC Pack**: Erstellen Sie in Azure einen Microsoft HPC Pack-Cluster, und fügen Sie RDMA-fähige Computeknoten hinzu, auf denen eine unterstützte Linux-Distribution für den Zugriff auf das RDMA-Netzwerk ausgeführt wird. Weitere Informationen finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Beispielschritte für die Bereitstellung im Rahmen des klassischen Modells
Die folgenden Schritte zeigen, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle über den Azure Marketplace einen virtuellen HPC-Computer unter SUSE Linux Enterprise Server (SLES) 12 SP1 bereitstellen, den virtuellen Computer anpassen und ein benutzerdefiniertes VM-Image erstellen. Mit dem Image können Sie dann ein Skript für die Bereitstellung eines Clusters mit RDMA-fähigen virtuellen Computern erstellen.

> [!TIP]
> Ein Cluster mit RDMA-fähigen virtuellen Computern, die auf anderen CentOS-basierten HPC-Images aus dem Azure Marketplace basieren, kann auf ganz ähnliche Weise bereitgestellt werden. Auf Abweichungen bei den Schritten wird ggf. hingewiesen. 
>
>

### <a name="prerequisites"></a>Voraussetzungen
* **Clientcomputer**: Sie benötigen einen Mac-, Linux- oder Windows-basierten Clientcomputer für die Kommunikation mit Azure. Bei diesen Schritten wird davon ausgegangen, dass Sie einen Linux-Client verwenden.
* **Azure-Abonnement**: Falls Sie über kein Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) einrichten. Bei größeren Clustern empfiehlt sich die Verwendung eines Abonnements mit nutzungsbasierter Bezahlung oder einer anderen Kaufoption.
* **Verfügbare VM-Größen**: Derzeit sind folgende Instanzgrößen für RDMA geeignet: H16r, H16mr, A8 und A9. Informationen zur Verfügbarkeit in den Azure-Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/) .
* **Kernnutzungskontingent**: Zur Bereitstellung eines Clusters mit rechenintensiven virtuellen Computern ist unter Umständen eine Erhöhung des Kontingents für Kerne erforderlich. Beispielsweise benötigen Sie mindestens 128 Kerne, wenn Sie, wie in diesem Artikel dargestellt, acht A9-VMs bereitstellen möchten. Möglicherweise ist bei Ihrem Abonnement auch die Anzahl von Kernen beschränkt, die in bestimmten VM-Größenkategorien bereitgestellt werden können. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../../../azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern.
* **Azure CLI**: [Installieren](../../../cli-install-nodejs.md) Sie die Azure-Befehlszeilenschnittstelle, und [stellen Sie auf dem Clientcomputer eine Verbindung mit dem Azure-Abonnement her](../../../xplat-cli-connect.md).

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>Bereitstellen eines virtuellen HPC-Computers unter SLES 12 SP1
Führen Sie nach der Anmeldung bei Azure über die Azure-Befehlszeilenschnittstelle den Befehl `azure config list` aus, um sicherzustellen, dass der Dienstverwaltungsmodus verwendet wird. Wenn nicht, aktivieren Sie den Modus durch Ausführen dieses Befehls:

    azure config mode asm


Geben Sie Folgendes ein, um alle Abonnements aufzulisten, zu deren Verwendung Sie berechtigt sind:

    azure account list

Das aktuelle aktive Abonnement erkennen Sie daran, dass für `Current` der Wert `true` angegeben ist. Ist dies nicht das Abonnement, das Sie zum Erstellen des Clusters verwenden möchten, legen Sie die entsprechende Abonnement-ID als aktives Abonnement fest:

    azure account set <subscription-Id>

Führen Sie einen Befehl wie den folgenden aus, um die öffentlich verfügbaren SLES 12 SP1-HPC-Images in Azure anzuzeigen (vorausgesetzt, Ihre Shellumgebung unterstützt **grep**):

    azure vm image list | grep "suse.*hpc"

Stellen Sie einen RDMA-fähigen virtuellen Computer mit einem SLES 12 SP1-HPC-Image bereit, indem Sie einen Befehl wie den folgenden ausführen:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Hierbei gilt:

* Die Größe (in diesem Beispiel A9) ist eine der RDMA-fähigen VM-Größen.
* Die externe SSH-Portnummer (in diesem Beispiel der SSH-Standardwert 22) ist eine beliebige gültige Portnummer. Die interne SSH-Portnummer ist auf 22 festgelegt.
* Ein neuer Clouddienst wird in der durch den Standort festgelegten Azure-Region erstellt. Geben Sie einen Ort an, an dem die gewählte VM-Größe verfügbar ist.
* Für kostenpflichtigen SUSE Priority Support kann der Name des SLES 12 SP1-Images eine dieser beiden Varianten sein: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-the-vm"></a>Anpassen des virtuellen Computers
Nach Abschluss der VM-Bereitstellung stellen Sie mithilfe der externen IP-Adresse des virtuellen Computers (oder des DNS-Namens) und der externen Portnummer, die Sie konfiguriert haben, eine SSH-Verbindung zum virtuellen Computer her, und nehmen Sie die Anpassungen vor. Weitere Informationen zur Verbindung finden Sie unter [Anmelden bei einem mit Linux betriebenen virtuellen Computer](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sofern zum Ausführen eines Schritts kein Stammzugriff erforderlich ist, führen Sie Befehle als der Benutzer aus, den Sie für den virtuellen Computer konfiguriert haben.

> [!IMPORTANT]
> Microsoft Azure bietet keinen Stammzugriff auf Linux-VMs. Wenn Sie beim virtuellen Computer als Benutzer angemeldet sind, können Sie Befehle mit `sudo`ausführen, um Administratorzugriff zu erhalten.
>
>

* **Updates**: Installieren Sie Updates mit zypper. Sie können auch NFS-Dienstprogramme installieren.

  > [!IMPORTANT]
  > Bei einem virtuellen HPC-Computer unter SLES 12 SP1 empfiehlt es sich nicht, Kernel-Updates zu installieren, da dies zu Problemen mit den Linux RDMA-Treibern führen kann.
  >
  >
* **Intel MPI**: Führen Sie den folgenden Befehl aus, um die Installation von Intel MPI auf dem virtuellen HPC-Computer unter SLES 12 SP1 abzuschließen:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Sperren des Arbeitsspeichers**: Um den für RDMA verfügbaren Arbeitsspeicher mit MPI-Codes zu sperren, müssen Sie die folgenden Einstellungen in der Datei „/etc/security/limits.conf“ hinzufügen oder ändern. Sie benötigen Root-Zugriff zum Bearbeiten dieser Datei.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Zu Testzwecken können Sie "memlock" auch auf "unbegrenzt" festlegen. Beispiel: `<User or group name>    hard    memlock unlimited`. Weitere Informationen finden Sie unter [Best Known Methods for Setting Locked Memory Size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size) (Beste bekannte Methoden zum Festlegen der Größe des gesperrten Arbeitsspeichers).
  >
  >
* **SSH-Schlüssel für virtuelle SLES-Computer**: Generieren Sie SSH-Schlüssel, um für Ihr Benutzerkonto beim Ausführen von MPI-Aufträgen eine Vertrauensstellung zwischen den Computeknoten im SLES-Cluster einzurichten. Überspringen Sie diesen Schritt, wenn Sie eine CentOS-basierte HPC-VM bereitgestellt haben. Anweisungen zum Einrichten einer kennwortlosen SSH-Vertrauensstellung zwischen den Clusterknoten nach Erfassung des Images und Bereitstellung des Clusters finden Sie weiter unten in diesem Artikel.

    Führen Sie den folgenden Befehl aus, um SSH-Schlüssel zu erstellen. Wenn Sie zur Eingabe aufgefordert werden, drücken Sie einfach die **EINGABETASTE**, um die Schlüssel am Standardspeicherort zu erstellen, ohne eine Passphrase festzulegen.

        ssh-keygen

    Fügen Sie den öffentlichen Schlüssel der Datei „authorized_keys“ für bekannte öffentliche Schlüssel an.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Bearbeiten oder erstellen Sie im Verzeichnis „~/.ssh“ die Datei „config“. Geben Sie den IP-Adressbereich des privaten Netzwerks an, das Sie in Azure verwenden möchten (in diesem Beispiel: 10.32.0.0/16):

        host 10.32.0.*
        StrictHostKeyChecking no

    Führen Sie alternativ die VPN-IP-Adressen der einzelnen virtuellen Computer in Ihrem Cluster wie folgt auf:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Das Konfigurieren von `StrictHostKeyChecking no` kann ein potenzielles Sicherheitsrisiko darstellen, wenn eine bestimmte IP-Adresse oder ein bestimmter Adressbereich nicht angegeben wird.
  >
  >
* **Anwendungen**: Installieren Sie alle benötigten Anwendungen, oder führen Sie andere Anpassungen aus, bevor Sie das Image erfassen.

### <a name="capture-the-image"></a>Erfassen des Images
Zum Erfassen des Images führen Sie zuerst den folgenden Befehl auf dem virtuellen Linux-Computer aus. Dieser Befehl hebt die Bereitstellung des virtuellen Computers auf, eingerichtete Benutzerkonten und SSH-Schlüssel werden jedoch beibehalten.

```
sudo waagent -deprovision
```

Führen Sie auf Ihrem Clientcomputer die folgenden Azure CLI-Befehle zum Erfassen des Images aus. Weitere Informationen dazu finden Sie unter [Erfassen eines klassischen virtuellen Linux-Computers als Image](capture-image.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nachdem Sie diese Befehle ausgeführt haben, wird das Image des virtuellen Computers für Ihre Verwendung erfasst, und der virtuelle Computer wird gelöscht. Jetzt ist Ihr benutzerdefiniertes Image zur Bereitstellung eines Clusters fertig.

### <a name="deploy-a-cluster-with-the-image"></a>Bereitstellen eines Clusters mit dem Image
Ändern Sie das folgende Bash-Skript mit den entsprechenden Werten für Ihre Umgebung, und führen Sie es auf Ihrem Clientcomputer aus. Da die virtuellen Computer im klassischen Bereitstellungsmodell von Azure seriell bereitgestellt werden, dauert es einige Minuten, bis die acht im Skript vorgeschlagenen virtuellen A9-Computer bereitgestellt wurden.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Zu berücksichtigende Aspekte bei einem CentOS-HPC-Cluster
Wenn Sie anstelle von SLES 12 für HPC einen Cluster auf der Grundlage eines der CentOS-basierten HPC-Images aus dem Azure Marketplace einrichten möchten, führen Sie die allgemeinen Schritte des vorherigen Abschnitts aus. Beim Bereitstellen und Konfigurieren des virtuellen Computers sind folgende Unterschiede zu beachten:

- Auf virtuellen Computern, die auf der Grundlage eines CentOS-basierten HPC-Images bereitgestellt werden, ist Intel MPI bereits installiert.
- Einstellungen zum Sperren von Arbeitsspeicher sind in der Datei „/etc/security/limits.conf“ des virtuellen Computers bereits enthalten.
- Generieren Sie auf dem virtuellen Computer, den Sie zur Erfassung bereitstellen, keine SSH-Schlüssel. Richten Sie stattdessen nach der Bereitstellung des Clusters eine benutzerbasierte Authentifizierung ein. Weitere Informationen finden Sie im folgenden Abschnitt.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Einrichten einer kennwortlosen SSH-Vertrauensstellung für den Cluster
Bei einem CentOS-basierten HPC-Cluster kann die Vertrauensstellung zwischen den Computeknoten auf zwei Arten eingerichtet werden: mit einer hostbasierten Authentifizierung oder mit einer benutzerbasierten Authentifizierung. Die hostbasierte Authentifizierung wird in diesem Artikel nicht behandelt und muss grundsätzlich mithilfe eines Erweiterungsskripts während der Bereitstellung erfolgen. Mit der benutzerbasierten Authentifizierung können Sie nach der Bereitstellung komfortabel eine Vertrauensstellung einrichten. Zu diesem Zweck müssen SSH-Schlüssel generiert und von den Computeknoten im Cluster genutzt werden. Diese Methode wird häufig als SSH-Anmeldung ohne Kennwort bezeichnet und ist zum Ausführen von MPI-Aufträgen erforderlich.

Auf [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) steht ein Beispielskript aus der Community zur Verfügung, das eine einfache Benutzerauthentifizierung in einem CentOS-basierten HPC-Cluster ermöglicht. Laden und verwenden Sie dieses Skript wie im Folgenden beschrieben. Sie können das Skript auch bearbeiten oder eine andere Methode verwenden, um eine kennwortlose SSH-Authentifizierung zwischen den Computeknoten des Clusters einzurichten.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

Für die Skriptausführung müssen Sie das Präfix Ihrer Subnetz-IP-Adressen kennen. Ermitteln Sie das Präfix durch Ausführung des folgenden Befehls auf einem der Clusterknoten. Die Ausgabe sollte in etwa wie folgt aussehen: 10.1.3.5. In diesem Fall wäre „10.1.3“ das Präfix.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Führen Sie nun das Skript aus, und geben Sie dabei drei Parameter an: den gemeinsamen Benutzernamen auf den Computeknoten, das gemeinsame Kennwort für diesen Benutzer auf den Computeknoten und das zurückgegebene Subnetzpräfix aus dem vorherigen Befehl.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Das Skript bewirkt Folgendes:

* Es erstellt auf dem Hostknoten ein Verzeichnis namens „.ssh“ (wird für die Anmeldung ohne Kennwort benötigt).
* Es erstellt im Verzeichnis „.ssh“ eine Konfigurationsdatei, mit der die kennwortlose Anmeldung angewiesen wird, Anmeldungen von beliebigen Knoten im Cluster zuzulassen.
* Es erstellt Dateien mit den Namen und IP-Adressen aller Knoten im Cluster. Diese Dateien bleiben nach Ausführung des Skripts zu Referenzzwecken erhalten.
* Es erstellt ein Schlüsselpaar mit privatem und öffentlichem Schlüssel für jeden Clusterknoten (auch für den Hostknoten) und erstellt einen Eintrag in der Datei „authorized_keys“.

> [!WARNING]
> Die Ausführung dieses Skripts stellt ein potenzielles Sicherheitsrisiko dar. Stellen Sie sicher, dass die Informationen des öffentlichen Schlüssels in „~/.ssh“ nicht weitergegeben werden.
>
>

## <a name="configure-intel-mpi"></a>Konfigurieren von Intel MPI
Zum Ausführen von MPI-Anwendungen auf Azure Linux RDMA müssen Sie bestimmte, für Intel MPI spezifische Umgebungsvariablen konfigurieren. Im Anschluss sehen Sie ein Bash-Beispielskript zum Konfigurieren der Variablen, die zum Ausführen einer Anwendung benötigt werden. Passen Sie ggf. den Pfad zu „mpivars.sh“ für Ihre Installation von Intel MPI an.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Das Format der Hostdatei sieht wie folgt aus. Fügen Sie eine Zeile für jeden Knoten im Cluster hinzu. Geben Sie private IP-Adressen aus dem zuvor definierten VNet an, keine DNS-Namen. Für zwei Hosts mit den IP-Adressen 10.32.0.1 und 10.32.0.2 enthält die Datei beispielsweise Folgendes:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Ausführen von MPI in einem einfachen Cluster mit zwei Knoten
Richten Sie zunächst die Umgebung für Intel MPI ein, sofern noch nicht geschehen.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Ausführen eines MPI-Befehls
Führen Sie einen MPI-Befehl auf einem der Computeknoten aus, um zu überprüfen, ob MPI ordnungsgemäß installiert ist und die Kommunikation zwischen mindestens zwei Computerknoten möglich ist. Mit dem folgenden **mpirun**-Befehl wird der **hostname**-Befehl auf zwei Knoten ausgeführt.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
In Ihrer Ausgabe sollten die Namen aller Knoten aufgeführt sein, die Sie als Eingabe für `-hosts`übergeben haben. Ein **mpirun**-Befehl mit zwei Knoten gibt beispielsweise eine Ausgabe zurück, die etwa wie folgt aussieht:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Ausführen eines MPI-Benchmarks
Mit dem folgenden Intel MPI-Befehl werden die Clusterkonfiguration und die Verbindung mit dem RDMA-Netzwerk mithilfe eines Pingpongbenchmarks überprüft.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

In einem funktionierenden Cluster mit zwei Knoten sieht die Ausgabe in etwa wie folgt aus. Im Azure RDMA-Netzwerk ist für Nachrichten mit bis zu 512 Bytes eine Latenz von maximal drei Mikrosekunden zu erwarten.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Nächste Schritte
* Stellen Sie Ihre Linux-MPI-Anwendungen im Linux-Cluster bereit, und führen Sie sie aus.
* Anleitungen zu Intel MPI finden Sie in der [Dokumentation zu Intel MPI Library](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) .
* Verwenden Sie eine [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) , um einen Intel Lustre-Cluster unter Verwendung eines CentOS-basierten HPC-Images zu erstellen. Weitere Informationen finden Sie unter [Bereitstellen von Intel Cloud Edition für Lustre in Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).

