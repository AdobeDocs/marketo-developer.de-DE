---
title: Firmen
feature: REST API
description: Konfigurieren von Unternehmensdaten mit Marketo-APIs.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---

# Firmen

[Firmen-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Firmen stellen die Organisation dar, zu der Lead-Datensätze gehören. Leads werden zu einem Unternehmen hinzugefügt, indem das entsprechende `externalCompanyId` mithilfe der Endpunkte [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) oder [Massenimport von Leads](bulk-lead-import.md) ausgefüllt wird. Nachdem ein Lead zu einer Firma hinzugefügt wurde, können Sie den Lead nicht mehr aus dieser Firma löschen (es sei denn, Sie fügen den Lead einer anderen Firma hinzu). Mit einem Unternehmensdatensatz verknüpfte Leads erben die Werte direkt von einem Unternehmensdatensatz, als ob die Werte im eigenen Datensatz des Leads vorhanden wären.

Unternehmens-APIs sind schreibgeschützt für Abonnements, bei denen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert sind.

## beschreiben

Durch die Beschreibung des Firmenobjekts erhalten Sie alle Informationen, die Sie für die Interaktion mit ihnen benötigen.

```
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Abfrage

Das Muster für [Unternehmen abfragen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) folgt dem Muster der Leads-API mit der zusätzlichen Einschränkung, dass der `filterType` die Felder akzeptiert, die im durchsuchbaren Fields-Array des Aufrufs „Describe Companies“ oder „dedupeFields“ aufgeführt sind.

`filterType` und `filterValues` sind erforderliche Abfrageparameter.  `fields`, `nextPageToken` und `batchSize` sind optionale Parameter.  Die Parameter funktionieren genau wie die entsprechenden Parameter in den Leads- und Opportunities-APIs. Wenn beim Anfordern einer Liste von `fields` ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, ist der Wert impliziert null.

Wenn der Feldparameter weggelassen wird, wird standardmäßig folgender Satz von Feldern zurückgegeben:

- ID
- deduplizierte Felder
- updatedAt
- createdAt

```
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Erstellen und aktualisieren

Der Endpunkt [Unternehmen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) akzeptiert den erforderlichen `input`, der ein Array von Unternehmensobjekten enthält. Genau wie Opportunities gibt es drei Modi zum Erstellen und Aktualisieren von Unternehmen: createOnly, updateOnly und createOrUpdate.  Modi werden im `action` der Anfrage angegeben. Sowohl der Parameter `dedupeBy` als auch der Parameter `action` sind optional und standardmäßig auf die Modi dedupeFields und createOrUpdate festgelegt.

```
POST /rest/v1/companies.json
```

```
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Felder

Das Unternehmensobjekt enthält einen Satz von Feldern. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind Anzeigename, API-Name und Datentyp. Diese Attribute werden zusammen als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder im Unternehmensobjekt abfragen. Diese APIs erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der `Read-Write Schema Standard Field` oder `Read-Write Schema Custom Field` Berechtigungen verfügt.

### Abfragefelder

Die Abfrage von Unternehmensfeldern ist unkompliziert. Sie können ein einzelnes Unternehmensfeld nach API-Namen abfragen oder den Satz aller Unternehmensfelder abfragen.

#### Nach Name

Der Endpunkt [Unternehmensfeld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im Firmenobjekt ab. Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an. Die Antwort ähnelt dem Endpunkt „Firma beschreiben“, enthält jedoch zusätzliche Metadaten wie das `isCustom`, das angibt, ob das Feld ein benutzerdefiniertes Feld ist.

```
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
        {
            "displayName": "Industry",
            "name": "industry",
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
    "success": true
}
```

#### Durchsuchen

Der [Endpunkt Firmenfelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) ruft Metadaten für alle Felder im Firmenobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den `batchSize` Abfrageparameter verwenden, um diese Zahl zu reduzieren. Wenn das Attribut `moreResult` wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

```
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Löschen

Die Löschkriterien werden im `input`-Array angegeben, das eine Liste der Suchwerte enthält.  Die Löschmethode wird im Parameter `deleteBy` angegeben.  Zulässige Werte sind: dedupeFields, idField.  Der Standardwert ist deduplizierte Felder.

```
Content-Type: application/json
```

```
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
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

- Für Unternehmens-Endpunkte gilt eine Zeitüberschreitung von 30 Sekunden, sofern unten nicht anders angegeben
   - Synchronisierungsunternehmen: 60er
   - Firmen löschen: 60er
