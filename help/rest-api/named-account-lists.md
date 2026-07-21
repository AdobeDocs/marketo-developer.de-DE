---
title: Spezifische Kontolisten
feature: REST API
description: Erfahren Sie, wie Sie Marketo Named Account Lists mit der REST-API verwalten, einschließlich Berechtigungen, Feldern, Filtern und Endpunkten zum Abfragen, Erstellen, Aktualisieren und Löschen.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
TQID: https://experienceleague.adobe.com/18lMhheW21Gz1-3TMHwleHhmLTOqJsZSQ5aqkbbchhM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 686
ht-degree: 3%

---

# Spezifische Kontolisten

[Endpunkt-Referenz für benannte Kontolisten](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Account-Lists)

[Spezifische Kontenlisten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) sind Sammlungen von spezifischen Konten in Marketo. Verwenden Sie sie für die Kategorisierung, Datenanreicherung und intelligente Kampagnenfilterung.

Mit den APIs für die Liste benannter Konten können Sie Listen-Assets und ihre Abonnements remote verwalten.
`Content`

## Berechtigungen

Die erforderliche Berechtigung hängt vom Vorgang ab:

- Abfrage von Listen benannter Konten: Nur-Lese-Liste benannter Konten oder Lese-/Schreibzugriff benannter Konten.
- Erstellen, Aktualisieren oder Löschen von Listen: Liste mit Lese- und Schreibzugriff für benannte Konten.
- Abfragelistenmitgliedschaft: Schreibgeschütztes benanntes Konto oder Lese-/Schreib-benanntes Konto.
- Verwalten der Listenmitgliedschaft: Benanntes Konto mit Lese-/Schreibzugriff.

## Modell

Benannte Kontolisten verfügen über einen begrenzten Satz von Standardfeldern und unterstützen keine benutzerdefinierten Felder.
`Named Account List Field`

| Name | Datentyp | Aktualisierbar | Hinweise |
| --- | --- | --- | --- |
| marketoGUID | String | Falsch | Eindeutige Zeichenfolgenkennung der benannten Kontoliste. Dieses Feld wird vom System verwaltet und ist beim Erstellen eines Datensatzes nicht als Feld zulässig. Von „dedupeBy“ verwendetes Feld:„idField“ beim Erstellen oder Aktualisieren. |
| name | String | True | Name der Liste. Von „dedupeBy“ verwendetes Feld: „dedupeFields“ bei der Durchführung eines Erstellungs- oder Aktualisierungsvorgangs. |
| createdAt | Datum/Uhrzeit | Falsch | Datum/Uhrzeit der Erstellung der Liste. Dieses Feld wird vom System verwaltet und ist als Feld beim Erstellen oder Aktualisieren eines Datensatzes nicht zulässig. |
| updatedAt | Datum/Uhrzeit | Falsch | Datum/Uhrzeit der letzten Aktualisierung der Liste. Dieses Feld wird vom System verwaltet und ist als Feld beim Erstellen oder Aktualisieren eines Datensatzes nicht zulässig. |
| type | String | Falsch | Listentyp. Kann den Wert „default“ oder „external“ haben. Externe Listen werden von der CRM-Kontoansicht erstellt. |

## Abfrage

Abfragen von benannten Kontolisten unterstützen zwei Filtertypen: „dedupeFields“ und „idField“. Legen Sie das Feld im `filterType` Abfrageparameter fest und geben Sie die Werte in `filterValues as` kommagetrennten Liste an.

Die Filter `nextPageToken` und `batchSize` sind optional.

```http
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

Einträge in benannten Kontolisten mithilfe des standardmäßigen Lead-Datenbankmusters erstellen und aktualisieren. Benannte Kontolisten haben nur ein aktualisierbares Feld: `name`.

Der Endpunkt unterstützt zwei Standardaktionstypen: „createOnly“ und „updateOnly“. Die `action defaults` zu „createOnly“.

Sie können die optionale `dedupeBy parameter` angeben, wenn die Aktion `updateOnly` wird. Die zulässigen Werte sind „dedupeFields“, was „name“ entspricht, und „idField“, was „marketoGUID“ entspricht.

In `createOnly` Modi ist nur „name“ als `dedupeBy` zulässig. Sie können bis zu 300 Datensätze gleichzeitig senden.

```http
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

Löschen Sie Listen benannter Konten entweder mithilfe der `name` oder `marketoGUID` der Liste. Um den Schlüssel auszuwählen, übergeben Sie „dedupeFields“ für den Namen oder „idField“ für marketoGUID im`deleteB`Mitglied der Anfrage.

Wenn nicht festgelegt, ist der Wert standardmäßig deduplizierte Felder. Sie können bis zu 300 Datensätze gleichzeitig löschen.

```http
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

Wenn für einen Schlüssel kein Datensatz gefunden werden kann, weist das entsprechende Ergebniselement den `status` „übersprungen“ auf. Sie enthält auch einen Grund mit einem Code und einer Meldung, die den Fehler beschreiben.

## Verwalten der Mitgliedschaft

### Abfrage-Zugehörigkeit

Fragen Sie die Mitgliedschaft in der benannten Kontoliste ab, indem Sie die`i` der Kontoliste angeben. Die optionalen Parameter sind:

-`field`: Eine kommagetrennte Liste von Feldern, die in die Antwortdatensätze aufgenommen werden sollen
-`nextPageToke` - für das Paging durch den Ergebnissatz
-`batchSiz` - zum Angeben der Anzahl der zurückzugebenden Datensätze

Wenn`field` nicht festgelegt ist`marketoGUI` werden `nam`, `createdA` und `updatedA` zurückgegeben. `batchSiz` hat einen maximalen und einen standardmäßigen Wert von 300.

```http
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

Hinzufügen von benannten Konten zu einer Liste benannter Konten mithilfe ihrer marketoGUID. Sie können bis zu 300 Datensätze gleichzeitig hinzufügen.

```http
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

Beim Entfernen von Datensätzen aus einer Kontoliste wird ein anderer Pfad, aber dieselbe Schnittstelle verwendet. Geben Sie für `marketoGUI` zu entfernenden Datensatz ein. Sie können bis zu 300 Datensätze gleichzeitig entfernen.

```http
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

- Die maximale Wartezeit für Endpunkte benannter Kontolisten beträgt 30 Sekunden, sofern nicht anders angegeben.
- Die maximale Wartezeit für die Synchronisierung von benannten Kontolisten beträgt 60 Sekunden.
- Die maximale Wartezeit für das Löschen von benannten Kontolisten beträgt 60 Sekunden.
- Die maximale Wartezeit von benannten Kontolisten für GET beträgt 60.
- Mitglieder der Liste benannter Konten hinzufügen haben eine Zeitüberschreitung von 60 Jahren.
- Die maximale Wartezeit von benannten Mitgliedern der Kontoliste beträgt 60 Sekunden.
- Die maximale Wartezeit von Mitgliedern der Liste benanntes Konto abrufen beträgt 60 Sekunden.
