---
title: Vertriebspersonal
feature: REST API
description: Lesen Sie Daten zu Verkaufspersonen.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Vertriebspersonal

[Endpunkt-Referenz für Vertriebspersonen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Vertriebspersonen-APIs sind schreibgeschützt für Abonnements, bei denen [SFDC Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) aktiviert sind. Verkaufspersonen sind eine Art von Personendatensätzen, die Verkaufseigentümer von Lead-Datensätzen sind. Sie sind durch das Feld externalSalesPersonId für jeden Lead-Datensatz mit Lead-Datensätzen verknüpft. Wenn ein Lead durch ein ausgefülltes externes Feld „SalesPersonId“ mit einer Verkaufsperson verknüpft ist, werden die entsprechenden Suchfelder für den Lead-Inhaber für diesen Lead-Datensatz in Marketo ausgefüllt, sodass die entsprechenden Filter und Token verwendet werden können.

Vertriebspersonen werden mit Lead-Datensätzen verknüpft, indem sie den Endpunkt [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) verwenden und das Attribut externalSalesPersonId übergeben.

Vertriebspersonen werden mit Opportunity-Datensätzen verknüpft, indem sie den Endpunkt [Opportunities synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) verwenden und das Attribut externalSalesPersonId übergeben.

Vertriebspersonen werden mit Firmendatensätzen verknüpft, indem sie den Endpunkt [Unternehmen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) verwenden und das Attribut externalSalesPersonId übergeben.

Die Datensätze von Vertriebspersonen können nur über die API bearbeitet werden.

## beschreiben

Die Beschreibung der Datensätze von Vertriebspersonen folgt dem Standardmuster für Lead-Datenbankobjekte.

```
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

Standardmäßig lautet der `idField` der Vertriebspersonen „id“ und der `dedupeFields` nur „externalSalesPersonId“.

## Abfrage

Vertriebspersonen, die das Standardabfragemuster für einfache Schlüssel verwenden. Dieses Beispiel zeigt, wie die E-Mail-Adresse des Benutzers als externe SalesPersonId verwendet wird. Standardmäßig gibt die Abfrage alle Felder zurück, die für die zurückgegebenen Datensätze ausgefüllt sind.

```
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

Das Muster für Aktualisierungen ist Standard.

```
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

Das Muster für Löschvorgänge ist standardmäßig.

Die Löschung von Vertriebspersonen ist bei „Verwendung“ nicht zulässig. In diesem Fall wird der Vertriebsmitarbeiter übersprungen. Beispiele:

- Wenn Vertriebsperson mit aktiven Leads verknüpft ist
- Wenn ein Vertriebsmitarbeiter mit einer Firma verknüpft ist, die gelöscht wurde

```
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

- Für Personen-Endpunkte im Vertrieb beträgt die Zeitüberschreitung 30 Sekunden, es sei denn, dies wird weiter unten angegeben
   - Vertriebspersonen synchronisieren: 60er
   - Vertriebspersonen löschen: 60er
