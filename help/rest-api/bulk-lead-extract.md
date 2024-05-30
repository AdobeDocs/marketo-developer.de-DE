---
title: "Bulk Lead Extract"
feature: REST API
description: "Batch-Extraktion von Lead-Daten."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1173'
ht-degree: 2%

---


# Bulk-Lead-Extraktion

[Referenz zu Massen-Lead-Extract-Endpunkt](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

Der Satz von REST-APIs zur Massenextraktion von Leads bietet eine programmatische Schnittstelle zum Abrufen großer Mengen von Lead-/Personendatensätzen aus Marketo. Außerdem können damit Leads schrittweise abgerufen werden, basierend auf dem Erstellungsdatum des Datensatzes, der neuesten Aktualisierung, der statischen Listenzugehörigkeit oder der Mitgliedschaft in einer intelligenten Liste. Die empfohlene Schnittstelle für Anwendungsfälle, die einen kontinuierlichen Datenaustausch zwischen Marketo und einem oder mehreren externen Systemen erfordern, zum ETL, Data Warehouse und zur Archivierung.

## Berechtigungen

Für die Bulk Lead Extract-APIs muss der Eigentümer-API-Benutzer über eine oder beide der Lese-/Schreibzugriff-Lead-Berechtigungen verfügen.

## Filter

Leads unterstützen verschiedene Filteroptionen. Bestimmte Filter, einschließlich der `updatedAt`, `smartListName`, und `smartListId` zusätzliche Infrastrukturkomponenten erforderlich, die noch nicht für alle Abonnements bereitgestellt wurden. Pro Exportauftrag kann nur ein Filtertyp angegeben werden.

| Filtertyp | Datentyp | Hinweise |
|---|---|---|
| createdAt | Datumsbereich | Akzeptiert ein JSON-Objekt mit den Mitgliedern `startAt` und `endAt`. `startAt` akzeptiert einen Datum/Uhrzeit-Wert, der das niedrige Wasserzeichen darstellt, und `endAt` Akzeptiert einen Datum/Uhrzeit-Wert, der das High-Wasserzeichen darstellt. Der Zeitraum muss 31 Tage oder weniger betragen. Die Datenzeiten sollten im ISO-8601-Format vorliegen, ohne Millisekunden. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die innerhalb des Datumsbereichs erstellt wurden. |
| updatedAt* | Datumsbereich | Akzeptiert ein JSON-Objekt mit den Mitgliedern `startAt` und `endAt`. `startAt` akzeptiert einen Datum/Uhrzeit-Wert, der das niedrige Wasserzeichen darstellt, und `endAt` Akzeptiert einen Datum/Uhrzeit-Wert, der das High-Wasserzeichen darstellt. Der Zeitraum muss 31 Tage oder weniger betragen. Die Datenzeiten sollten im ISO-8601-Format vorliegen, ohne Millisekunden. Hinweis: Dieser Filter filtert nicht nach dem sichtbaren Feld &quot;updatedAt&quot;, das nur Aktualisierungen an Standardfeldern widerspiegelt. Er filtert basierend darauf, wann die letzte Feldaktualisierung an einem Lead-RecordJobs mit diesem Filtertyp vorgenommen wurde, gibt alle verfügbaren Datensätze zurück, die zuletzt im Datumsbereich aktualisiert wurden. |
| staticListName | Zeichenfolge | Akzeptiert den Namen einer statischen Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zum Zeitpunkt der Verarbeitung des Auftrags Mitglieder der statischen Liste sind. Rufen Sie mit dem Endpunkt &quot;Listen abrufen&quot;statische Listennamen ab. |
| staticListId | Ganze Zahl | Akzeptiert die ID einer statischen Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zum Zeitpunkt der Verarbeitung des Auftrags Mitglieder der statischen Liste sind. Rufen Sie statische Listen-IDs mit dem Endpunkt &quot;Listen abrufen&quot;ab. |
| smartListName* | Zeichenfolge | Akzeptiert den Namen einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zu dem Zeitpunkt, zu dem der Auftrag verarbeitet wird, Mitglieder der Smart-Listen sind. Rufen Sie mit dem Endpunkt Smart-Listen abrufen Smart-Namen ab. |
| smartListId* | Ganze Zahl | Akzeptiert die ID einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zu dem Zeitpunkt, zu dem der Auftrag verarbeitet wird, Mitglieder der Smart-Listen sind. Rufen Sie Smart-Listen-IDs mithilfe des Endpunkts Smart-Listen abrufen ab. |


Für einige Abonnements ist der Filtertyp nicht verfügbar. Wenn Sie für Ihr Abonnement nicht verfügbar sind, erhalten Sie beim Aufruf des Endpunkts &quot;Lead-Exportauftrag erstellen&quot;eine Fehlermeldung (&quot;1035, Nicht unterstützter Filtertyp für Zielabonnement&quot;). Kunden können sich an den Marketo-Support wenden, damit diese Funktion in ihrem Abonnement aktiviert wird.

## Optionen

Der Endpunkt &quot;Lead-Auftrag erstellen&quot;bietet verschiedene Formatierungsoptionen, mit denen Benutzer bestimmte Felder in die exportierte Datei aufnehmen können, Spaltenüberschriften dieser Felder umbenennen können und das Format der exportierten Datei.

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| Felder | Array[Zeichenfolge] | Ja | Der Feldparameter akzeptiert ein JSON-Array von Zeichenfolgen. Jede Zeichenfolge muss der REST-API-Name eines Marketo-Lead-Felds sein. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Die Spaltenüberschrift für jedes Feld ist der REST-API-Name jedes Felds, es sei denn, sie wird mit columnHeader überschrieben. Hinweis: Wenn die Variable [!DNL Adobe Experience Cloud Audience Sharing] aktiviert ist, erfolgt ein Cookie-Synchronisierungsprozess, der [!DNL Adobe Experience Cloud] ID (ECID) mit Marketo-Leads. Sie können das Feld &quot;ecids&quot;angeben, um ECIDs in die Exportdatei aufzunehmen. |
| columnHeaderName | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenüberschriftsnamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportauftrag enthalten ist. Dies ist der API-Name des Felds, das durch Aufruf von &quot;Lead beschreiben&quot;abgerufen werden kann. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| format | Zeichenfolge | Nein | Akzeptiert eines von: CSV, TSV, SSV. Die exportierte Datei wird als Datei mit kommagetrennten Werten, tabulatorgetrennten Werten oder durch Leerzeichen getrennten Werten gerendert, sofern festgelegt. Wenn nicht festgelegt, wird standardmäßig CSV verwendet. |


## Erstellen eines Auftrags

Die Parameter für den Auftrag werden definiert, bevor der Export mit der [Lead-Exportauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) -Endpunkt. Wir müssen die `fields` die für den Export benötigt werden, der Typ der Parameter `filter`, die `format` der Datei und gegebenenfalls der Spaltenüberschriften.

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

Diese Anfrage beginnt mit dem Export einer Reihe von Leads, die zwischen dem 1. Januar 2017 und dem 31. Januar 2017 erstellt wurden, einschließlich der Werte aus dem entsprechenden `firstName`, `lastName`, `id`, und `email` -Felder.

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

Dadurch wird eine Statusantwort zurückgegeben, die angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, wurde jedoch noch nicht gestartet. Dazu muss die Variable [Lead-Vorgang beim Export in die Warteschlange](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) Endpunkt muss mit der exportId aus der Erstellungsstatusantwort aufgerufen werden:

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

Dies antwortet mit einer `status` des Felds &quot;In Warteschlange&quot;, nach dem der Wert auf &quot;Verarbeitung&quot;eingestellt wird, wenn ein Exportfenster verfügbar ist.

## Abruf-Auftragsstatus

`Note:` Der Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags seinen Status abfragen, um seinen Fortschritt zu ermitteln. Umfrage mit der [Status des Lead-Exportauftrags abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) -Endpunkt. Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine kürzere Abrufhäufigkeit nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Sehen wir uns die Umfrage kurz an.

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

Der Statusendpunkt gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Sobald sich der Auftragsstatus in &quot;Abgeschlossen&quot;ändert, kann er heruntergeladen werden.

Das Statusfeld kann mit einer der folgenden Optionen antworten:

- Erstellt
- In Warteschl. versch
- Verarbeitung läuft
- Abgebrochen
- Abgeschlossen
- Fehlgeschlagen

## Abrufen Ihrer Daten

Um die Datei eines abgeschlossenen Lead-Exports abzurufen, rufen Sie einfach die [Lead-Datei exportieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) Endpunkt mit `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

Die Antwort enthält eine Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der Datei.

Wenn ein angefordertes Lead-Feld leer ist (keine Daten enthält), dann `null` wird in das entsprechende Feld in der Exportdatei eingefügt. Im folgenden Beispiel ist das E-Mail-Feld für den zurückgegebenen Lead leer.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Um das teilweise und wiederverwendbare Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional den HTTP-Header-Bereich der Typ-Bytes. Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Weitere Informationen zur Verwendung der Bereichskopfzeile mit Marketo [Massenextraktion](bulk-extract.md).

## Abbruch eines Auftrags

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem [Export Lead Job abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) Endpunkt:

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

Diese Antwort erhält einen Status, der angibt, dass der Auftrag abgebrochen wurde.
