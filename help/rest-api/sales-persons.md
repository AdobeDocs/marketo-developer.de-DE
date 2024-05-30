---
title: "Vertriebsmitarbeiter"
feature: REST API
description: "Lesen Sie Daten zu Vertriebsmitarbeitern."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---


# Vertriebsmitarbeiter

[Endpunkt-Referenz für Vertriebsmitarbeiter](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Vertriebsmitarbeiter-APIs bieten schreibgeschützten Zugriff auf Abonnements mit [SFDC-Synchronisierung](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) aktiviert sind. Sales Persons sind eine Art von Personendatensätzen, die zum Verkaufsinhaber von Lead-Datensätzen gehören. Sie beziehen sich auf Lead-Datensätze, die vom Feld externalSalesPersonId für jeden Lead-Datensatz bereitgestellt werden. Wenn ein Lead von einem ausgefüllten Feld externalSalesPersonId mit einer Verkaufsperson verbunden wird, werden die entsprechenden Suchfelder des Lead-Eigentümers für diesen Lead-Datensatz in Marketo ausgefüllt, sodass die entsprechenden Filter und Token verwendet werden können.

Vertriebsmitarbeiter beziehen sich auf Lead-Datensätze, indem sie [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) Endpunkt und Übergabe des Attributs externalSalesPersonId .

Verkaufsmitarbeiter beziehen sich auf Einsendungen von Möglichkeiten, indem sie [Synchronisierungsmöglichkeiten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) Endpunkt und Übergabe des Attributs externalSalesPersonId .

Vertriebsmitarbeiter beziehen sich mithilfe der Variablen [Synchronisierungsunternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) Endpunkt und Übergabe des Attributs externalSalesPersonId .

Datensätze von Vertriebsmitarbeitern können nur über die API bearbeitet werden.

## Beschreibung

Die Beschreibung der Datensätze von Vertriebsmitarbeitern folgt dem Standardmuster für Lead-Datenbankobjekte.

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

Standardmäßig wird die Variable `idField` von Vertriebsmitarbeitern ist &quot;id&quot;und die Variable `dedupeFields` ist nur &quot;externalSalesPersonId&quot;.

## Anfrage

Vertriebsmitarbeiter, die das Standardabfragemuster für einfache Schlüssel verwenden. In diesem Beispiel wird gezeigt, wie die E-Mail des Benutzers als externalSalesPersonId verwendet wird. Standardmäßig gibt die Abfrage alle Felder zurück, die für die zurückgegebenen Datensätze ausgefüllt sind.

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

## Erstellen und Aktualisieren

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

Das Löschmuster ist Standard.

Die Löschung von Vertriebsmitarbeitern ist nicht zulässig, wenn sie &quot;verwendet&quot;werden. In diesem Fall wird die Vertriebsperson übersprungen. Beispiele:

- Wenn Vertriebsmitarbeiter aktiven Leads zugeordnet sind
- Wenn eine Vertriebsperson mit einem gelöschten Unternehmen verknüpft ist

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

## Timeouts

- Endpunkte von Vertriebsmitarbeitern haben eine Zeitüberschreitung von 30 Sekunden, sofern nicht unten angegeben
   - Vertriebsmitarbeiter synchronisieren: 60 s
   - Löschen von Verkaufsmitarbeitern: 60 s
