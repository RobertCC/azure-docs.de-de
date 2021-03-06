---
title: "Überwachen eines Azure Storage-Kontos | Microsoft-Dokumentation"
description: "Erfahren Sie, wie ein Speicherkonto in Azure mithilfe des Azure-Portals überwacht wird."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: e8fbc4ecdffe62806019f494e1412cfedbccf71f
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Überwachen eines Speicherkontos im Azure-Portal

[Azure-Speicheranalyse](../storage-analytics.md) enthält Metriken für alle Speicherdienste und Protokolle für Blobs, Warteschlangen und Tabellen. Sie können im [Azure-Portal](https://portal.azure.com) konfigurieren, welche Metriken und Protokolle für Ihr Konto aufgezeichnet werden, und Diagramme konfigurieren, die visuelle Darstellungen der Metrikdaten bereitstellen.

> [!NOTE]
> Für die Untersuchung von Überwachungsdaten im Azure-Portal fallen Kosten an. Weitere Informationen finden Sie unter [Speicheranalyse und Speicheranalysekosten](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Der Azure-Dateispeicher unterstützt derzeit Metriken der Speicheranalyse, allerdings noch keine Protokollierung.
>
> Für Speicherkonten mit dem Replikationstyp „Zonenredundanter Speicher (ZRS)“ ist die Metrik- oder Protokollierungsfunktion derzeit nicht aktiviert.
> 
> Eine ausführliche Anleitung zum Verwenden der Speicheranalyse sowie weiterer Tools, um Azure Storage-bezogene Probleme zu identifizieren, zu diagnostizieren und zu beheben, finden Sie unter [Microsoft Azure Storage: Überwachung, Diagnose und Problembehandlung](../storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Konfigurieren der Überwachung für ein Speicherkonto

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) **Speicher** und dann den Speicherkontonamen, um das Kontodashboard zu öffnen.
1. Wählen Sie im Abschnitt **ÜBERWACHUNG** des Menüblatts die Option **Diagnose** aus.

    ![Überwachungsoptionen](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Wählen Sie den **Typ** der Metrikdaten für jeden **Dienst**, den Sie überwachen möchten, und die **Aufbewahrungsrichtlinie** für die Daten. Sie können die Überwachung auch deaktivieren, indem Sie für **Status** **Aus** festlegen.

    ![Überwachungsoptionen](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   Es gibt zwei Arten von standardmäßig für neue Speicherkonten aktivierten Metriken, die Sie für jeden Dienst aktivieren können:

   * **Aggregieren**: Erfasst Metriken wie Eingang/Ausgang, Verfügbarkeit, Latenz sowie Erfolgsprozentwerte. Diese Metriken werden für die Blob-, Warteschlangen-, Tabellen- und Dateidienste aggregiert.
   * **Pro API**: Sammelt zusätzlich zu den Aggregatmetriken den gleichen Satz Metriken für jeden Speichervorgang in der Azure Storage-Dienst-API.

   Zum Festlegen der Datenaufbewahrungsrichtlinie verschieben Sie den Schieberegler **Aufbewahrung (Tage)**, oder geben Sie die Anzahl der Tage ein, für die Daten aufbewahrt werden sollen (zwischen 1 und 365). Die Standardeinstellung für neue Speicherkonten beträgt sieben Tage. Wenn Sie keine Aufbewahrungsrichtlinie festlegen möchten, geben Sie null (0) ein. Ist keine Aufbewahrungsrichtlinie festgelegt, müssen Sie die Überwachungsdaten selbst löschen.

   > [!WARNING]
   > Ihnen werden Gebühren berechnet, wenn Sie Metrikdaten manuell löschen. Veraltete Analysedaten (Daten, die älter sind als die Aufbewahrungsrichtlinie) werden vom System kostenlos gelöscht. Wir empfehlen, die Aufbewahrungsrichtlinie entsprechend dem Zeitraum festzulegen, den Sie Speicheranalysedaten für Ihr Konto behalten möchten. Weitere Informationen finden Sie unter [Welche Kosten fallen an, wenn Speichermetriken aktiviert werden?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics).
   >

1. Wählen Sie nach Abschluss der Überwachungskonfiguration **Speichern**.

Ein Standardsatz von Metriken wird in Diagrammen auf dem Speicherkontoblatt angezeigt, wie auch die Blätter der einzelnen Dienste (Blob, Warteschlange, Tabelle und Datei). Nachdem Sie die Metriken für einen Dienst aktiviert haben, kann es bis zu einer Stunde dauern, bis Daten in den Diagrammen angezeigt werden. Sie können **Bearbeiten** auf jedem beliebigen Metrikdiagramm auswählen, um zu [konfigurieren, welche Metriken](#how-to-customize-metrics-charts) im Diagramm angezeigt werden.

Sie können die Sammlung von Metriken und die Protokollierung deaktivieren, indem Sie **Status** auf **Aus** setzen.

> [!NOTE]
> Azure Storage verwendet [Tabellenspeicher](../common/storage-introduction.md#table-storage), um die Metriken für Ihr Speicherkonto zu speichern, und speichert die Metriken in Tabellen in Ihrem Konto. Weitere Informationen finden Sie unter: [Speichern von Metriken](../common/storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Anpassen von Metrikendiagrammen

Verwenden Sie das folgende Verfahren, um auszuwählen, welche Speichermetriken in einem Metrikdiagramm angezeigt werden. 

1. Beginnen Sie mit der Anzeige eines Speichermetrikdiagramms im Azure-Portal. Diagramme finden Sie auf dem **Speicherkontoblatt** und dem Blatt **Metriken** eines individuellen Diensts (Blob, Warteschlange, Tabelle, Datei).

   In diesem Beispiel arbeiten wir mit dem folgenden, auf dem **Speicherkontoblatt** angezeigten Diagramm:

   ![Diagrammauswahl im Azure-Portal](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Als Nächstes klicken Sie auf eine beliebige Stelle im Diagramm, um das Blatt **Metrik** zu öffnen. Wählen Sie **Diagramm bearbeiten**, um das Blatt **Diagramm bearbeiten** zu öffnen.

   ![Schaltfläche „Diagramm bearbeiten“ auf dem Diagrammblatt](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. Wählen Sie auf dem Blatt **Diagramm bearbeiten** den **Zeitraum** der Metriken aus, die im Diagramm angezeigt werden sollen, und den **Dienst** (Blob, Warteschlange, Tabelle, Datei), dessen Metriken angezeigt werden sollen. Hier haben wir die Metriken für den Blobdienst der vergangenen Woche zur Anzeige ausgewählt:

   ![Auswahl von Zeitraum und Dienst auf dem Blatt „Diagramm bearbeiten“](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Wählen Sie die einzelnen **Metriken** aus, die im Diagramm angezeigt werden sollen, und klicken Sie dann auf **OK**. Beispielsweise haben wir hier die Metriken *ContainerCount* und *ObjectCount* zur Anzeige ausgewählt:

   ![Individuelle Auswahl von Metriken auf dem Blatt „Diagramm bearbeiten“](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

Ihre Diagrammeinstellungen haben keine Auswirkung auf Sammlung, Aggregierung oder Speicherung von Überwachungsdaten im Speicherkonto, nur auf die Anzeige der Metrikdaten.

### <a name="metrics-availability-in-charts"></a>Verfügbarkeit von Metriken in Diagrammen

Die Liste der verfügbaren Metriken ändert sich je nach dem Dienst, den Sie in der Dropdownliste ausgewählt haben, und dem Einheitentyp des Diagramms, das Sie gerade bearbeiten. Sie können z.B. prozentuale Metriken wie *PercentNetworkError* und *PercentThrottlingError* nur dann auswählen, wenn Sie ein Diagramm bearbeiten, das Einheiten in Prozent angezeigt:

![Fehlerprozentsatz-Diagramm für Anforderung im Azure-Portal](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Auflösung von Metriken

Die Metriken, die Sie in „Diagnose“ ausgewählt haben, bestimmen die Auflösung der Metriken, die für Ihr Konto verfügbar sind:

* **Aggregieren**-Überwachung erfasst Metriken wie Eingang/Ausgang, Verfügbarkeit, Latenz sowie Erfolgsprozentwerte. Diese Metriken werden aus Blob-, Tabellen-, Datei- und Warteschlangendienst aggregiert.
* **Pro API** bietet eine feinere Auflösung mit Metriken, die für einzelne Speicheroperationen verfügbar sind, in Ergänzung der Aggregate auf Dienstebene.

## <a name="configure-metrics-alerts"></a>Konfigurieren von Metrikenwarnungen

Sie können Warnungen erstellen, damit Sie benachrichtigt werden, wenn Speicherressourcenmetriken Schwellenwerte erreichen.

1. Um das **Warnungsregelnblatt** zu öffnen, scrollen Sie zum Abschnitt **ÜBERWACHUNG** des **Menüblatts** nach unten, und wählen Sie **Warnungsregeln**.
1. Wählen Sie **Warnung hinzufügen**, um das Blatt **Warnungsregel hinzufügen** zu öffnen.
1. Wählen Sie eine **Ressource** (Blob, Datei, Warteschlange, Tabelle) in der Dropdownliste aus, und geben Sie **Namen** und **Beschreibung** für Ihre neue Warnungsregel ein.
1. Wählen Sie die **Metrik**, für die Sie eine Warnung hinzufügen möchten, eine **Bedingung** für die Warnung und einen **Schwellenwert**. Der Schwellenwerteinheiten-Typ ändert sich je nach der Metrik, die Sie ausgewählt haben. Beispielsweise ist „count“ der Einheitentyp für *ContainerCount*, die Einheit für die Metrik *PercentNetworkError* ist dagegen ein Prozentsatz.
1. Wählen Sie den **Zeitraum** aus. Metriken, die den Schwellenwert innerhalb des Zeitraums erreichen oder überschreiten, lösen eine Warnung aus.
1. (Optional) Konfigurieren Sie **E-Mail**- und **Webhook**-Benachrichtigungen. Weitere Informationen über Webhooks finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../../monitoring-and-diagnostics/insights-webhooks-alerts.md). Wenn Sie keine E-Mail- oder Webhook-Benachrichtigungen konfigurieren, werden Warnungen nur im Azure-Portal angezeigt.

![Blatt „Warnungsregel hinzufügen“ im Azure-Portal](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a>Hinzufügen von Metrikdiagrammen zum Portaldashboard

Sie können Ihrem Portaldashboard Azure Storage-Metrikdiagramme für beliebige Ihrer Speicherkonten hinzufügen.

1. Wählen Sie **Dashboard bearbeiten**, während Ihr Dashboard im [Azure-Portal](https://portal.azure.com) angezeigt wird.
1. Wählen Sie im **Kachelkatalog** die Option **Kacheln suchen nach:** > **Typ**.
1. Wählen Sie **Typ** > **Speicherkonten**.
1. Wählen Sie in **Ressourcen** das Speicherkonto aus, dessen Metriken Sie dem Dashboard hinzufügen möchten.
1. Wählen Sie **Kategorien** > **Überwachung**.
1. Ziehen Sie die Diagrammkachel für die Metrik, die Sie anzeigen möchten, auf Ihr Dashboard, und legen Sie sie ab. Wiederholen Sie dies für alle Metriken, die Sie auf dem Dashboard anzeigen möchten. In der folgenden Abbildung ist das Diagramm „Blobs – Anforderungen insgesamt“ als Beispiel hervorgehoben, aber alle Diagramme sind für die Platzierung auf Ihrem Dashboard verfügbar.

   ![Kachelkatalog im Azure-Portal](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Wählen Sie **Anpassung abgeschlossen** am oberen Rand des Dashboards aus, wenn Sie keine weiteren Diagramme hinzufügen möchten.

Nachdem Sie Ihrem Dashboard Diagramme hinzugefügt haben, können Sie sie weiter anpassen, wie in [Anpassen von Metrikdiagrammen](#how-to-customize-metrics-charts) beschrieben.

## <a name="configure-logging"></a>Konfigurieren der Protokollierung

Sie können Azure Storage anweisen, Diagnoseprotokolle für Lese-, Schreib- und Löschanforderungen für die Blob-, Tabellen- und Warteschlangendienste zu speichern. Die Datenaufbewahrungsrichtlinie, die Sie festlegen, gilt auch für diese Protokolle.

> [!NOTE]
> Der Azure-Dateispeicher unterstützt derzeit Metriken der Speicheranalyse, allerdings noch keine Protokollierung.
>

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) **Speicherkonten** und dann den Speicherkontonamen, um das Speicherkontoblatt zu öffnen.
1. Wählen Sie im Abschnitt **ÜBERWACHUNG** des Menüblatts die Option **Diagnose** aus.

    ![Element des Menüs „Diagnose“ unter ÜBERWACHUNG im Azure-Portal.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Stellen Sie sicher, dass **Status** auf **Ein** festgelegt ist, und wählen Sie die **Dienste**, für die Sie die Protokollierung aktivieren möchten.

    ![Konfigurieren Sie die Protokollierung im Azure-Portal.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. Klicken Sie auf **Speichern**.

Die Diagnoseprotokolle werden in einem Blob-Container namens $logs in Ihren Speicherkonto gespeichert. Sie können die Protokolldaten über einen Speicher-Explorer wie den [Microsoft Speicher-Explorer](http://storageexplorer.com) oder programmgesteuert mit der Speicherclientbibliothek oder PowerShell anzeigen.

Informationen zum Zugreifen auf den Container $logs finden Sie unter [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data) (Aktivieren der Speicherprotokolierung und Zugreifen auf Protokolldaten).

## <a name="next-steps"></a>Nächste Schritte

* Hier finden Sie weitere Informationen zu [Metriken, Protokollierung und Abrechnung](../storage-analytics.md) für die Speicheranalyse.
* [Aktivieren der Azure-Speichermetriken und Anzeigen von Metrikdaten](../storage-enable-and-view-metrics.md) mithilfe von PowerShell und programmgesteuert mit C#.

