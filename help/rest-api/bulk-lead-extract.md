---
title: Massenauszug von Blei
feature: REST API
description: Erfahren Sie, wie Sie mit Marketo-REST-APIs für die Massenextraktion von Leads Leads Datums-, Listen- und Smart-Listen-Filtern, benutzerdefinierten Feldern und CSV/TSV-Formaten exportieren können.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 2%

---

# Massenauszug von Blei

[Referenz zum Massenextraktions-Endpunkt für Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

Der Satz von REST-APIs für die Lead-Massenextraktion bietet eine programmgesteuerte Schnittstelle zum Abrufen großer Mengen von Lead/Personen-Datensätzen aus Marketo. Sie kann auch verwendet werden, um Leads inkrementell basierend auf dem Erstellungsdatum des Datensatzes, der letzten Aktualisierung, der statischen Listenmitgliedschaft oder der Smart-Listen-Mitgliedschaft abzurufen. Die empfohlene Benutzeroberfläche für Anwendungsfälle, für die ein kontinuierlicher Datenaustausch zwischen Marketo und einem oder mehreren externen Systemen für ETL-, Data Warehousing- und Archivierungszwecke erforderlich ist.

## Berechtigungen

Die APIs zum Massen-Lead-Extrahieren erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der Berechtigungen Schreibgeschützter Lead oder Lese-/Schreib-Lead verfügt.

## Filter

Leads unterstützen verschiedene Filteroptionen. Bestimmte Filter, einschließlich `updatedAt`, `smartListName` und `smartListId`, erfordern zusätzliche Infrastrukturkomponenten, die noch nicht für alle Abonnements bereitgestellt wurden. Pro Exportvorgang kann nur ein Filtertyp angegeben werden.

| Filtertyp | Datentyp | Hinweise |
|---|---|---|
| createdAt | Datumsbereich | Akzeptiert ein JSON-Objekt mit den `startAt` und `endAt`. `startAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und `endAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die innerhalb des Datumsbereichs zugegriffen werden kann. |
| updatedAt* | Datumsbereich | Akzeptiert ein JSON-Objekt mit den `startAt` und `endAt`. `startAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und `endAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden. Hinweis: Dieser Filter filtert nicht nach dem sichtbaren Feld „updatedAt“, sondern nur Aktualisierungen an Standardfeldern. Es filtert danach, wann die letzte Aktualisierung des Felds an einen Lead-Datensatz vorgenommen wurde. Vorgänge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zuletzt im Datumsbereich aktualisiert wurden. |
| staticListName | String | Akzeptiert den Namen einer statischen Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der statischen Liste sind, zu dem der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listennamen mithilfe des Endpunkts „Listen abrufen“ ab. |
| staticListId | Ganzzahl | Akzeptiert die ID einer statischen Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der statischen Liste sind, zu dem der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listen-IDs mithilfe des GET-Listen-Endpunkts ab. |
| smartListName* | String | Akzeptiert den Namen einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der Smart-Listen sind, zu dem der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listennamen mithilfe des Endpunkts „Smart-Listen abrufen“. |
| smartListId* | Ganzzahl | Akzeptiert die ID einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der Smart-Listen sind, zu dem der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listen-IDs mit dem Endpunkt Smart-Listen abrufen . |

Filtertyp ist für einige Abonnements nicht verfügbar. Wenn für Ihr Abonnement nicht verfügbar ist, erhalten Sie eine Fehlermeldung beim Aufruf des Endpunkts „Exportleitungs-Auftrag erstellen“ („1035, Nicht unterstützter Filtertyp für Zielabonnement„). Kunden können sich an den Marketo-Support wenden, um diese Funktion in ihrem Abonnement aktivieren zu lassen.

## Optionen

Der Endpunkt Exportvorgang erstellen bietet mehrere Formatierungsoptionen, die es Benutzenden ermöglichen, bestimmte Felder in die exportierte Datei einzuschließen, Spaltenüberschriften dieser Felder umzubenennen und das Format der exportierten Datei anzugeben.

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| Felder | array[string] | Ja | Der Feldparameter akzeptiert ein JSON-Zeichenfolgen-Array. Jede Zeichenfolge muss der REST-API-Name eines Marketo-Lead-Felds sein. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Die Spaltenüberschrift für jedes Feld ist der REST-API-Name jedes Felds, sofern er nicht mit columnHeader überschrieben wird. Hinweis: Wenn die [!DNL Adobe Experience Cloud Audience Sharing]-Funktion aktiviert ist, findet ein Cookie-Synchronisierungsvorgang statt, der [!DNL Adobe Experience Cloud] ID (ECID) mit Marketo-Leads verknüpft. Sie können das Feld „ecids“ angeben, um ECIDs in die Exportdatei aufzunehmen. |
| columnHeaderNames | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenkopfzeilennamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportvorgang enthalten ist. Dies ist der API-Name des Felds, das durch Aufruf von Describe Lead abgerufen werden kann. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Format | Zeichenfolge | Nein | Akzeptiert eine der folgenden Optionen: CSV, TSV, SSV. Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertedatei gerendert, sofern festgelegt. Die Standardeinstellung ist CSV, wenn nicht festgelegt. |

## Erstellen von Aufträgen

Die Parameter für den Auftrag werden vor dem Start des Exports mithilfe des Endpunkts [Exportvorgang erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) definiert. Wir müssen die für den Export erforderlichen `fields`, den Parametertyp des `filter`, den `format` der Datei und gegebenenfalls die Namen der Spaltenüberschriften definieren.

```
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
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Diese Anfrage beginnt mit dem Export eines Lead-Satzes, der zwischen dem 1. Januar 2017 und dem 31. Januar 2017 erstellt wurde, einschließlich der Werte aus den entsprechenden `firstName`-, `lastName`-, `id`- und `email`.

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

Dadurch wird eine Statusantwort zurückgegeben, die angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, aber noch nicht gestartet. Dazu muss der Endpunkt [Exportauftrag in die Warteschlange einreihen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) mit der exportId aus der Erstellungsstatusantwort aufgerufen werden:

```
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

Daraufhin wird mit der `status` „In Warteschlange“ geantwortet, die auf „Wird verarbeitet“ gesetzt wird, wenn ein Exportsteckplatz verfügbar ist.

## Status des Abrufauftrags

`Note:` Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags dessen Status abfragen, um den Fortschritt zu ermitteln. Abfrage mit dem Endpunkt [Status des Exportvorgangs abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine niedrigere Abfrageintervall nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Werfen wir einen kurzen Blick auf die Umfragen.

```
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

Der Status-Endpunkt antwortet und gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Sobald sich der Auftragsstatus in „Abgeschlossen“ ändert, wird er zum Download vorbereitet.

Das Statusfeld kann mit einem der folgenden Elemente antworten:

- Erstellt
- In Warteschl. versch
- Verarbeitung läuft
- Abgebrochen
- Abgeschlossen
- Fehlgeschlagen

## Daten abrufen

Um die Datei eines abgeschlossenen Lead-Exports abzurufen, rufen Sie einfach den Endpunkt [Lead-Exportdatei abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) mit Ihrem `exportId` auf.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

Die Antwort enthält eine -Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der -Datei.

Wenn ein angefordertes Lead-Feld leer ist (keine Daten enthält), wird `null` in der Exportdatei im entsprechenden Feld platziert. Im folgenden Beispiel ist das E-Mail-Feld für den zurückgegebenen Lead leer.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Um das partielle und fortsetzungsfreundliche Abrufen extrahierter Daten zu unterstützen, unterstützt der Datei-Endpunkt optional den HTTP-Header-Bereich vom Typ Byte. Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Erfahren Sie mehr über die Verwendung des Bereichs-Headers mit Marketo [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem Endpunkt [Exportvorgang abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) abgebrochen werden:

```
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

Daraufhin wird ein Status angezeigt, der angibt, dass der Vorgang abgebrochen wurde.
