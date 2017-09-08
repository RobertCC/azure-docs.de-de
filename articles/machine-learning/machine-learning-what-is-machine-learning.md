---
title: Was ist Machine Learning Studio in Azure? | Microsoft Docs
description: "Es werden grundlegende Machine Learning-Konzepte für die Cloud und die Nutzungsmöglichkeiten beschrieben, und Sie erhalten Definitionen von Machine Learning-Begriffen."
keywords: Was ist Machine Learning,Machine Learning-Begriffe,Vorhersage,Was ist Predictive Analytics
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: eaee083e-eaa1-4408-838b-93e51423d159
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 179a0d3696c6044ffb5b9e377effa30dda54ba7f
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="introduction-to-machine-learning-in-the-azure-cloud"></a>Einführung in Machine Learning in der Azure-Cloud

## <a name="what-is-machine-learning"></a>Was ist Machine Learning?
Machine Learning ist ein Data Science-Verfahren, mit dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen. Mithilfe von Machine Learning lernen Computer, ohne explizit programmiert worden zu sein. 

Machine Learning gilt als Unterkategorie von künstlicher Intelligenz (KI). Dank solcher Vorhersagen oder Prognosen aus Machine Learning können Apps und Geräte „intelligenter“ werden. Wenn Sie online einkaufen, trägt maschinelles Lernen dazu bei, dass Ihnen anhand der gekauften Produkte weitere Produkte empfohlen werden, die Ihnen gefallen könnten. Wenn Ihre Kreditkarte verwendet wird, vergleichen Machine Learning-Prozesse die Transaktion mit einer Transaktionsdatenbank und helfen bei der Betrugserkennung. Wenn ein automatischer Staubsauger ein Zimmer saugt, wird anhand von Machine Learning-Prozessen entschieden, ob die Arbeit erledigt ist.

