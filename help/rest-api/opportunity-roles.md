---
title: Opportunity-Rollen
feature: REST API
description: Verwalten Sie Opportunity-Rollen von Marketo über die REST-API, einschließlich Beschreiben von Abfragen mit zusammengesetzten Deduplizierungsfeldern, Erstellen, Aktualisieren, Löschen, Timeouts und keiner CRM-Synchronisierung.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
TQID: https://experienceleague.adobe.com/aE27mBhsrn-0SO41M-pV5NFjoMq--1Lp-L2TQGL7-8Y
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 254
ht-degree: 0%

---

# Opportunity-Rollen

[Opportunity Roles Endpoint-Referenz](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityRolesUsingGET)

Die Zwischenobjektverknüpfung `opportunityRole` führt zu Opportunities.

Opportunity Role APIs sind nur für Abonnements verfügbar, für die die native CRM-Synchronisierung nicht aktiviert ist.

## beschreiben

Wie bei Opportunities stellt die API einen Describe-Aufruf und CRUD-Vorgänge für Opportunity-Rollen bereit.

```http
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

Die `dedupeFields`- und `searchableFields` unterscheiden sich von Opportunities. `dedupeFields` bietet einen zusammengesetzten Schlüssel, für den `externalOpportunityId`, `leadId` und `role` erforderlich sind. Damit die Datensatzerstellung erfolgreich ist, müssen die Opportunity und der Lead, auf die von den ID-Feldern verwiesen wird, in der Zielinstanz vorhanden sein.

Die `searchableFields` Werte `marketoGUID`, `leadId` und `externalOpportunityId` sind für einzelne Abfragen gültig, die dasselbe Muster wie Opportunities verwenden. Sie können auch eine Abfrage anhand des zusammengesetzten Schlüssels durchführen. Für diese Abfrage ist ein JSON-Objekt erforderlich, das über POST mit dem `_method=GET` Abfrageparameter gesendet wird.

```http
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

Diese Anfrage erzeugt denselben Antworttyp wie eine standardmäßige GET-Abfrage, verwendet jedoch eine andere Anfrageschnittstelle.

## Erstellen und aktualisieren

Opportunity-Rollen erstellen und aktualisieren, indem dieselbe Oberfläche wie Opportunities verwendet wird.

```http
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

Opportunity-Rollen nach Deduplizierungsfeldern oder ID-Feldern löschen. Legen Sie den Parameter deleteBy entweder auf dedupeFields oder auf idField fest. Der Standardwert ist deduplizierte Felder.

Der Anfragetext enthält ein Eingabearray mit Opportunity-Rollen, die gelöscht werden sollen. Jeder Aufruf ermöglicht maximal 300 Opportunity-Rollen.

```http
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

- Opportunity-Rollenendpunkte haben eine Zeitüberschreitung von 30 s, sofern nicht anders angegeben.
- Die maximale Wartezeit von „Opportunity-Rollen synchronisieren“ beträgt 60 Sekunden.
- Opportunity-Rollen löschen hat eine Zeitüberschreitung von 60 Jahren.
