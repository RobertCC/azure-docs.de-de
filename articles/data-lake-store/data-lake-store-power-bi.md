---
title: Analysieren von Daten im Data Lake Store mithilfe von Power BI | Microsoft Docs
description: Analysieren von im Azure Data Lake-Speicher gespeicherten Daten mithilfe von Power BI
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 59f072c7a8272fc04e1d662c0ab17e7ee4500fa6
ms.openlocfilehash: f965e3a5067dd8dd2ac6dc29446c7f5b50cdd450
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017

---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analysieren von Daten im Data Lake-Speicher mithilfe von Power BI
In diesem Artikel erfahren Sie, wie Sie Power BI Desktop verwenden, um Daten im Azure Data Lake-Speicher zu analysieren und zu visualisieren.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md)aus. In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Data Lake Store-Konto erstellt, **mybidatalakestore** aufgerufen und eine Beispieldatendatei (**Drivers.txt**) geladen haben. Diese Beispieldatei kann aus dem [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)heruntergeladen werden.
* **Power BI Desktop**. Sie können das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331) herunterladen. 

## <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop
1. Starten Sie Power BI Desktop auf Ihrem Computer.
2. Klicken Sie auf dem Menüband **Start** auf **Daten abrufen** und dann auf „Mehr“. Klicken Sie im Dialogfeld **Daten abrufen** auf **Azure**. Klicken Sie auf **Azure Data Lake Store** und dann auf **Verbinden**.
   
    ![Herstellen einer Verbindung mit Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Herstellen einer Verbindung mit Data Lake Store")
3. Wenn ein Dialogfeld anzeigt, dass der Connector sich in der Entwicklungsphase befindet, klicken Sie auf die Option zum Fortfahren.
4. Geben Sie im Dialogfeld **Microsoft Azure Data Lake Store** die URL zu Ihrem Data Lake Store-Konto an, und klicken Sie dann auf **OK**.
   
    ![URL für Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL für Data Lake Store")
5. Klicken Sie im nächsten Dialogfeld auf **Anmelden** , um sich beim Data Lake-Speicherkonto anzumelden. Sie werden zur Anmeldeseite Ihrer Organisation weitergeleitet. Folgen Sie den Anweisungen, um sich beim Konto anzumelden.
   
    ![Anmelden bei Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Anmelden bei Data Lake Store")
6. Wenn Sie sich erfolgreich angemeldet haben, klicken Sie auf **Verbinden**.
   
    ![Herstellen einer Verbindung mit Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Herstellen einer Verbindung mit Data Lake Store")
7. Das nächste Dialogfeld zeigt die Datei, die Sie in Ihr Data Lake-Speicherkonto hochgeladen haben. Überprüfen Sie die Informationen, und klicken Sie dann auf **Laden**.
   
    ![Laden von Daten aus Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Laden von Daten aus Data Lake Store")
8. Nachdem die Daten erfolgreich in Power BI geladen wurden, werden auf der Registerkarte **Felder** folgende Felder angezeigt.
   
    ![Importierte Felder](./media/data-lake-store-power-bi/imported-fields.png "Importierte Felder")
   
    Um die Daten visualisieren und analysieren zu können, sollten sie jedoch gemäß der folgenden Felder verfügbar sein:
   
    ![Gewünschte Felder](./media/data-lake-store-power-bi/desired-fields.png "Gewünschte Felder")
   
    In den nächsten Schritten aktualisieren wir die Abfrage, sodass die importierten Daten in das gewünschte Format konvertiert werden.
9. Klicken Sie auf dem Menüband **Start** auf **Abfragen bearbeiten**.
   
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/edit-queries.png "Bearbeiten von Abfragen")
10. Klicken Sie im Abfrage-Editor unter der Spalte **Inhalt** auf **Binär**.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query1.png "Bearbeiten von Abfragen")
11. Es wird ein Dateisymbol angezeigt, das die von Ihnen hochgeladene Datei **Drivers.txt** darstellt. Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie auf **CSV**.    
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query2.png "Bearbeiten von Abfragen")
12. Es sollte eine Ausgabe wie unten angezeigt werden. Ihre Daten stehen jetzt in einem Format zur Verfügung, das Sie zum Erstellen von Visualisierungen verwenden können.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query3.png "Bearbeiten von Abfragen")
13. Klicken Sie auf dem Menüband **Start** auf **Schließen und übernehmen** und dann auf **Schließen & übernehmen**.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/load-edited-query.png "Bearbeiten von Abfragen")
14. Sobald die Abfrage aktualisiert wurde, zeigt die Registerkarte **Felder** die neuen Felder, die zur Visualisierung verfügbar sind.
    
    ![Aktualisierte Felder](./media/data-lake-store-power-bi/updated-query-fields.png "Aktualisierte Felder")
15. Erstellen wir nun ein Kreisdiagramm, um für ein bestimmtes Land die Fahrer in jeder Stadt darzustellen. Wählen Sie dazu folgende Optionen aus.
    
    1. Klicken Sie auf der Registerkarte „Visualisierungen“ auf das Symbol für ein Kreisdiagramm.
       
        ![Erstellen eines Kreisdiagramms](./media/data-lake-store-power-bi/create-pie-chart.png "Erstellen eines Kreisdiagramms")
    2. Wir verwenden folgende Spalten: **Spalte 4** (Name der Stadt) und **Spalte 7** (Name des Landes). Ziehen Sie diese Spalten aus der Registerkarte **Felder** auf die Registerkarte **Visualisierungen**, wie unten gezeigt.
       
        ![Erstellen von Visualisierungen](./media/data-lake-store-power-bi/create-visualizations.png "Erstellen von Visualisierungen")
    3. Das Kreisdiagramm sollte nun wie das unten gezeigte aussehen.
       
        ![Kreisdiagramm](./media/data-lake-store-power-bi/pie-chart.png "Erstellen von Visualisierungen")
16. Indem Sie aus den Filtern auf Seitenebene ein bestimmtes Land auswählen, können Sie die Anzahl von Fahrern in jeder Stadt des ausgewählten Landes anzeigen. Wählen Sie z.B. auf der Registerkarte **Visualisierungen** unter **Filter auf Seitenebene** als Land **Brasilien** aus.
    
    ![Auswählen eines Lands](./media/data-lake-store-power-bi/select-country.png "Auswählen eines Lands")
17. Das Kreisdiagramm wird automatisch aktualisiert und zeigt die Fahrer in den brasilianischen Städten an.
    
    ![Treiber in einem Land](./media/data-lake-store-power-bi/driver-per-country.png "Treiber pro Land")
18. Klicken Sie im Menü **Datei** auf **Speichern**, um die Visualisierung als Power BI Desktop-Datei zu speichern.

## <a name="publish-report-to-power-bi-service"></a>Veröffentlichen des Berichts im Power BI-Dienst
Nachdem Sie die Visualisierungen in Power BI Desktop erstellt haben, können Sie diese für andere Personen freigeben, indem Sie sie im Power BI-Dienst veröffentlichen. Ausführliche Anweisungen hierzu finden Sie unter [Veröffentlichen aus Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Siehe auch
* [Analysieren von Daten im Data Lake-Speicher mit Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)