Eine kurze Übersicht finden Sie in der Videoserie [Data Science für Einsteiger](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Ohne störende Fachsprache oder Mathematik erhalten Sie in „Data Science für Einsteiger“ eine Einführung in Machine Learning, und es wird ein einfaches Vorhersagemodell beschrieben.

## <a name="what-is-machine-learning-in-the-microsoft-azure-cloud"></a>Was ist Machine Learning in der Microsoft Azure-Cloud?

![Was ist Machine Learning? Ein grundlegender Workflow zum Operationalisieren von Predictive Analytics im Rahmen von Azure Machine Learning.](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Azure Machine Learning ist ein Predictive Analytics-Clouddienst, der die schnelle Erstellung und Bereitstellung von Vorhersagemodellen als Analyselösungen ermöglicht.

Sie können mit einer fertigen Bibliothek mit Algorithmen arbeiten, sie zum Erstellen von Modellen auf einem PC mit Internetverbindung verwenden und Ihre Vorhersagelösung schnell bereitstellen. Beginnen Sie mit fertigen Beispielen und Lösungen in der [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).

Azure Machine Learning bietet nicht nur Tools zur Modellierung von Predictive Analytics-Lösungen, sondern auch einen vollständig verwalteten Dienst, über den Sie Ihre Vorhersagemodelle als sofort nutzbare Webdienste bereitstellen können.

## <a name="what-is-predictive-analytics"></a>Was ist Predictive Analytics?
Für Predictive Analytics werden mathematische Formeln verwendet, die als Algorithmen bezeichnet werden. Hiermit werden Verlaufsdaten oder aktuelle Daten analysiert, um Muster oder Trends zu ermitteln und zukünftige Ereignisse vorhersagen zu können.

## <a name="tools-to-build-complete-machine-learning-solutions-in-the-cloud"></a>Tool zum Entwickeln von vollständigen Lösungen für Machine Learning in der Cloud
Azure Machine Learning bietet alles, was Sie zum Erstellen von vollständigen Predictive Analytics-Lösungen in der Cloud benötigen: eine große Algorithmusbibliothek, eine Studio-Komponente zum Entwickeln von Modellen sowie eine einfache Möglichkeit, Ihr Modell als Webdienst bereitzustellen. Sie können Vorhersagemodelle schnell erstellen, testen, operationalisieren und verwalten.

### <a name="machine-learning-studio-create-predictive-models"></a>Machine Learning Studio: Erstellen von Vorhersagemodellen
In [Machine Learning Studio](machine-learning-what-is-ml-studio.md)können Sie in kurzer Zeit Vorhersagemodelle erstellen, indem Sie Module per Drag &amp; Drop platzieren und miteinander verbinden. Sie können mit unterschiedlichen Kombinationen experimentieren und dies [kostenlos ausprobieren](https://studio.azureml.net/?selectAccess=true&o=2).

* Im [Cortana Intelligence-Katalog](machine-learning-gallery-how-to-use-contribute-publish.md)können Sie Lösungen für die Analyse testen, die von anderen Benutzern erstellt wurden, oder eigene Lösungen bereitstellen. Über soziale Netzwerke wie LinkedIn und Twitter können Sie Fragen oder Kommentare zu Experimenten an die Community senden oder Links zu Experimenten freigeben.

  ![Probieren Sie Vorhersageexperimente aus, oder ergänzen Sie den Azure Cortana Intelligence-Katalog durch eigene Experimente.](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)
* Nutzen Sie die umfangreiche Bibliothek mit [Machine Learning-Algorithmen und -Modulen](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) in Machine Learning Studio, um sofort mit dem Entwickeln von Vorhersagemodellen zu beginnen. Wählen Sie zwischen Beispielexperimenten, R- und Python-Paketen sowie erstklassigen Algorithmen aus Microsoft-Produktbereichen wie Xbox und Bing. Erweitern Sie Studio-Module mit Ihren eigenen benutzerdefinierten [R](machine-learning-extend-your-experiment-with-r.md)- und [Python](machine-learning-execute-python-scripts.md)-Skripts.

  ![Was ist Predictive Analytics? Beispiel eines Predictive Analytics-Experiments in Azure Machine Learning Studio](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

### <a name="operationalize-predictive-analytics-solutions-by-publishing-your-own"></a>Operationalisieren von Predictive Analytics-Lösungen durch das Veröffentlichen eigener Dienste
In den folgenden Tutorials wird gezeigt, wie Sie Ihre Predictive Analytics-Modelle operationalisieren:

 * [Bereitstellen von Webdiensten](machine-learning-publish-a-machine-learning-web-service.md)
 * [Erneutes Trainieren von Modellen über APIs](machine-learning-retrain-models-programmatically.md)
 * [Verwalten von Webdienstendpunkten](machine-learning-create-endpoint.md)
 * [Skalieren eines Webdiensts](machine-learning-scaling-webservice.md)
 * [Nutzen von Webdiensten](machine-learning-consume-web-services.md)

## <a name="key-machine-learning-terms-and-concepts"></a>Wichtige Machine Learning-Begriffe und -Konzepte
Machine Learning-Begriffe können manchmal verwirrend sein. Hier finden Sie deshalb die Definitionen der wichtigsten Begriffe. Sie können auch gern die Kommentarfunktion nutzen, um uns andere Begriffe mitzuteilen, für die Sie eine Definition wünschen.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Durchsuchen von Daten, beschreibende Analyse und Predictive Analytics

**Durchsuchen von Daten** werden Informationen über ein umfangreiches und häufig unstrukturiertes Dataset erfasst, um Merkmale für eine gezielte Analyse zu ermitteln.

**Data Mining** bezieht sich auf das automatisierte Durchsuchen von Daten.

Bei der **beschreibenden Analyse** wird ein Dataset analysiert, um Vorgänge zusammenzufassen. Bei den weitaus meisten Business Analytics-Prozessen – z. B. Verkaufsberichten, Webmetriken und Analysen sozialer Netzwerke – handelt es sich um beschreibende Analysen.

**Predictive Analytics** werden Modelle basierend auf vergangenen oder aktuellen Daten entwickelt, um zukünftige Ergebnisse vorhersagen zu können.

### <a name="supervised-and-unsupervised-learning"></a>Überwachtes und nicht überwachtes Lernen
 **überwachte Lernen** werden mit bezeichneten Daten trainiert, also mit Daten, die aus Beispielen der gewünschten Antworten bestehen. Ein Modell zur Identifizierung von betrügerischer Kreditkartennutzung wird beispielsweise mit einem Dataset trainiert, das beschriftete Datenpunkte für bekannte betrügerische und gültige Buchungen enthält. Die meisten maschinellen Lernprozesse sind überwacht.

 **Nicht überwachte Lernprozesse** werden bei Daten ohne Bezeichnungen verwendet. Das Ziel hierbei ist es, Beziehungen zwischen den Daten zu finden. Ein Beispiel hierfür ist das Ermitteln von Kundengruppen mit ähnlichem Kaufverhalten.

### <a name="model-training-and-evaluation"></a>Trainieren und Auswerten des Modells
Ein Modell zum maschinellen Lernen ist eine Abstraktion der Frage, die Sie beantworten möchten, oder des Ergebnisses, das Sie vorhersagen möchten. Modelle werden anhand vorhandener Daten trainiert und ausgewertet.

#### <a name="training-data"></a>Trainingsdaten
Wenn Sie ein Modell mit Daten trainieren, verwenden Sie ein bekanntes Dataset und nehmen basierend auf den Datenmerkmalen Anpassungen am Modell vor, um eine möglichst genaue Antwort zu erhalten. In Azure Machine Learning wird ein Modell aus einem Algorithmusmodul entwickelt, das Trainingsdaten und funktionale Module, wie z. B. ein Bewertungsmodul, verarbeitet.

Wenn Sie ein Betrugserkennungsmodell in einem überwachten Lernprozess trainieren, verwenden Sie einen Satz Transaktionen, die entweder als betrügerisch oder als gültig bezeichnet sind. Sie teilen Ihr Dataset nach dem Zufallsprinzip und verwenden einen Teil zum Trainieren und einen Teil zum Testen oder Auswerten des Modells.

#### <a name="evaluation-data"></a>Auswertungsdaten
Nachdem Sie das Modell trainiert haben, werten Sie es mithilfe der verbleibenden Testdaten aus. Hierzu verwenden Sie Daten, deren Ergebnisse Sie bereits kennen, um herauszufinden, ob die Vorhersage Ihres Modells genau ist.

## <a name="other-common-machine-learning-terms"></a>Weitere gängige Begriffe des maschinellen Lernens
* **Algorithmus**: Ein eigenständiger Regelsatz, der zum Lösen von Problemen mithilfe von Datenverarbeitung, mathematischen Berechnungen oder automatisierter Argumentation verwendet wird.
* **Anomalieerkennung**: Ein Modell, bei dem ungewöhnliche Ereignisse oder Werte gemeldet werden, damit Probleme erkannt werden können. Bei der Erkennung von Kreditkartenbetrug wird beispielsweise nach ungewöhnlichen Käufen gesucht.
* **Kategorische Daten**: Daten, die nach Kategorien organisiert sind und in Gruppen unterteilt werden können. Ein kategorisches Dataset für Fahrzeuge könnte z. B. Jahr, Marke, Modell und Preis angeben.
* **Klassifizierung**: Ein Modell für die Einordnung von Datenpunkten in Kategorien, basierend auf einem Dataset, für das die Kategoriegruppierungen bereits bekannt sind.
* **Featureentwicklung**: Der Prozess des Extrahierens oder Auswählens von Features in Zusammenhang mit einem Dataset, um das Dataset zu erweitern und die Ergebnisse zu verbessern. Flugpreisdaten könnten z. B. durch Wochentage und Ferien erweitert werden. Siehe [Entwicklung und Auswahl von Features in Azure Machine Learning](machine-learning-feature-selection-and-engineering.md).
* **Modul**: Ein Funktionselement in einem Machine Learning Studio-Modell, z.B. das Modul zur Dateneingabe, das die Eingabe und Bearbeitung kleiner Datasets ermöglicht. Auch bei einem Algorithmus handelt es sich um eine Art Modul in Machine Learning Studio.
* **Modell**: Ein Modell für überwachtes Lernen ist das Produkt eines Machine Learning-Experiments, das aus Trainingsdaten, einem Algorithmusmodul und Funktionsmodulen besteht, z.B. einem Bewertungsmodellmodul.
* **Numerische Daten**: Daten, die eine Bedeutung als Messung (kontinuierliche Daten) oder Zählung (diskrete Daten) haben. Diese Daten werden auch als *quantitative Daten*bezeichnet.
* **Partitionieren**: Die Methode, mit der Sie Daten in Stichproben unterteilen. Weitere Informationen finden Sie unter [Partition and Sample](https://msdn.microsoft.com/library/azure/dn905960.aspx) .
* **Vorhersage**: Eine Vorhersage ist die Prognose eines oder mehrerer Werte aus einem Machine Learning-Modell. Ihnen wird möglicherweise auch der Begriff „vorhergesagte Bewertung“ begegnen. Hierbei handelt es sich aber nicht um die finalen Ergebnisse eines Modells. Der Bewertung folgt eine Auswertung des Modells.
* **Regression**: Ein Modell zur Vorhersage eines Werts basierend auf unabhängigen Variablen, um z.B. den Preis eines Autos anhand des Baujahrs und der Marke vorherzusagen.
* **Bewertung**: Ein vorhergesagter Wert, der mithilfe des Moduls [Score Model](https://msdn.microsoft.com/library/azure/dn905995.aspx) in Machine Learning Studio aus einem trainierten Klassifizierungs- oder Regressionsmodell generiert wurde. Klassifizierungsmodelle geben auch eine Bewertung für die Wahrscheinlichkeit des vorhergesagten Werts zurück. Sobald Sie Bewertungen aus einem Modell generiert haben, können Sie die Genauigkeit des Modells mithilfe des Moduls [Evaluate Model](https://msdn.microsoft.com/library/azure/dn905915.aspx)auswerten.
* **Stichprobe**: Ein Teil eines Datasets, das repräsentativ für das gesamte Dataset steht. Stichproben können nach dem Zufallsprinzip oder basierend auf bestimmten Features des Datasets ausgewählt werden.

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen von Predictive Analytics und Machine Learning werden anhand eines [schrittweisen Tutorials](machine-learning-create-experiment.md) und auf der [Grundlage von Beispielen](machine-learning-sample-experiments.md) vermittelt.  

<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/

