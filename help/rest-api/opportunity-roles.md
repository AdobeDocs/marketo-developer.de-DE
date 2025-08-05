---
title: Opportunity-Rollen
feature: REST API
description: Umgang mit Opportunity-Rollen in Marketo.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Opportunity-Rollen

[Endpunkt-Referenz zu Opportunity-Rollen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Leads sind mit Opportunities über das Zwischenobjekt `opportunityRole` verknüpft.

Opportunity Role APIs werden nur für Abonnements bereitgestellt, für die keine native CRM-Synchronisierung aktiviert ist.

## beschreiben

Wie bei Opportunities werden Aufrufe beschreiben und CRUD-Vorgänge für Opportunity-Rollen verfügbar gemacht.

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

## Abfrage

Beachten Sie, dass sich sowohl `dedupeFields` als auch `searchableFields` in gewisser Weise von Opportunities unterscheiden. `dedupeFields` stellt einen zusammengesetzten Schlüssel bereit, für den alle drei `externalOpportunityId`, `leadId` und `role` erforderlich sind. Sowohl die Opportunity- als auch die Lead-Relation nach den ID-Feldern müssen in der Zielinstanz vorhanden sein, damit die Datensatzerstellung erfolgreich ist. `searchableFields` sind `marketoGUID`, `leadId` und `externalOpportunityId` alle für Abfragen allein gültig und verwenden ein Muster, das mit Opportunities identisch ist. Es gibt jedoch eine zusätzliche Option, den zusammengesetzten Schlüssel für die Abfrage zu verwenden, was erfordert, dass ein JSON-Objekt über POST mit dem zusätzlichen Abfrageparameter `_method=GET` übermittelt wird.

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

Dadurch wird derselbe Antworttyp erzeugt wie bei einer standardmäßigen GET-Abfrage, sie weist einfach eine andere Oberfläche für die Anfrage auf.

## Erstellen und aktualisieren

Opportunity-Rollen haben dieselbe Oberfläche zum Erstellen und Aktualisieren von Datensätzen wie Opportunities.

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

Opportunity-Rollen können nach Deduplizierungsfeldern oder ID-Feldern gelöscht werden. Geben Sie mithilfe des Parameters deleteBy mit dem Wert dedupeFields oder idField an. Wenn kein Wert angegeben ist, lautet der Standardwert dedupeFields. Der Anfragetext enthält ein Eingabearray mit Opportunity-Rollen, die gelöscht werden sollen. Pro Aufruf sind maximal 300 Opportunity-Rollen zulässig.

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

## Zeitüberschreitungen

- Opportunity-Rollenendpunkte haben eine Zeitüberschreitung von 30 s, sofern unten nicht anders angegeben
   - Opportunity-Rollen synchronisieren: 60er
   - Opportunity-Rollen löschen: 60er
