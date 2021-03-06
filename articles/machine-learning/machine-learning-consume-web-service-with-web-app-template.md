---
title: Verwenden eines Machine Learning-Webdiensts mit einer Web-App-Vorlage | Microsoft Docs
description: Verwenden Sie eine Web-App-Vorlage in Azure Marketplace, um einen Vorhersagewebdienst in Azure Machine Learning zu nutzen.
keywords: Webdienst, Operationalisierung, REST-API, Machine Learning
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 95aa1fa23d83ec0dcd00870179167e803bafbd16
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Verwenden eines Azure Machine Learning-Webdiensts mit einer Web-App-Vorlage

Nachdem Sie Ihr Vorhersagemodell entwickelt und mit Machine Learning Studio (oder Tools wie R oder Python) als Azure-Webdienst bereitgestellt haben, können Sie auf das operationalisierte Modell mit einer REST-API zugreifen.

Es gibt eine Reihe von Möglichkeiten, die REST-API zu nutzen und auf den Webdienst zuzugreifen. Beispielsweise können Sie eine Anwendung in C#, R oder Python schreiben und dabei den Beispielcode verwenden, der für Sie beim Bereitstellen des Webdiensts generiert wurde (verfügbar im [Machine Learning Web Services-Portal](https://services.azureml.net/quickstart) oder im Webdienstdashboard in Machine Learning Studio). Sie können auch die Microsoft Excel-Beispielarbeitsmappe verwenden, die gleichzeitig für Sie erstellt wurde.

