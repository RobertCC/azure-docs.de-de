---
title: "Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell – Azure| Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie administrative Aufgaben für Hadoop-Cluster in HDInsight mit Azure PowerShell ausführen."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 3522cae228e92b47023cfca217e09c2e2104190b
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell kann zum Steuern und Automatisieren der Bereitstellung und Verwaltung Ihrer Workloads in Azure verwendet werden. In diesem Artikel erfahren Sie, wie Sie Hadoop-Cluster in Azure HDInsight mithilfe von Azure PowerShell verwalten. Eine Liste der HDInsight PowerShell-Cmdlets finden Sie unter [HDInsight-Cmdlet-Referenz][hdinsight-powershell-reference].

**Voraussetzungen**

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Installieren von Azure PowerShell
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Wenn Sie Azure PowerShell Version 0.9x installiert haben, müssen Sie sie deaktivieren, bevor Sie eine neue Version installieren können.

So überprüfen Sie die Version der installierten PowerShell:

```powershell
Get-Module *azure*
```

Um die ältere Version zu deinstallieren, rufen Sie „Programme und Features“ in der Systemsteuerung auf.

## <a name="create-clusters"></a>Erstellen von Clustern
Siehe [Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Auflisten von Clustern
Verwenden Sie den folgenden Befehl, um alle Cluster des aktuellen Abonnements aufzulisten:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Anzeigen von Clustern
Verwenden Sie den folgenden Befehl, um Details zu einem bestimmten Cluster des aktuellen Abonnements anzuzeigen:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Löschen von Clustern
Mit dem folgenden Befehl können Sie ein Cluster löschen:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

Sie können einen Cluster auch löschen, indem Sie die Ressourcengruppe entfernen, die den Cluster enthält. Beim Löschen einer Ressourcengruppe werden alle Ressourcen in der Gruppe, einschließlich des Standardspeicherkontos, gelöscht.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Skalieren von Clustern
Mithilfe der Clusterskalierung können Sie die Anzahl der von einem in Azure HDInsight ausgeführten Cluster verwendeten Workerknoten ändern, ohne den Cluster neu erstellen zu müssen.

> [!NOTE]
> Es werden nur Cluster mit HDInsight-Versionen ab 3.1.3 unterstützt. Überprüfen Sie ggf. auf der Seite „Eigenschaften“ die Version Ihres Clusters.  Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

Auswirkungen der Änderung der Anzahl von Datenknoten für die von HDInsight unterstützten Clustertypen:

* Hadoop

    Sie können die Anzahl der Workerknoten in einem aktiven Hadoop-Cluster problemlos ohne Auswirkungen auf ausstehende oder aktive Aufträge erhöhen. Neue Aufträge können auch während des Vorgangs gesendet werden. Fehler bei einer Skalierung werden ordnungsgemäß behandelt, sodass der Cluster immer in einem funktionsfähigen Zustand verbleibt.

    Wenn ein Hadoop-Cluster durch Verringern der Anzahl der Datenknoten zentral herunterskaliert wird, werden einige der Dienste im Cluster neu gestartet. Ein Neustart von Diensten führt beim Abschluss des Skalierungsvorgangs bei allen aktiven und ausstehenden Aufträgen zu einem Fehler. Sie können die Aufträge jedoch nach Abschluss des Vorgangs erneut senden.
* HBase

    Sie können Knoten reibungslos Ihrem HBase-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Regionale Server werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Allerdings können Sie die regionalen Server auch manuell ausgleichen, indem Sie sich am Hauptknoten des Clusters anmelden und in einem Eingabeaufforderungsfenster die folgenden Befehle ausführen:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Storm

    Sie können Datenknoten übergangslos zum Storm-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Nach erfolgreichen Abschluss des Skalierungsvorgangs müssen Sie die Topologie neu ausgleichen.

    Es stehen zwei Methoden für den erneuten Ausgleich zur Verfügung:

  * Storm-Webbenutzeroberfläche
  * Befehlszeilenschnittstelle (CLI)

    Weitere Informationen finden Sie in der [Apache Storm-Dokumentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    Die Storm-Webbenutzeroberfläche ist für den HDInsight-Cluster verfügbar:

    ![HDInsight Storm-Skalierung ausgleichen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Es folgt ein Beispiel, wie die Storm-Topologie mithilfe des CLI-Befehls neu ausgeglichen werden kann:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Führen Sie den folgenden Befehl auf einem Clientcomputer aus, um die Hadoop-Clustergröße mithilfe von Azure PowerShell zu ändern:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>Gewähren/Entziehen des Zugriffs
In HDInsight-Clustern stehen die folgenden HTTP-Webdienste zur Verfügung (alle mit RESTful-Endpunkten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Der Zugriff auf diese Dienste wird standardmäßig gewährt. Sie können den Zugriff widerrufen/gewähren. Zum Widerrufen:

```powershell
Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

Zum Gewähren:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = "<Enter the Password>"
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]
> Durch Gewähren/Widerrufen des Zugriffs werden der Benutzername und das Kennwort des Clusterbenutzers zurückgesetzt.
>
>

Der Zugriff kann auch über das Portal gewährt bzw. widerrufen werden. Siehe [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>Aktualisieren von HTTP-Anmeldeinformationen
Dabei handelt es sich um die gleiche Vorgehensweise wie beim [Gewähren/Widerrufen des HTTP-Zugriffs](#grant/revoke-access). Wenn dem Cluster der HTTP-Zugriff gewährt wurde, müssen Sie ihn zunächst widerrufen.  Gewähren Sie anschließend den Zugriff mit den neuen HTTP-Anmeldeinformationen.

## <a name="find-the-default-storage-account"></a>Suchen des Standardspeicherkontos
Das folgende PowerShell-Skript veranschaulicht, wie der Name und zugehörigen Informationen des Standardspeicherkontos abgerufen werden:

```powershell
#Login-AzureRmAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Suchen der Ressourcengruppe
Im Resource Manager-Modus gehört jeder HDInsight-Cluster einer Azure-Ressourcengruppe an.  So finden Sie die Ressourcengruppe:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Übermitteln von Aufträgen
**So übermitteln Sie MapReduce-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Hadoop MapReduce-Beispielen in Windows-basiertem HDInsight](hdinsight-run-samples.md).

**So übermitteln Sie Hive-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Hive-Abfragen mit PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**So übermitteln Sie Pig-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Pig-Aufträgen mit PowerShell](hdinsight-hadoop-use-pig-powershell.md).

**So übermitteln Sie Sqoop-Aufträge**

Weitere Informationen finden Sie unter [Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md).

**So übermitteln Sie Oozie-Aufträge**

Weitere Informationen finden Sie unter [Verwenden von Oozie mit Hadoop zum Definieren und Ausführen eines Workflows in HDInsight](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Hochladen von Daten in Azure-Blobspeicher
Siehe [Hochladen von Daten in HDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Weitere Informationen
* [Referenzdokumentation zum HDInsight-Cmdlet][hdinsight-powershell-reference]
* [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal]
* [Verwalten von HDInsight über eine Befehlszeilenschnittstelle][hdinsight-admin-cli]
* [Erstellen von HDInsight-Clustern][hdinsight-provision]
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Programmgesteuerte Übermittlung von Hadoop-Aufträgen][hdinsight-submit-jobs]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png

