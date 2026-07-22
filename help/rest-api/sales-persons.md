---
title: Vertriebspersonal
feature: REST API
description: Marketo-REST-API-Handbuch für Vertriebspersonendatensätze mit SFDC oder Dynamics Sync, unter Verwendung von externalSalesPersonId, um Leads zu verknüpfen und Abfragen, Upsert und Löschen durchzuführen.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
TQID: https://experienceleague.adobe.com/JwLNgM0zgztyoYJotCiSdGxMixnzA0kvkFbvq8kEkzE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 0%

---

# Vertriebspersonal

[Endpunktreferenz für Vertriebspersonen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)

Vertriebspersonen-APIs bieten schreibgeschützten Zugriff für Abonnements, für die [SFDC Sync](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) aktiviert ist.

Vertriebspersonen sind Personendatensätze, die die Vertriebsinhaber von Lead-Datensätzen repräsentieren. Das Feld externalSalesPersonId in jedem Lead-Datensatz bezieht sich auf einen Lead und eine Verkaufsperson. Wenn dieses Feld ausgefüllt wird, füllt Marketo die entsprechenden Suchfelder für Lead-Inhaber im Lead-Datensatz. Sie können dann die zugehörigen Filter und Token verwenden.

Verknüpfen Sie Vertriebspersonen mit anderen Datensätzen, indem Sie das Attribut externalSalesPersonId an den entsprechenden Endpunkt übergeben:

- Lead-Datensätze: [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).
- Opportunity-Datensätze: [Opportunities synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/syncOpportunitiesUsingPOST).
- Firmendatensätze: [Firmen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

Die Datensätze von Vertriebspersonen können nur über die API bearbeitet werden.

## beschreiben

Beschreiben Sie die Datensätze von Vertriebspersonen mithilfe des Standardmusters für Lead-Datenbankobjekte.

```http
GET /rest/v1/salespersons/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[
            "externalSalesPersonId"
         ],
         "searchableFields":[
            [
               "email"
            ],
            [
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[
            {
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

Standardmäßig ist der `idField` „SalesPerson“ „id“ und `dedupeFields` „externalSalesPersonId“.

## Abfrage

Fragen Sie Vertriebspersonen anhand des Standardabfragemusters für einfache Schlüssel ab. Im folgenden Beispiel wird die E-Mail-Adresse des Benutzers als externalSalesPersonId verwendet.

Standardmäßig gibt die Abfrage alle ausgefüllten Felder für die entsprechenden Datensätze zurück.

```http
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Erstellen und aktualisieren

Erstellen oder aktualisieren Sie Vertriebspersonen mithilfe des standardmäßigen Aktualisierungsmusters.

```http
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status": "updated",
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Löschen

Löschen Sie Vertriebspersonen mithilfe des standardmäßigen Löschmusters.

Sie können eine Verkaufsperson, die „in Gebrauch“ ist, nicht löschen. In folgenden Fällen wird der Vertriebsmitarbeiter durch die Anfrage übersprungen:

- Die Vertriebsperson ist mit aktiven Leads verknüpft.
- Der Verkäufer ist mit einer Firma verknüpft, die gelöscht wurde.

```http
POST /rest/v1/salespersons/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com"
      },
      {
         "externalSalesPersonId":"david@test.com"
      },
      {
         "externalSalesPersonId":"raj@test.com"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Zeitüberschreitungen

- Für Personen-Endpunkte im Vertrieb beträgt die Zeitüberschreitung 30 Sekunden, sofern nicht anders angegeben.
- Für „Vertriebspersonen synchronisieren“ gilt eine Zeitüberschreitung von 60 Jahren.
- Für „Vertriebspersonen löschen“ beträgt die Zeitüberschreitung 60 Jahre.
