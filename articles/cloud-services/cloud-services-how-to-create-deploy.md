---
title: Erstellen und Bereitstellen eines Clouddiensts| Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Clouddienst mithilfe der Schnellerfassung in Azure erstellen und bereitstellen.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Erstellen und Bereitstellen eines Clouddiensts
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-how-to-create-deploy-portal.md)
> * [klassischen Azure-Portal](cloud-services-how-to-create-deploy.md)
> 
> 

Das klassische Azure-Portall bietet zwei Methoden zum Erstellen und Bereitstellen eines Clouddiensts: **Schnellerfassung** und **Benutzerdefiniert erstellen**.

In diesem Thema wird erläutert, wie Sie die Schnellerfassungsmethode zum Erstellen eines neuen Clouddiensts und dann **Hochladen** verwenden, um ein Clouddienstpaket in Azure hochzuladen und bereitzustellen. Wenn Sie diese Methode verwenden, werden im Azure-Portal praktische Links zum Erfüllen aller Anforderungen zur Verfügung gestellt. Wenn Sie Ihren Clouddienst bei der Erstellung auch bereitstellen möchten, können Sie beides mithilfe von **Benutzerdefinierte Erstellung**durchführen.

> [!NOTE]
> Wenn Sie Ihren Clouddienst über Visual Studio Team Services (VSTS) veröffentlichen möchten, verwenden Sie die **Schnellerfassung**. Richten Sie die VSTS-Veröffentlichung dann über **Schnellstart** oder das Dashboard ein.
> 
> 

## <a name="concepts"></a>Konzepte
Für die Bereitstellung einer Anwendung als Clouddienst in Azure sind drei Komponenten erforderlich:

* **Dienstdefinition:**  
  Die Clouddienst-Definitionsdatei (.csdef) definiert das Dienstmodell einschließlich der Rollenanzahl.
* **Dienstkonfiguration:**  
  Die Clouddienst-Konfigurationsdatei (.cscfg) enthält Konfigurationseinstellungen für den Clouddienst sowie einzelne Rollen, darunter die Anzahl der Rolleninstanzen.
* **Dienstpaket:**  
  Das Dienstpaket (.cspkg) enthält den Anwendungscode, die Konfigurationen und die Dienstdefinitionsdatei.

