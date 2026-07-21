---
title: Opportunitys
feature: REST API
description: Marketo-REST-API zum Beschreiben, Abfragen, Erstellen und Aktualisieren von Opportunities, Deduplizierung und durchsuchbaren Feldern, Einschränkungen und schreibgeschütztem Verhalten bei der SFDC- oder Dynamics-Synchronisierung.
exl-id: 46451285-4125-4857-890a-575069a68288
TQID: https://experienceleague.adobe.com/rBDJcXWQrN5qyKRWHyzVC-sc9BH2mQFLm7fKUk-NUn8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 708
ht-degree: 0%

---

# Opportunitys

[Opportunity-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

Marketo bietet APIs zum Lesen, Schreiben, Erstellen und Aktualisieren von Opportunity-Datensätzen. In Marketo verknüpft das Zwischenobjekt Opportunity-Rolle Opportunity-Datensätze mit Lead- und Kontakt-Datensätzen. Eine Opportunity kann daher mit vielen individuellen Leads verknüpft werden.

Die -API macht beide Objekttypen verfügbar. Wie die meisten Lead-Datenbank-Objekttypen verfügt jeder über einen entsprechenden Describe-Aufruf, der Objektmetadaten zurückgibt.

Opportunity-APIs bieten schreibgeschützten Zugriff für Abonnements, bei denen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert ist.

## beschreiben

Beschreiben Sie Opportunity-Datensätze anhand des Standardmusters für Lead-Datenbankobjekte.

```http
GET /rest/v1/opportunities/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId"
         ],
         "searchableFields":[
            [
               "externalOpportunityId"
            ],
            [
               "marketoGUID"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

Die wichtigsten Antwortfelder sind:

- `idField`: Identifiziert den Opportunity-Primärschlüssel, marketoGUID. Dieser systemgenerierte Schlüssel unterstützt Lese- und Aktualisierungsvorgänge, jedoch keine Einfügungen.
- `dedupeFields`: Identifiziert gültige Schlüssel für Einfügevorgänge. Für Opportunitys ist der einzige Schlüssel externalOpportunityId.
- `searchableFields`: Gibt Felder an, die für Abfragen gültig sind. Diese Felder sind externalOpportunityId und marketoGUID.

## Abfrage

Das Muster für [Abfrage von Opportunities](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunitiesUsingGET) folgt der Leads-API. Der `filterType` akzeptiert jedoch nur Felder, die im `searchableFields`-Array der entsprechenden Describe-Antwort oder dedupeFields aufgeführt sind.

Bei benutzerdefinierten Opportunity-Feldern werden nur Felder des Typs „String“ oder „Integer“ im Array „searchableFields“ angezeigt.

```http
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

Sie können die folgenden optionalen Abfrageparameter einbeziehen:

- `fields`: Gibt zusätzliche Opportunity-Felder zurück.
- `nextPageToken`: Seiten durch Ergebnismengen, die größer als die Stapelgröße sind.
- `batchSize`: Gibt die Stapelgröße an. Der Standard- und Höchstwert ist 300.

Wenn Sie eine Liste von `fields` anfordern, hat ein angefordertes Feld, das nicht zurückgegeben wird, einen impliziten Wert von null.

## Erstellen und aktualisieren

Opportunitys folgen dem Leads-API-Muster mit einigen Einschränkungen. Die `action` Werte sind createOnly, createOrUpdate und updateOnly.

- Fügen Sie für den createOnly- oder createOrUpdate-Modus das Feld externalOpportunityId in jeden Datensatz ein.
- Verwenden Sie für den updateOnly-Modus entweder marketoGUID oder externalOpportunityId.
- Wenn kein Wert angegeben ist, wird für den Modus standardmäßig createOrUpdate verwendet.

Der `lookupField` Parameter in der Leads-API ist nicht verfügbar. Der dedupeBy-Parameter ersetzt ihn und ist nur gültig, wenn action auf updateOnly festgelegt ist.

Die dedupeBy-Werte sind „dedupeFields“ und „idField“, die von der Antwort als externalOpportunityId bzw. marketoGUID bezeichnet werden. Wenn dedupeBy nicht angegeben ist, wird standardmäßig der Modus dedupeFields verwendet. Das Feld „Name“ darf nicht null sein.

Sie können bis zu 300 Datensätze gleichzeitig senden.

```http
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

Die Antwort enthält die folgenden Werte für jeden Datensatz:

- `marketoGUID`: Die Datensatzkennung.
- `status`: Erfolg oder Misserfolg des einzelnen Datensatzes.
- `seq`: Der Index des gesendeten Datensatzes, der den Anfragedatensatz mit der Antwortreihenfolge korreliert.

### Felder

Das Unternehmensobjekt enthält Felder, die durch Attribute wie Anzeigename, API-Name und Datentyp definiert sind. Zusammen werden diese Attribute als Metadaten bezeichnet.

Die folgenden Endpunkte geben Abfragefelder für das Unternehmensobjekt an. Der API-Benutzer muss über eine Rolle mit der Berechtigung `Read-Write Schema Standard Field`, der Berechtigung `Read-Write Schema Custom Field` oder beidem verfügen.

### Abfragefelder

Abfragen eines Unternehmensfelds nach API-Namen oder Abrufen aller Unternehmensfelder.

#### Nach Name

Der Endpunkt [Feld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) ruft Metadaten für ein Feld im Firmenobjekt ab. Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an.

Die Antwort ähnelt der Antwort von „Opportunity beschreiben“, enthält jedoch zusätzliche Metadaten. Beispielsweise gibt das `isCustom`-Attribut an, ob das Feld benutzerdefiniert ist.

```http
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Durchsuchen

Der Endpunkt [Chancen-Felder abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldsUsingGET) ruft Metadaten für alle Felder im Unternehmensobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Verwenden Sie den `batchSize` Abfrageparameter, um diese Zahl zu reduzieren.

Wenn das `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Fahren Sie mit dem Aufruf des Endpunkts mit dem zurückgegebenen `nextPageToken` fort, bis moreResult den Wert „false“ hat.

```http
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Stage",
            "name": "stage",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Löschen

Opportunities nach Deduplizierungsfeldern oder ID-Feldern löschen. Legen Sie den `deleteBy` Parameter entweder auf dedupeFields oder auf idField fest. Der Standardwert ist deduplizierte Felder.

Der Anfragetext enthält eine `input` Reihe von Gelegenheiten zum Löschen. Jeder Aufruf ermöglicht maximal 300 Gelegenheiten.

```http
POST /rest/v1/opportunities/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000"
      },
      {
         "externalOpportunityId":"29UYA31581L000000"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Zeitüberschreitungen

- Opportunity-Endpunkte haben eine Zeitüberschreitung von 30 s, sofern nicht anders angegeben.
- Synchronisierungsmöglichkeiten haben eine Zeitüberschreitung von 60 Jahren.
- Opportunities-Löschung hat eine Zeitüberschreitung von 60 Jahren.
