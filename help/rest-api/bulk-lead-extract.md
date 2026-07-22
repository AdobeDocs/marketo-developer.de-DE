---
title: Massenauszug von Blei
feature: REST API
description: Erfahren Sie, wie Sie mit Marketo-REST-APIs für die Massenextraktion von Leads Leads Datums-, Listen- und Smart-Listen-Filtern, benutzerdefinierten Feldern und CSV/TSV-Formaten exportieren können.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
TQID: https://experienceleague.adobe.com/4eMJR87fHDdccrVid3wHtspvBVQmrBGHYMlIwFCSdEI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1037
ht-degree: 2%

---

# Massenauszug von Blei

[Referenz zum Massenextraktionsendpunkt von Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads)

Die REST-APIs für die Lead-Extraktion in Batches rufen große Mengen von Lead-/Personen-Datensätzen aus Marketo ab. Sie können Leads auch inkrementell basierend auf dem Erstellungsdatum des Datensatzes, der letzten Aktualisierung, der statischen Mitgliedschaft in der Liste oder der Smart-Listen-Mitgliedschaft abrufen.

Verwenden Sie Lead-Massenextraktion für den kontinuierlichen Datenaustausch zwischen Marketo und externen Systemen, einschließlich ETL, Data Warehousing und Archivierungs-Workflows.

## Berechtigungen

Der API-Benutzer, dem der Auftrag gehört, muss über eine Rolle mit der Berechtigung „Schreibgeschützter Lead“, der Berechtigung „Lese-/Schreib-Lead“ oder beiden verfügen.

## Filter

Lead-Exportvorgänge unterstützen mehrere Filtertypen. Jeder Exportvorgang kann nur einen Filtertyp verwenden.

Die `updatedAt`-, `smartListName`- und `smartListId` erfordern eine Infrastruktur, die nicht in allen Abonnements verfügbar ist.

| Filtertyp | Datentyp | Hinweise |
| --- | --- | --- |
| createdAt | Datumsbereich | Ein JSON-Objekt mit `startAt` und `endAt`. `startAt` ist die Datetime mit niedrigem Wasserzeichen und `endAt` die Datetime mit hohem Wasserzeichen. Verwenden Sie ISO-8601-Datums- und Zeitwerte ohne Millisekunden. Der Bereich muss 31 Tage oder weniger betragen. Der Auftrag gibt alle Datensätze zurück, auf die innerhalb des Datumsbereichs zugegriffen werden kann. |
| updatedAt* | Datumsbereich | Ein JSON-Objekt mit `startAt` und `endAt`. `startAt` ist die Datetime mit niedrigem Wasserzeichen und `endAt` die Datetime mit hohem Wasserzeichen. Verwenden Sie ISO-8601-Datums- und Zeitwerte ohne Millisekunden. Der Bereich muss 31 Tage oder weniger betragen. Dieser Filter verwendet nicht das sichtbare `updatedAt` Feld, das nur Aktualisierungen an Standardfeldern widerspiegelt. Stattdessen wird die Zeit der letzten Feldaktualisierung für einen Lead-Datensatz verwendet. Der Auftrag gibt alle Datensätze zurück, auf die im Datumsbereich zugegriffen werden kann und die zuletzt aktualisiert wurden. |
| staticListName | Zeichenfolge | Der Name einer statischen Liste. Der Auftrag gibt alle Datensätze zurück, auf die zugegriffen werden kann und die Mitglieder der statischen Liste sind, wenn der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listennamen mithilfe des Endpunkts „Listen abrufen“ ab. |
| staticListId | Ganzzahl | Die ID einer statischen Liste. Der Auftrag gibt alle Datensätze zurück, auf die zugegriffen werden kann und die Mitglieder der statischen Liste sind, wenn der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listen-IDs mithilfe des GET-Listen-Endpunkts ab. |
| smartListName* | String | Der Name einer Smart-Liste. Der Auftrag gibt alle Datensätze zurück, auf die zugegriffen werden kann und die Mitglieder der Smart-Liste sind, wenn der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listennamen mithilfe des Endpunkts „Smart-Listen abrufen“. |
| smartListId* | Ganzzahl | Die ID einer Smart-Liste. Der Auftrag gibt alle Datensätze zurück, auf die zugegriffen werden kann und die Mitglieder der Smart-Liste sind, wenn der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listen-IDs mithilfe des Endpunkts „Smart-Listen abrufen“. |

Die mit einem Sternchen gekennzeichneten Filtertypen sind für einige Abonnements nicht verfügbar. Wenn für Ihr Abonnement kein Filtertyp verfügbar ist, gibt der Endpunkt Exportleitungs-Auftrag erstellen den Fehler „1035, Nicht unterstützter Filtertyp für Zielabonnement“ zurück. Wenden Sie sich an den Marketo-Support, um diese Funktion für Ihr Abonnement zu aktivieren.

