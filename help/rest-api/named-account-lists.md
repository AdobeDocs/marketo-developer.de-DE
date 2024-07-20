---
title: Namenskontenlisten
feature: REST API
description: Spezifische Kontolisten konfigurieren.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 3%

---

# Namenskontenlisten

[Endpunktverweis für benannte Kontolisten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Namensgebene Kontolisten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) in Marketo stellen Sammlungen benannter Konten dar. Sie können für eine Vielzahl von Fällen verwendet werden, einschließlich Kategorisierung, Anreicherung von Daten und Filterung intelligenter Kampagnen. Die APIs für die Liste benannter Konten ermöglichen die Remote-Verwaltung dieser Listen-Assets und ihrer Mitgliedschaft.
`Content`

## Berechtigungen

Zum Abfragen von Listen mit benannten Konten ist die Berechtigung Schreibgeschützte Kontoliste oder Lese-Schreib-Liste mit benannten Konten erforderlich. Zum Erstellen, Aktualisieren oder Löschen von Listen ist die Berechtigung &quot;Benannte Kontoliste lesen&quot;erforderlich. Für die Abfrage der Listenzugehörigkeit sind die Berechtigungen &quot;Schreibgeschütztes Konto&quot;oder &quot;Benanntes Konto lesen/schreiben&quot;erforderlich. Für die Verwaltung der Mitgliedschaft sind die Berechtigungen &quot;Benanntes Konto lesen und schreiben&quot;erforderlich.

## Modell

Namenslisten verfügen über eine begrenzte Anzahl von Standardfeldern und sind nicht durch benutzerdefinierte Felder erweiterbar.
`Named Account List Field`

| Name | Datentyp | Aktualisierbar | Hinweise |
|---|---|---|---|
| marketoGUID | Zeichenfolge | Falsch | Eindeutige Zeichenfolgenkennung der benannten Kontoliste. Dieses Feld wird vom System verwaltet und ist beim Erstellen eines Datensatzes nicht als Feld zulässig. Feld, das von &quot;dedupeBy&quot;:&quot;idField&quot; bei der Erstellung oder Aktualisierung verwendet wird. |
| name | Zeichenfolge | Richtig | Name der Liste. Feld, das von &quot;dedupeBy&quot;:&quot;dedupeFields&quot;bei der Erstellung oder Aktualisierung verwendet wird. |
| createdAt | Datum/Uhrzeit | Falsch | Zeitpunkt der Erstellung der Liste. Dieses Feld wird vom System verwaltet und ist beim Erstellen oder Aktualisieren eines Datensatzes nicht als Feld zulässig. |
| updatedAt | Datum/Uhrzeit | Falsch | Zeitpunkt der letzten Aktualisierung der Liste. Dieses Feld wird vom System verwaltet und ist beim Erstellen oder Aktualisieren eines Datensatzes nicht als Feld zulässig. |
| Typ | Zeichenfolge | Falsch | Listentyp. Kann den Wert &quot;default&quot;oder &quot;external&quot;haben. Externe Listen werden von der CRM-Kontoansicht erstellt. |


## Anfrage

Die Abfrage von Kontolisten ist einfach und einfach. Derzeit gibt es nur zwei gültige filterTypes für die Abfrage von benannten Kontolisten: &quot;dedupeFields&quot;und &quot;idField&quot;. Das Feld, nach dem gefiltert werden soll, wird im Parameter `filterType` der Abfrage festgelegt und die Werte werden in `filterValues as` in einer kommagetrennten Liste festgelegt. Die Filter `nextPageToken` und `batchSize` sind ebenfalls optionale Parameter.

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

## Erstellen und Aktualisieren

Das Erstellen und Aktualisieren von benannten Kontolistendatensätzen folgt den festgelegten Mustern für andere Vorgänge zum Erstellen und Aktualisieren von Lead-Datenbanken. Beachten Sie, dass benannte Kontolisten nur ein aktualisierbares Feld, `name`, aufweisen.

Der Endpunkt ermöglicht die beiden standardmäßigen Aktionstypen &quot;createOnly&quot;und &quot;updateOnly&quot;.  Der `action defaults`-Wert &quot;createOnly&quot;.

Das optionale `dedupeBy parameter` kann angegeben werden, wenn die Aktion `updateOnly` ist.  Zulässige Werte sind &quot;dedupeFields&quot;(entspricht &quot;name&quot;) oder &quot;idField&quot;(entspricht &quot;marketoGUID&quot;).  In den Modi `createOnly` ist nur &quot;name&quot;als Feld `dedupeBy` zulässig. Sie können bis zu 300 Datensätze gleichzeitig versenden.

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

Das Löschen von Listen mit benannten Konten ist einfach und kann entweder auf der Basis von `name` oder der `marketoGUID` der Liste erfolgen. Um den Schlüssel auszuwählen, den Sie verwenden möchten, übergeben Sie entweder &quot;dedupeFields&quot;für name oder &quot;idField&quot;für marketoGUID im `deleteB`-Mitglied Ihrer Anfrage. Wenn diese Option deaktiviert ist, wird standardmäßig dedupeFields verwendet. Sie können bis zu 300 Datensätze gleichzeitig löschen.

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

Falls ein Datensatz für einen bestimmten Schlüssel nicht gefunden werden kann, hat das entsprechende Ergebniselement den Wert `status` &quot;skipped&quot;(übersprungen) und einen Grund mit einem Code und einer Meldung, die den Fehler beschreibt, wie im obigen Beispiel gezeigt.

## Verwalten der Mitgliedschaft

### Mitgliedschaft in Abfragen

Die Abfrage der Mitgliedschaft in einer Liste mit benannten Konten ist einfach und erfordert nur den `i` der Kontoliste. Optionale Parameter sind:

-`field` - eine kommagetrennte Liste von Feldern, die in die Antwortdatensätze aufgenommen werden sollen
-`nextPageToke` - für das Paging durch den Ergebnissatz
-`batchSiz` - zur Angabe der Anzahl der zurückzugebenden Datensätze

Wenn `field` nicht festgelegt ist, werden `marketoGUI`, `nam`, `createdA` und `updatedA` zurückgegeben. `batchSiz` hat einen Maximalwert und einen Standardwert von 300.

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

Named-Konten können einfach zu einer Liste mit benannten Konten hinzugefügt werden. Konten dürfen nur mit ihrer marketoGUID hinzugefügt werden. Sie können bis zu 300 Datensätze gleichzeitig hinzufügen.

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

### Mitglieder löschen

Das Entfernen von Datensätzen aus einer Kontoliste hat einen anderen Pfad, aber dieselbe Benutzeroberfläche, die einen `marketoGUI` für jeden Datensatz erfordert, den Sie löschen möchten. Sie können bis zu 300 Datensätze gleichzeitig entfernen.

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

## Timeouts

- Die Endpunkte der benannten Kontoliste haben eine Zeitüberschreitung von 30 s, es sei denn, sie werden unten angegeben.
   - Synchronisieren von benannten Kontolisten: 60 s 
   - Benannte Kontolisten löschen: 60 s 
   - Benannte Kontolisten abrufen: 60 s 
   - Hinzufügen von Mitgliedern der Liste mit benannten Konten: 60 s 
   - Mitglieder der Liste mit benannten Konten entfernen: 60 s 
   - Mitglieder der Liste mit benannten Konten abrufen: 60 s
