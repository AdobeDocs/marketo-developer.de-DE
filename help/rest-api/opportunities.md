---
title: Opportunitys
feature: REST API
description: " Konfigurieren Sie die Möglichkeiten mit der Marketo-API."
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Opportunitys

[Opportunity Endpoint Reference](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo stellt APIs zum Lesen, Schreiben, Erstellen und Aktualisieren von Opportunity-Datensätzen bereit. In Marketo werden Opportunity-Datensätze mit Lead- und Kontaktdatensätzen über das Intermediär-Opportunity-Rollenobjekt verknüpft, sodass eine Chance mit vielen einzelnen Leads verknüpft werden kann.  Beide Objektarten werden über die API verfügbar gemacht. Wie die meisten Objekttypen der Lead-Datenbank verfügen beide über einen entsprechenden Deskriptionsaufruf, der Metadaten zu den Objekttypen zurückgibt.

Opportunity-APIs sind schreibgeschützter Zugriff für Abonnements, für die die Funktion [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert ist.

## Beschreibung

Die Beschreibung von Opportunity-Datensätzen folgt dem Standardmuster für Lead-Datenbank-Objekte.

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

Die wichtigsten Felder für diesen Antworttyp sind `idField`, `dedupeFields` und `searchableFields`.  idField gibt den Primärschlüssel für Chancen an, marketoGUID.  Dies ist ein vom System generierter eindeutiger Schlüssel, der für Lese- und Aktualisierungsvorgänge, aber nicht für Einfügungen verwendet werden kann, da er vom System verwaltet wird.  Das dedupeFields-Array gibt an, welche Felder gültige Schlüssel für Einfügevorgänge sind. Bei Gelegenheiten ist dies nur externalOpportunityId.  Das searchableFields -Array gibt Ihnen die für die Abfrage gültigen Felder, externalOpportunityId und marketoGUID an.

## Anfrage

Das Muster für [ Abfragen von Möglichkeiten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) ähnelt dem der Leads-API mit der hinzugefügten Einschränkung, dass der `filterType`-Parameter die im Array `searchableFields` aufgelisteten Felder oder des entsprechenden Beschreibungsaufrufs oder dedupeFields akzeptiert.  Beachten Sie, dass bei der Verwendung benutzerdefinierter Opportunity-Felder nur benutzerdefinierte Opportunity-Felder vom Typ String oder Integer im Array searchableFields aufgeführt werden.

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

Sie können auch die optionalen Abfrageparameter `fields` einbeziehen, um zusätzliche Opportunitätsfelder, `nextPageToken`, für Paging durch Sets, die größer als die Batch-Größe sind, `batchSize` zurückzugeben, die standardmäßig auf eingestellt sind und eine maximale Größe von 300 haben.  Wenn bei der Anforderung einer Liste von `fields` ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

## Erstellen und Aktualisieren

Chancen folgen dem Leads-API-Muster genau, mit einigen Einschränkungen.  Die für `action` verfügbaren Werte sind: createOnly, createOrUpdate und updateOnly.  Bei Verwendung des Modus createOnly oder createOrUpdate muss in jedem Datensatz das Feld externalOpportunityId enthalten sein.  Für den Modus updateOnly können entweder marketoGUID oder externalOpportunityId verwendet werden.  Im Modus wird standardmäßig createOrUpdate verwendet, falls nicht anders angegeben.

Der Parameter `lookupField` aus der Lead-API ist nicht verfügbar und wird durch den Parameter dedupeBy ersetzt, der nur gültig ist, wenn die Aktion updateOnly lautet.  Die für dedupeBy verfügbaren Werte sind entweder &quot;dedupeFields&quot;oder &quot;idField&quot;, die durch den Beschreibungsaufruf als externalOpportunityId bzw. marketoGUID angegeben werden.  Wenn dedupeBy nicht angegeben ist, wird standardmäßig der dedupeFields -Modus verwendet.  Das Feld &quot;name&quot;darf nicht null sein.

Sie können bis zu 300 Datensätze gleichzeitig versenden.

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

Die API antwortet mit dem Wert `marketoGUID` für jeden Datensatz sowie einem Feld `status`, das den Erfolg oder Misserfolg jedes Datensatzes angibt, und einem Feld `seq` , das zur Korrelation der gesendeten Datensätze mit der Reihenfolge der Antwort verwendet wird.  Die Zahl im Feld ist der Index des in der Anfrage gesendeten Datensatzes.

### Felder

Das Objekt company enthält eine Reihe von Feldern.  Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben.  Beispiele für Attribute sind der Anzeigename, der API-Name und der dataType.  Diese Attribute werden kollektiv als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder für das Unternehmensobjekt abfragen. Diese APIs erfordern, dass der Eigentümer-API-Benutzer über eine oder beide der Berechtigungen `Read-Write Schema Standard Field` oder `Read-Write Schema Custom Field` verfügt.

### Abfragefelder

Die Abfrage von Opportunitätsfeldern ist unkompliziert.  Sie können ein einzelnes Unternehmensfeld nach API-Namen abfragen oder die Gruppe aller Unternehmensfelder abfragen.

#### Nach Name

Der Endpunkt [Opportunity Field by Name](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im Unternehmensobjekt ab.  Der erforderliche Pfadparameter `fieldApiName` gibt den API-Namen des Felds an.  Die Antwort entspricht dem Endpunkt &quot;Chancen beschreiben&quot;, enthält jedoch zusätzliche Metadaten wie das Attribut `isCustom` , das angibt, ob es sich bei dem Feld um ein benutzerdefiniertes Feld handelt.

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

Der Endpunkt [Opportunity Fields](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) ruft Metadaten für alle Felder im Unternehmensobjekt ab.  Standardmäßig werden maximal 300 Datensätze zurückgegeben.  Sie können den Abfrageparameter `batchSize` verwenden, um diese Zahl zu reduzieren.  Wenn das Attribut `moreResult` &quot;true&quot;ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind.  Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind.  Die von dieser API zurückgegebene `nextPageToken` sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

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

Sie können Chancen durch Deduplizierungsfelder oder ID-Feld löschen. Geben Sie die Verwendung des Parameters `deleteBy` mit dem Wert dedupeFields oder idField an. Wenn kein Wert angegeben wird, ist der Standardwert dedupeFields. Der Anfrageinhalt enthält eine `input` -Gruppe von Löschmöglichkeiten. Pro Anruf sind maximal 300 Möglichkeiten zulässig.

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

## Timeouts

- Opportunity-Endpunkte haben eine Zeitüberschreitung von 30 Sekunden, sofern nicht unten angegeben
   - Synchronisierungsmöglichkeiten: 60 s 
   - Löschmöglichkeiten: 60 s
