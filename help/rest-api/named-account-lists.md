---
title: Spezifische Kontolisten
feature: REST API
description: Konfigurieren von Listen benannter Konten.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 3%

---

# Spezifische Kontolisten

[Endpunkt-Referenz für benannte Kontolisten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Spezifische Kontolisten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) in Marketo stellen Sammlungen benannter Konten dar. Sie können für eine Vielzahl von Fällen verwendet werden, einschließlich Kategorisierung, Datenanreicherung und intelligenter Kampagnenfilterung. Die APIs für die Liste benannter Konten ermöglichen die Remote-Verwaltung dieser Listen-Assets und ihrer Mitgliedschaft.
`Content`

## Berechtigungen

Zum Abfragen von Listen benannter Konten ist die Berechtigung Schreibgeschützter benannter Konten oder die Berechtigung zum Lesen/Schreiben der Liste benannter Konten erforderlich. Zum Erstellen, Aktualisieren oder Löschen von Listen ist die Berechtigung zum Lesen/Schreiben von Listen mit benannten Konten erforderlich. Für die Abfrage der Listenmitgliedschaft sind die Berechtigungen Schreibgeschütztes benanntes Konto oder Schreibgeschütztes benanntes Konto erforderlich, während für die Verwaltung der Mitgliedschaft die Berechtigungen Schreibzugriff mit benanntem Konto erforderlich sind.

## Modell

Benannte Kontolisten verfügen über eine begrenzte Anzahl von Standardfeldern und sind nicht mit benutzerdefinierten Feldern erweiterbar.
`Named Account List Field`

| Name | Datentyp | Aktualisierbar | Hinweise |
|---|---|---|---|
| marketoGUID | Zeichenfolge | Falsch | Eindeutige Zeichenfolgenkennung der benannten Kontoliste. Dieses Feld wird vom System verwaltet und ist beim Erstellen eines Datensatzes nicht als Feld zulässig. Von „dedupeBy“ verwendetes Feld:„idField“ beim Erstellen oder Aktualisieren. |
| name | Zeichenfolge | True | Name der Liste. Von „dedupeBy“ verwendetes Feld: „dedupeFields“ bei der Durchführung eines Erstellungs- oder Aktualisierungsvorgangs. |
| createdAt | Datum/Uhrzeit | Falsch | Datum/Uhrzeit der Erstellung der Liste. Dieses Feld wird vom System verwaltet und ist als Feld beim Erstellen oder Aktualisieren eines Datensatzes nicht zulässig. |
| updatedAt | Datum/Uhrzeit | Falsch | Datum/Uhrzeit der letzten Aktualisierung der Liste. Dieses Feld wird vom System verwaltet und ist als Feld beim Erstellen oder Aktualisieren eines Datensatzes nicht zulässig. |
| type | Zeichenfolge | Falsch | Listentyp. Kann den Wert „default“ oder „external“ haben. Externe Listen werden von der CRM-Kontoansicht erstellt. |

## Abfrage

Die Abfrage von Kontolisten ist einfach und unkompliziert. Derzeit gibt es nur zwei gültige Filtertypen für die Abfrage benannter Kontolisten: „dedupeFields“ und „idField“. Das Feld, nach dem gefiltert werden soll, wird im `filterType`-Parameter der Abfrage festgelegt, und die Werte werden in `filterValues as` kommagetrennten Liste festgelegt. Die `nextPageToken`- und `batchSize` sind ebenfalls optionale Parameter.

```
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Erstellen und aktualisieren

Das Erstellen und Aktualisieren von Datensätzen mit benannten Kontolisten folgt den etablierten Mustern für andere Vorgänge zum Erstellen und Aktualisieren von Lead-Datenbanken. Beachten Sie, dass die Listen für benannte Konten nur ein aktualisierbares Feld `name` haben.

Der Endpunkt lässt die beiden Standardaktionstypen zu: „createOnly“ und „updateOnly“.  Die `action defaults` zu „createOnly“.

Die optionale `dedupeBy parameter` kann angegeben werden, wenn die Aktion `updateOnly` ist.  Zulässige Werte sind „dedupeFields“ (entspricht „name„) oder „idField“ (entspricht „marketoGUID„).  In `createOnly` Modi ist nur „name“ als `dedupeBy` zulässig. Sie können bis zu 300 Datensätze gleichzeitig senden.

```
POST /rest/v1/namedAccountLists.json
```

```json
{
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "name": "SAAS List"
      },
      {
         "name": "Manufacturing (Domestic)"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Löschen

Das Löschen von Listen benannter Konten ist einfach und kann entweder auf der Grundlage der `name` oder der `marketoGUID` der Liste erfolgen. Um den Schlüssel auszuwählen, den Sie verwenden möchten, übergeben Sie entweder „dedupeFields“ für den Namen oder „idField“ für marketoGUID im `deleteB` Ihrer Anfrage. Wenn nicht festgelegt, werden standardmäßig deduplizierte Felder verwendet. Sie können bis zu 300 Datensätze gleichzeitig löschen.

```
POST /rest/v1/namedAccountLists/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
         "name": "Saas List"
      },
      {
         "name": "B2C List"
      },
      {
         "name": "Launchpoint Partner List"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      {
         "seq": 2,
         "status": "skipped",
         "reasons": [
            {
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

Falls für einen bestimmten Schlüssel kein Datensatz gefunden werden kann, weist das entsprechende Ergebniselement den `status` „übersprungen“ und einen Grund mit einem Code und einer Meldung auf, die den Fehler beschreiben, wie im obigen Beispiel gezeigt.

## Verwalten der Mitgliedschaft

### Abfrage-Zugehörigkeit

Die Abfrage der Mitgliedschaft in einer benannten Kontoliste ist einfach und erfordert nur die `i` der Kontoliste. Optionale Parameter sind:

-`field`: Eine kommagetrennte Liste von Feldern, die in die Antwortdatensätze aufgenommen werden sollen
-`nextPageToke` - für das Paging durch den Ergebnissatz
-`batchSiz` - zum Angeben der Anzahl der zurückzugebenden Datensätze

Wenn`field` nicht festgelegt ist`marketoGUI` werden `nam`, `createdA` und `updatedA` zurückgegeben. `batchSiz` hat einen maximalen und einen standardmäßigen Wert von 300.

```
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Mitglieder hinzufügen

Benannte Konten können einfach zu einer Liste benannter Konten hinzugefügt werden. Konten können nur mit ihrer marketoGUID hinzugefügt werden. Sie können bis zu 300 Datensätze gleichzeitig hinzufügen.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Mitglieder entfernen

Das Entfernen von Datensätzen aus einer Kontenliste hat einen anderen Pfad, aber dieselbe Benutzeroberfläche, sodass für jeden Datensatz, den Sie löschen möchten, `marketoGUI` erforderlich ist. Sie können bis zu 300 Datensätze gleichzeitig entfernen.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Zeitüberschreitungen

- Die maximale Wartezeit für Endpunkte benannter Kontolisten beträgt 30 Sekunden, es sei denn, dies wird unten angegeben
   - Spezifische Kontenlisten synchronisieren: 60er
   - Spezifische Kontenlisten löschen: 60er
   - Benannte Kontenlisten abrufen: 60er
   - Mitglieder der benannten Kontoliste hinzufügen: 60s
   - Mitglieder der benannten Kontoliste entfernen: 60s
   - Mitglieder der benannten Kontoliste abrufen: 60er