## Optionen

Der Endpunkt Exportvorgang erstellen bietet Optionen zum Auswählen exportierter Felder, Umbenennen von Spaltenüberschriften und Festlegen des Dateiformats.

| Parameter | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| Felder | array[string] | Ja | Ein JSON-Array von Zeichenfolgen. Jede Zeichenfolge muss der REST-API-Name eines Marketo-Lead-Felds sein. Der Export umfasst jedes aufgelistete Feld und verwendet den zugehörigen REST-API-Namen als Spaltenüberschrift, es sei denn, `columnHeaderNames` überschreibt es. Wenn die [!DNL Adobe Experience Cloud Audience Sharing]-Funktion aktiviert ist, verknüpft ein Cookie-Synchronisierungsprozess die [!DNL Adobe Experience Cloud]-ID (ECID) mit Marketo-Leads. Geben Sie das `ecids` an, um ECIDs in die Exportdatei aufzunehmen. |
| columnHeaderNames | Objekt | Nein | Ein JSON-Objekt mit Schlüssel-Wert-Paaren für Felder und Spaltenüberschriften. Jeder Schlüssel muss der API-Name eines Felds sein, das im Exportvorgang enthalten ist. Rufen Sie den API-Namen ab, indem Sie „Lead beschreiben“ aufrufen. Jeder Wert ist die exportierte Spaltenüberschrift für dieses Feld. |
| Format | String | Nein | Das Exportdateiformat: CSV für kommagetrennte Werte, TSV für tabulatorgetrennte Werte oder SSV für Leerzeichen getrennte Werte. Der Standardwert ist CSV. |

## Erstellen von Aufträgen

Verwenden Sie den Endpunkt [Exportvorgang erstellen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) um einen Exportvorgang zu definieren. Geben Sie die zu exportierende `fields`, einen `filter` und dessen Parameter, den `format` und alle benutzerdefinierten Spaltenkopfzeilennamen an.

```http
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "endAt": "2017-01-31T00:00:00Z"
      }
   }
}
```

Diese Anfrage erstellt einen Exportvorgang für Leads, die zwischen dem 1. Januar 2017 und dem 31. Januar 2017 erstellt wurden. Der Export enthält Werte aus den Feldern `firstName`, `lastName`, `id` und `email`.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Die Antwort bestätigt, dass der Auftrag erstellt, aber nicht gestartet wurde. Um den Auftrag zu starten, rufen Sie den Endpunkt [Exportvorgang in Warteschlange stellen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) mit dem `exportId` aus der Erstellungsantwort auf.

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

Die Enqueue-Antwort weist den `status` „In Warteschlange“ auf. Sobald ein Exportsteckplatz verfügbar ist, wechselt der Status zu „Verarbeitung läuft“.

## Status des Abrufauftrags

Sie können den Status nur für Aufträge abrufen, die von demselben API-Benutzer erstellt wurden.

Lead-Exportvorgänge werden asynchron ausgeführt. Abfrage [&#x200B; Endpunkts „Exportstatus des Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET), um den Fortschritt des Auftrags zu verfolgen.

Der Status wird nur einmal alle 60 Sekunden aktualisiert. Führen Sie keine häufigeren Befragungen durch; in fast allen Fällen ist dieses Intervall immer noch zu lang.

```http
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Diese Antwort zeigt an, dass der Auftrag noch verarbeitet wird, sodass die Datei nicht verfügbar ist. Wenn sich der Auftragsstatus in „Abgeschlossen“ ändert, kann die Datei heruntergeladen werden.

Das `status` kann einen der folgenden Werte zurückgeben:

- Erstellt
- In Warteschl. versch
- Verarbeitung läuft
- Abgebrochen
- Abgeschlossen
- Fehlgeschlagen

## Daten abrufen

Um einen abgeschlossenen Lead-Export abzurufen, rufen Sie den Endpunkt [Lead-](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) abrufen) mit dem `exportId` auf.

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

Der Antworttext enthält die Datei im für den Auftrag konfigurierten Format.

Wenn ein angefordertes Lead-Feld keine Daten enthält, enthält das entsprechende Feld in der Exportdatei `null`. Im folgenden Beispiel hat der zurückgegebene Lead ein leeres E-Mail-Feld.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Für den teilweisen oder wiederholbaren Abruf unterstützt der Dateiendpunkt den optionalen HTTP-`Range`-Header mit dem `bytes`. Wenn Sie die -Kopfzeile nicht festlegen, gibt der Endpunkt den gesamten Inhalt zurück. Erfahren Sie mehr über die Verwendung des `Range`-Headers mit Marketo [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Um einen falsch konfigurierten oder unnötigen Auftrag abzubrechen, rufen Sie den Endpunkt [Exportleadauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) auf.

```http
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Die Antwort bestätigt, dass der Vorgang abgebrochen wurde.