Weitere Informationen zu diesen Komponenten sowie zum Erstellen eines Pakets finden Sie [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Vorbereiten Ihrer App
Bevor Sie einen Clouddienst bereitstellen können, müssen Sie das Clouddienstpaket (CSPKG) aus dem Anwendungscode und eine Clouddienstkonfigurationsdatei (CSCFG) erstellen. Das Azure-SDK stellt Tools zum Vorbereiten dieser erforderlichen Bereitstellungsdateien bereit. Sie können das SDK auf der Seite [Azure-Downloads](https://azure.microsoft.com/downloads/) in der Sprache herunterladen, in der Sie den Anwendungscode entwickeln möchten.

Drei Clouddienstfunktionen benötigen vor dem Export eines Dienstpakets spezielle Konfigurationen:

* Wenn Sie einen Clouddienst bereitstellen möchten, der Secure Sockets Layer (SSL) für die Datenverschlüsselung verwendet, [konfigurieren Sie die Anwendung für SSL](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) .
* Wenn Sie Remotedesktopverbindungen zu Rolleninstanzen konfigurieren möchten, [konfigurieren Sie die Rollen](cloud-services-role-enable-remote-desktop.md) für Remotedesktop.
* Wenn Sie die ausführliche Überwachung für den Clouddienst konfigurieren möchten, aktivieren Sie für den Clouddienst die Azure-Diagnose. *Minimale Überwachung* (die Standardüberwachungsstufe) verwendet Leistungsindikatoren, die aus den Hostbetriebssystemen für Rolleninstanzen (virtuelle Computer) erfasst wurden. Bei der "ausführlichen Überwachung" werden zusätzliche Kennzahlen basierend auf Leistungsdaten innerhalb der Rolleninstanzen erfasst, um eine genauere Analyse von Problemen zu ermöglichen, die während der Anwendungsverarbeitung auftreten. Informationen zum Aktivieren der Azure-Diagnose finden Sie unter [Aktivieren der Diagnose in Azure](cloud-services-dotnet-diagnostics.md).

Sie müssen das [Dienstpaket erstellen](cloud-services-model-and-package.md#servicepackagecspkg), um einen Clouddienst mit Bereitstellungen von Webrollen oder Workerrollen zu erstellen.

## <a name="before-you-begin"></a>Voraussetzungen
* Falls Sie das Azure SDK noch nicht installiert haben, klicken Sie auf **Install Azure SDK** (Azure SDK installieren), um die [Azure-Downloadseite](https://azure.microsoft.com/downloads/) zu öffnen. Laden Sie dann das SDK für die Sprache herunter, in der Sie den Code entwickeln möchten. (Dazu haben Sie auch später noch die Möglichkeit.)
* Falls Rolleninstanzen ein Zertifikat erfordern, erstellen Sie die Zertifikate. Clouddienste erfordern eine PFX-Datei mit einem privaten Schlüssel. Sie können die [Zertifikate zu Azure hochladen](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , wenn Sie den Clouddienst erstellen und bereitstellen.
* Wenn Sie den Clouddienst für eine Affinitätsgruppe bereitstellen möchten, erstellen Sie die Affinitätsgruppe. Sie können eine Affinitätsgruppe verwenden, um den Clouddienst und andere Azure-Dienste für den gleichen Standort in einer Region bereitzustellen. Sie können die Affinitätsgruppe im Bereich **Netzwerke** des klassischen Azure-Portals auf der Seite **Affinitätsgruppen** erstellen.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Erstellen eines Clouddiensts mithilfe der Schnellerfassung
1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/) auf **Neu**>**Compute**>**Clouddienst**>**Schnellerfassung**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. Geben Sie unter **URL**einen Unterdomänennamen ein, der in der öffentlichen URL für den Zugriff auf Ihren Clouddienst in Produktionsbereitstellungen verwendet werden soll. Das URL-Format für Produktionsbereitstellungen lautet „http://*myURL*.cloudapp.net“.
3. Wählen Sie unter **Region oder Affinitätsgruppe**die geografische Region oder die Affinitätsgruppe aus, für die der Clouddienst bereitgestellt werden soll. Wählen Sie eine Affinitätsgruppe aus, wenn Sie den Clouddienst für den gleichen Standort wie andere Azure-Dienste innerhalb einer Region bereitstellen möchten.
4. Klicken Sie auf **Clouddienst erstellen**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Sie können den Status des Prozesses im Nachrichtenbereich unten im Fenster überwachen.
   
    Der Bereich **Clouddienste** wird geöffnet und der neue Clouddienst wird angezeigt. Wenn der Status auf "Erstellt" geändert wird, wurde die Erstellung des Clouddiensts erfolgreich abgeschlossen.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Hochladen eines Zertifikats für einen Clouddienst
1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/) auf **Cloud Services**, auf den Namen des Clouddiensts und dann auf **Zertifikate**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Klicken Sie auf **Zertifikat hochladen** oder auf **Hochladen**.
3. Verwenden Sie unter **Datei** die Option **Durchsuchen**, um das zu verwendende Zertifikat (PFX-Datei) auszuwählen.
4. Geben Sie in **Kennwort**den privaten Schlüssel für das Zertifikat ein.
5. Klicken Sie auf **OK** (Häkchen).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Sie können des Fortschritt des Hochladens im unten angezeigten Nachrichtenbereich überwachen. Wenn das Hochladen abgeschlossen ist, wird das Zertifikat zur Tabelle hinzugefügt. Klicken Sie im Nachrichtenbereich auf "OK", um die Nachricht zu schließen.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Bereitstellen eines Clouddiensts
1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/) auf **Cloud Services**, auf den Namen des Clouddiensts und dann auf **Dashboard**.
2. Klicken Sie auf **Laden Sie eine neue Produktionsbereitstellung hoch.** oder auf **Hochladen**.
3. Geben Sie unter **Bereitstellungsbezeichnung**einen Namen für die neue Bereitstellung ein, zum Beispiel "MeinClouddienstv4".
4. Verwenden Sie unter **Paket** die Option **Durchsuchen**, um die zu verwendende Dienstpaketdatei (CSPKG-Datei) auszuwählen.
5. Verwenden Sie unter **Konfiguration** die Option **Durchsuchen**, um die zu verwendende Dienstkonfigurationsdatei (CSCFG-Datei) auszuwählen.
6. Falls der Clouddienst Rollen mit nur einer Instanz umfasst, aktivieren Sie das Kontrollkästchen mit der Bezeichnung wie **Auch bereitstellen, wenn eine oder mehrere Rollen eine einzelne Instanz enthalten** , um das Fortsetzen der Bereitstellung zu ermöglichen.
   
    Azure kann nur dann 99,95 % Zugriff auf den Clouddienst während Wartungen und Dienstaktualisierungen garantieren, wenn jede Rolle über mindestens zwei Instanzen verfügt. Bei Bedarf können Sie zusätzliche Rolleninstanzen auf der Seite **Skalieren** nach der Bereitstellung des Clouddiensts hinzufügen. Weitere Informationen finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).
7. Klicken Sie auf **OK** (Häkchen), um die Clouddienstbereitstellung zu starten.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Sie können den Status der Bereitstellung im Nachrichtenbereich überwachen. Klicken Sie auf "OK", um die Nachricht zu schließen.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Überprüfen, ob Ihre Bereitstellung erfolgreich abgeschlossen wurde
1. Klicken Sie auf **Dashboard**.
   
    Der Status sollte anzeigen, dass der Dienst **Ausgeführt**wird.
2. Klicken Sie unter **Schnellansicht**auf die Website-URL, um den Clouddienst in einem Webbrowser zu öffnen.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Nächste Schritte
* [Allgemeine Konfiguration Ihres Clouddiensts](cloud-services-how-to-configure.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name.md)
* [Verwalten Ihres Clouddiensts](cloud-services-how-to-manage.md)
* Konfigurieren von [SSL-Zertifikaten](cloud-services-configure-ssl-certificate.md)


