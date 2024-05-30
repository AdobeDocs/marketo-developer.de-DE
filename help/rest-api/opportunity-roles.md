---
title: "Opportunity Roles"
feature: REST API
description: "Umgang mit Opportunitätsrollen in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---


# Opportunity Roles

[Endpunktverweis zu Opportunity Roles](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Leads sind mit Chancen über das Zwischenprodukt verbunden `opportunityRole` -Objekt.

Die APIs für Angebotsrollen werden nur für Abonnements verfügbar gemacht, für die keine native CRM-Synchronisierung aktiviert ist.

## Beschreibung

Wie bei Gelegenheiten werden ein beschreibender Aufruf und CRUD-Vorgänge für Opportunitätsrollen offen gelegt.

```
GET /rest/v1/opportunities/roles/describe.json
```

```json
{  
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[  
      {  
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[  
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[  
            [  
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [  
               "marketoGUID"
            ],
            [  
               "leadId"
            ],
            [  
               "externalOpportunityId"
            ]
         ],
         "fields":[  
            {  
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {  
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {  
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {  
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {  
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {  
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Anfrage

Beachten Sie, dass beide `dedupeFields` und `searchableFields` unterscheiden sich etwas von Chancen. `dedupeFields` stellt tatsächlich einen zusammengesetzten Schlüssel bereit, bei dem alle drei `externalOpportunityId`, `leadId`, und `role` sind erforderlich. Sowohl die Opportunity- als auch die Lead-Verknüpfung der ID-Felder müssen in der Zielinstanz vorhanden sein, damit die Erstellung von Datensätzen erfolgreich ist. Für `searchableFields`, `marketoGUID`, `leadId`, und `externalOpportunityId` sind alle für Abfragen einzeln gültig und verwenden ein dem Opportunities identisches Muster. Es gibt jedoch eine zusätzliche Option, den zusammengesetzten Schlüssel für die Abfrage zu verwenden. Dies erfordert die Übermittlung eines JSON-Objekts über die POST mit dem zusätzlichen Abfrageparameter `_method=GET`.

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{  
   "filterType": "dedupeFields",
   "fields": [  
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [  
      {  
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {  
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {  
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Dadurch wird der gleiche Typ von Antwort wie eine Standardabfrage erzeugt, es verfügt lediglich über eine andere Benutzeroberfläche für die Anforderung.

## Erstellen und Aktualisieren

Opportunity-Rollen verfügen über die gleiche Oberfläche zum Erstellen und Aktualisieren von Datensätzen als Möglichkeiten.

```
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {  
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Löschen

Sie können Opportunity-Rollen nach Deduplizierungsfeldern oder ID-Feldern löschen. Geben Sie die Verwendung des Parameters deleteBy mit dem Wert dedupeFields oder idField an. Wenn kein Wert angegeben wird, ist der Standardwert dedupeFields. Der Anfrageinhalt enthält eine Eingabe-Gruppe von Opportunity-Rollen zum Löschen. Pro Aufruf sind maximal 300 Opportunity-Rollen zulässig.

```
POST /rest/v1/opportunities/roles/delete.json
```

```json
{  
   "deleteBy": "dedupeFields",
   "input": [  
      {  
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Timeouts

- Opportunity Role endpoints haben eine Zeitüberschreitung von 30 s, es sei denn, unten angegeben
   - Rollen für Synchronisierungsoptionen: 60 s 
   - Löschen von Opportunity-Rollen: 60 s