Aber die schnellste und einfachste Möglichkeit, auf Ihren Webdienst zuzugreifen, bieten die Web-App-Vorlagen, die im [Azure Marketplace für Web-Apps](https://azure.microsoft.com/marketplace/web-applications/all/)verfügbar sind.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure Machine Learning-Web-App-Vorlagen
Die Web-App-Vorlagen, die auf dem Azure Marketplace verfügbar sind, können eine benutzerdefinierte Web-App erstellen, der die Eingabedaten und die erwarteten Ergebnisse des Webdiensts bekannt sind. Dafür müssen Sie nur der Web-App Zugriff auf den Webdienst und die Daten gewähren, und die Vorlage übernimmt den Rest.

Zwei Vorlagen sind verfügbar:

* [Azure ML-Web-App-Vorlage für den Anfrage-/Antwort-Dienst](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML-Web-App-Vorlage für den Batchausführungsdienst](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Jede Vorlage erstellt mit dem API-URI und dem Schlüssel für den Webdienst eine ASP.NET-Beispielanwendung und stellt sie als Website in Azure bereit. Die Vorlage für den Anfrage-/Antwort-Dienst (Request-Response Service, RRS) erstellt eine Web-App, die es Ihnen ermöglicht, eine einzelne Zeile mit Daten an den Webdienst zu senden, um ein einzelnes Ergebnis zu erhalten. Die Vorlage für den Batchausführungsdienst (Batch Execution Service, BES) erstellt eine Web-App, die es Ihnen ermöglicht, viele Zeilen mit Daten zu senden, um mehrere Ergebnisse zu erhalten.

Es ist keine Codierung erforderlich, um diese Vorlagen zu verwenden. Sie geben nur den API-Schlüssel und den URI an, und die Vorlage erstellt die Anwendung für Sie.

So rufen Sie den API-Schlüssel und den Anforderungs-URI für einen Webdienst ab

1. Klicken Sie im [Web Services-Portal](https://services.azureml.net/quickstart) für einen neuen Webdienst oben auf **Web Services**. Oder klicken Sie für einen klassischen Webdienst auf **Classic Web Services**.
2. Klicken Sie auf den Webdienst, auf den Sie zugreifen möchten.
3. Klicken Sie für einen klassischen Webdienst auf den Endpunkt, auf den Sie zugreifen möchten.
4. Klicken Sie oben auf **Consume** (Nutzen).
5. Kopieren Sie den **primären** oder den **sekundären Schlüssel**, und speichern Sie ihn.
6. Wenn Sie eine Vorlage für den Anforderung/Antwort-Dienst (Request-Response Service, RRS) erstellen, kopieren Sie den URI für **Request-Response**, und speichern Sie ihn. Wenn Sie eine Vorlage für den Batchausführungsdienst (Batch Execution Service, BES) erstellen, kopieren Sie den URI für **Batch Requests**, und speichern Sie ihn.


## <a name="how-to-use-the-request-response-service-rrs-template"></a>Verwenden der Vorlage für den Anfrage-/Antwort-Dienst (RRS)
Führen Sie die folgenden Schritte aus, um die RRS-Web-App-Vorlage zu verwenden (siehe auch die folgende Abbildung).

![Prozess zur Verwendung einer RRS-Webvorlage][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), **melden Sie sich an**, und klicken Sie auf **Neu**. Navigieren Sie zu **Azure ML Request-Response Service Web App**, wählen Sie die App aus, und klicken Sie auf **Erstellen**. 
   
   * Geben Sie Ihrer Web-App einen eindeutigen Namen. Die URL der Web-App besteht aus diesem Namen gefolgt von `.azurewebsites.net.`. Beispiel: `http://carprediction.azurewebsites.net.`
   * Wählen Sie das Azure-Abonnement und die Dienste aus, mit denen der Webdienst ausgeführt wird.
   * Klicken Sie auf **Erstellen**.
     
     ![Web-App erstellen][image5]

4. Wenn Azure das Bereitstellen der Web-App beendet hat, klicken Sie auf die **URL** auf der Seite mit den Web-App-Einstellungen in Azure, oder geben Sie die URL in einem Webbrowser ein. Beispiel: `http://carprediction.azurewebsites.net.`
5. Bei der ersten Ausführung der Web-App werden Sie aufgefordert, die **Post-URL der API** und den **API-Schlüssel** anzugeben.
   Geben Sie die Werte ein, die Sie zuvor gespeichert haben (**Anforderungs-URI** bzw. **API-Schlüssel**).
     
     Klicken Sie auf **Senden**.
     
     ![Post-URI und API-Schlüssel eingeben][image6]

6. Die Web-App zeigt die Seite **Web-App-Konfiguration** mit den aktuellen Einstellungen für den Webdienst an. Hier können Sie die von der Web-App verwendeten Einstellungen ändern.
   
   > [!NOTE]
   > Wenn Sie hier Einstellungen ändern, werden sie nur für diese Web-App geändert. Die Standardeinstellungen des Webdiensts werden nicht geändert. Angenommen, Sie ändern hier die **Beschreibung**. In diesem Fall wird die im Webdienstdashboard in Machine Learning Studio angezeigte Beschreibung nicht geändert.
   > 
   > 
   
    Wenn Sie fertig sind, klicken Sie auf **Änderungen speichern** und dann auf **Zur Startseite wechseln**.

7. Auf der Startseite können Sie Werte eingeben, die an den Webdienst gesendet werden. Klicken Sie abschließend auf **Senden**. Das Ergebnis wird dann zurückgegeben.

Wenn Sie zur Seite **Konfiguration** zurückkehren möchten, wechseln Sie zur Seite `setting.aspx` der Web-App. Beispiel: `http://carprediction.azurewebsites.net/setting.aspx.` Sie werden aufgefordert, den API-Schlüssel erneut einzugeben. Sie benötigen ihn, um auf die Seite zuzugreifen und die Einstellungen zu aktualisieren.

Sie können die Web-App im Azure-Portal wie andere Web-Apps beenden, neu starten oder löschen. Solange sie ausgeführt wird, können Sie zur Webadresse der Startseite wechseln und neue Werte eingeben.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Verwenden der Vorlage für den Batchausführungsdienst (BES)
Sie können die BES-Web-App-Vorlage auf die gleiche Weise wie die RRS-Vorlage verwenden. Der Unterschied ist, dass die erstellte Web-App es ermöglicht, mehrere Datenzeilen zu senden und mehrere Ergebnisse zu empfangen.

Die Eingabewerte für einen Webdienst mit Batchausführung können aus Azure Storage oder einer lokalen Datei stammen. Die Ergebnisse werden in einem Azure Storage-Container gespeichert.
Daher benötigen Sie einen Azure-Speichercontainer für die Ergebnisse, die von der Web-App zurückgegeben werden, und Sie müssen Ihre Eingabedaten vorbereiten.

![Prozess zur Verwendung einer BES-Webvorlage][image2]

1. Führen Sie zum Erstellen der BES-Web-App die gleichen Schritte wie für die RRS-Vorlage aus, mit einer Ausnahme: Wechseln Sie zu [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/), um die BES-Vorlage im Azure Marketplace zu öffnen, und klicken Sie auf **Web-App erstellen**.

2. Um anzugeben, wo die Ergebnisse gespeichert werden sollen, geben Sie die Informationen zum Zielcontainer auf der Startseite der Web-App an. Geben Sie zudem an, woher die Web-App die Eingabewerte erhält: aus einer lokalen Datei oder einem Azure-Speichercontainer.
   Klicken Sie auf **Senden**.
   
    ![Speicherinformationen][image7]

Die Web-App zeigt eine Seite mit dem Auftragsstatus an.
Nach Abschluss des Auftrags werden Sie über den Speicherort der Ergebnisse im Azure-Blobspeicher informiert. Sie haben auch die Möglichkeit, die Ergebnisse in eine lokale Datei herunterzuladen.

## <a name="for-more-information"></a>Weitere Informationen
Erfahren Sie mehr:

* Weitere Informationen zum Erstellen eines Machine Learning-Experiments mit Machine Learning Studio finden Sie unter [Erstellen Ihres ersten Experiments im Azure Machine Learning Studio](machine-learning-create-experiment.md)
* Informationen zum Bereitstellen Ihres Machine Learning-Experiments als Webdienst finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](machine-learning-publish-a-machine-learning-web-service.md)
* Andere Möglichkeiten für den Zugriff auf den Webdienst finden Sie unter [Nutzen eines Azure Machine Learning-Webdiensts](machine-learning-consume-web-services.md)

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png

