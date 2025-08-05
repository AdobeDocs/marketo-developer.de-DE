---
title: Opportunitys
feature: REST API
description: ' Konfigurieren von Opportunities mit der Marketo-API'
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Opportunitys

[Opportunity-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo stellt APIs zum Lesen, Schreiben, Erstellen und Aktualisieren von Opportunity-Datensätzen bereit. In Marketo werden Opportunity-Datensätze über das dazwischenliegende Opportunity-Rollenobjekt mit Lead- und Kontaktdatensätzen verknüpft, sodass eine Opportunity mit vielen einzelnen Leads verknüpft werden kann.  Beide Objekttypen werden über die API verfügbar gemacht. Wie die meisten Objekttypen der Lead-Datenbank verfügen beide über einen entsprechenden Describe-Aufruf, der Metadaten über die Objekttypen zurückgibt.

Opportunity-APIs sind schreibgeschützt für Abonnements, bei denen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert sind.

## beschreiben

Die Beschreibung der Opportunity-Datensätze folgt dem Standardmuster für Lead-Datenbankobjekte.

```
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

Die wichtigsten Felder für diesen Antworttyp sind `idField`, `dedupeFields` und `searchableFields`.  idField gibt den Primärschlüssel für Opportunities marketoGUID an.  Dies ist ein vom System generierter eindeutiger Schlüssel, der für Lese- und Aktualisierungsvorgänge verwendet werden kann, jedoch nicht für Einfügungen, da er vom System verwaltet wird.  Das Array dedupeFields gibt an, welche Felder gültige Schlüssel für Einfügevorgänge sind. Im Fall von Opportunitys ist dies nur externalOpportunityId.  Das searchableFields-Array liefert die Menge der Felder, die für die Abfrage gültig sind, externalOpportunityId und marketoGUID.

## Abfrage

Das Muster für [Opportunities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) folgt dem der Leads-API mit der zusätzlichen Einschränkung, dass der `filterType` die im `searchableFields`-Array aufgelisteten Felder oder den entsprechenden Describe-Aufruf akzeptiert, oder dedupeFields.  Beachten Sie, dass bei der Verwendung benutzerdefinierter Opportunity-Felder nur benutzerdefinierte Opportunity-Felder vom Typ Zeichenfolge oder Ganzzahl im Array „SearchableFields“ aufgeführt werden.

```
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

Sie können auch die optionalen Abfrageparameter `fields` für die Rückgabe zusätzlicher Opportunity-Felder `nextPageToken` für Paging-Durchläufe einbeziehen, die größer sind als die Batch-Größe, `batchSize`, die standardmäßig auf eingestellt ist und maximal 300 beträgt.  Wenn beim Anfordern einer Liste von `fields` ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, ist der Wert impliziert null.

## Erstellen und aktualisieren

Opportunities können dem Muster der Leads-API mit einigen Einschränkungen genau folgen.  Die für `action` verfügbaren Werte sind: createOnly, createOrUpdate und updateOnly.  Bei Verwendung des Modus createOnly oder createOrUpdate muss in jedem Datensatz das Feld externalOpportunityId enthalten sein.  Für den updateOnly-Modus kann entweder marketoGUID oder externalOpportunityId verwendet werden.  Der Modus verwendet standardmäßig createOrUpdate , wenn kein Wert angegeben wird.

Der `lookupField` Parameter aus der Leads-API ist nicht verfügbar und wird durch den dedupeBy-Parameter ersetzt, der nur gültig ist, wenn action auf updateOnly festgelegt ist.  Die für dedupeBy verfügbaren Werte sind entweder „dedupeFields“ oder „idField“, die durch den Describe-Aufruf als externalOpportunityId bzw. marketoGUID angegeben werden.  Wenn dedupeBy nicht angegeben ist, wird standardmäßig der Modus dedupeFields verwendet.  Das Feld „Name“ darf nicht null sein.

Sie können bis zu 300 Datensätze gleichzeitig senden.

```
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

Die API antwortet mit dem `marketoGUID` für jeden Datensatz, sowie mit einem `status` Feld, das den individuellen Erfolg oder Misserfolg jedes Datensatzes angibt, und einem `seq` Feld, das verwendet wird, um die gesendeten Datensätze mit der Reihenfolge der Antwort zu korrelieren.  Die Zahl im Feld ist der Index des Datensatzes, der in der Anfrage gesendet wurde.

### Felder

Das Unternehmensobjekt enthält einen Satz von Feldern.  Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben.  Beispiele für Attribute sind Anzeigename, API-Name und Datentyp.  Diese Attribute werden zusammen als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder im Unternehmensobjekt abfragen. Diese APIs erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der `Read-Write Schema Standard Field` oder `Read-Write Schema Custom Field` Berechtigungen verfügt.

### Abfragefelder

Die Abfrage von Opportunity-Feldern ist unkompliziert.  Sie können ein einzelnes Unternehmensfeld nach API-Namen abfragen oder den Satz aller Unternehmensfelder abfragen.

#### Nach Name

Der Endpunkt [Feld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im Firmenobjekt ab.  Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an.  Die Antwort ähnelt dem Opportunity-Endpunkt, enthält jedoch zusätzliche Metadaten wie das `isCustom`, das angibt, ob das Feld ein benutzerdefiniertes Feld ist.

```
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

Der Endpunkt [Chancen-Felder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) ruft Metadaten für alle Felder im Unternehmensobjekt ab.  Standardmäßig werden maximal 300 Datensätze zurückgegeben.  Sie können den `batchSize` Abfrageparameter verwenden, um diese Zahl zu reduzieren.  Wenn das Attribut `moreResult` wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind.  Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind.  Die von dieser API zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

```
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

Opportunities können nach Deduplizierungsfeldern oder ID-Feldern gelöscht werden. Geben Sie mithilfe des `deleteBy`-Parameters mit dem Wert dedupeFields oder idField an. Wenn kein Wert angegeben ist, lautet der Standardwert dedupeFields. Der Anfragetext enthält eine `input` Reihe von Gelegenheiten zum Löschen. Pro Anruf sind maximal 300 Opportunities zulässig.

```
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

- Opportunity-Endpunkte haben eine Zeitüberschreitung von 30 s, sofern unten nicht anders angegeben
   - Synchronisationsmöglichkeiten: 60er
   - Opportunities löschen: 60er
