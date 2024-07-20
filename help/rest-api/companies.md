---
title: Unternehmen
feature: REST API
description: Konfigurieren von Unternehmensdaten mit Marketo-APIs.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---

# Unternehmen

[Endpunktverweis für Unternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Unternehmen repräsentieren die Organisation, zu der Lead-Datensätze gehören. Leads werden zu einem Unternehmen hinzugefügt, indem das entsprechende `externalCompanyId` -Feld mit den Endpunkten [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) oder [Massen-Lead-Import](bulk-lead-import.md) gefüllt wird. Nachdem einem Unternehmen ein Lead hinzugefügt wurde, können Sie den Lead aus diesem Unternehmen nicht löschen (es sei denn, Sie fügen den Lead zu einem anderen Unternehmen hinzu). Leads, die mit einem Unternehmensdatensatz verknüpft sind, erben direkt die Werte aus einem Unternehmensdatensatz, als ob die Werte im eigenen Datensatz des Leads vorhanden wären.

Unternehmens-APIs sind schreibgeschützt für Abonnements, für die die Funktion [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder die Option [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert ist.

## Beschreibung

Durch die Beschreibung des Objekt company erhalten Sie alle Informationen, die Sie benötigen, um mit ihnen zu interagieren.

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

## Anfrage

Das Muster für [Abfragen von Unternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) ähnelt dem der Lead-API mit der hinzugefügten Einschränkung, dass der Parameter `filterType` die im Array searchableFields des Aufrufs Describe Companies oder dedupeFields aufgelisteten Felder akzeptiert.

`filterType` und `filterValues` sind erforderliche Abfrageparameter.  `fields`, `nextPageToken` und `batchSize` sind optionale Parameter.  Die Parameter funktionieren genau wie die entsprechenden Parameter in den Leads- und Opportunities-APIs. Wenn bei der Anforderung einer Liste von `fields` ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

Wenn der Feldparameter weggelassen wird, wird standardmäßig der Feldsatz zurückgegeben:

- ID
- dedupeFields
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

## Erstellen und Aktualisieren

Der Endpunkt [Unternehmen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) akzeptiert den erforderlichen `input` -Parameter, der ein Array von Unternehmensobjekten enthält. Genau wie bei Gelegenheiten gibt es drei Modi zum Erstellen und Aktualisieren von Unternehmen: createOnly, updateOnly und createOrUpdate.  Die Modi werden im Parameter `action` der Anfrage angegeben. Sowohl die Parameter `dedupeBy` als auch `action` sind optional und standardmäßig den Modi dedupeFields und createOrUpdate .

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

Das Objekt company enthält eine Reihe von Feldern. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind der Anzeigename, der API-Name und der dataType. Diese Attribute werden kollektiv als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder für das Unternehmensobjekt abfragen. Diese APIs erfordern, dass der Eigentümer-API-Benutzer über eine oder beide der Berechtigungen `Read-Write Schema Standard Field` oder `Read-Write Schema Custom Field` verfügt.

### Abfragefelder

Die Abfrage von Unternehmensfeldern ist unkompliziert. Sie können ein einzelnes Unternehmensfeld nach API-Namen abfragen oder die Gruppe aller Unternehmensfelder abfragen.

#### Nach Name

Der Endpunkt [Unternehmensfeld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im Unternehmensobjekt ab. Der erforderliche Pfadparameter `fieldApiName` gibt den API-Namen des Felds an. Die Antwort entspricht dem Endpunkt Unternehmen beschreiben , enthält jedoch zusätzliche Metadaten wie das Attribut `isCustom` , das angibt, ob es sich bei dem Feld um ein benutzerdefiniertes Feld handelt.

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

Der Endpunkt [Unternehmensfelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) ruft Metadaten für alle Felder im Unternehmensobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den Abfrageparameter `batchSize` verwenden, um diese Zahl zu reduzieren. Wenn das Attribut `moreResult` &quot;true&quot;ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

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

Die Löschkriterien werden im Array `input` angegeben, das eine Liste von Suchwerten enthält.  Die Löschmethode wird im Parameter `deleteBy` angegeben.  Zulässige Werte sind: dedupeFields, idField.  Der Standardwert ist dedupeFields.

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

## Timeouts

- Firmen-Endpunkte haben eine Zeitüberschreitung von 30 Sekunden, sofern nicht unten angegeben
   - Synchronisierungsunternehmen: 60 s 
   - Unternehmen löschen: 60 s
