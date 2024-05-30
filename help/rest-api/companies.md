---
title: "Unternehmen"
feature: REST API
description: "Konfigurieren von Unternehmensdaten mit Marketo-APIs."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---


# Unternehmen

[Endpunktverweis für Unternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Unternehmen repräsentieren die Organisation, zu der Lead-Datensätze gehören. Leads werden zu einem Unternehmen hinzugefügt, indem die entsprechenden `externalCompanyId` Feld verwenden [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) oder [Import von Bulk-Lead](bulk-lead-import.md) -Endpunkte. Nachdem einem Unternehmen ein Lead hinzugefügt wurde, können Sie den Lead aus diesem Unternehmen nicht löschen (es sei denn, Sie fügen den Lead zu einem anderen Unternehmen hinzu). Leads, die mit einem Unternehmensdatensatz verknüpft sind, erben direkt die Werte aus einem Unternehmensdatensatz, als ob die Werte im eigenen Datensatz des Leads vorhanden wären.

Firmen-APIs bieten schreibgeschützten Zugriff für Abonnements mit [SFDC-Synchronisierung](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert sind.

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

Das Muster für [Abfrage von Unternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) folgt eng mit der Leads-API mit der hinzugefügten Einschränkung, dass die `filterType` akzeptiert die Felder, die im Array searchableFields des Aufrufs Describe Companies oder dedupeFields aufgeführt sind.

`filterType` und `filterValues` sind erforderliche Abfrageparameter.  `fields`, `nextPageToken`, und `batchSize` sind optionale Parameter.  Die Parameter funktionieren genau wie die entsprechenden Parameter in den Leads- und Opportunities-APIs. Bei der Anforderung einer Liste von `fields`Wenn ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

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

Die [Synchronisierungsunternehmen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) Endpunkt akzeptiert die erforderlichen `input` -Parameter, der ein Array von Unternehmensobjekten enthält. Genau wie bei Gelegenheiten gibt es drei Modi zum Erstellen und Aktualisieren von Unternehmen: createOnly, updateOnly und createOrUpdate.  Die Modi werden im Abschnitt `action` -Parameter der Anfrage. Beide `dedupeBy` und `action` -Parameter sind optional und standardmäßig den Modi dedupeFields und createOrUpdate .

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

Mit den folgenden Endpunkten können Sie Felder für das Unternehmensobjekt abfragen. Diese APIs erfordern, dass der Eigentümer-API-Benutzer eine Rolle mit einer oder beiden der `Read-Write Schema Standard Field` oder `Read-Write Schema Custom Field` Berechtigungen.

### Abfragefelder

Die Abfrage von Unternehmensfeldern ist unkompliziert. Sie können ein einzelnes Unternehmensfeld nach API-Namen abfragen oder die Gruppe aller Unternehmensfelder abfragen.

#### Nach Name

Die [Unternehmensfeld nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) -Endpunkt ruft Metadaten für ein einzelnes Feld im Unternehmensobjekt ab. Die erforderlichen `fieldApiName` path parameter gibt den API-Namen des Felds an. Die Antwort entspricht dem Endpunkt Unternehmen beschreiben , enthält jedoch zusätzliche Metadaten wie die `isCustom` -Attribut, das angibt, ob das Feld ein benutzerdefiniertes Feld ist.

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

Die [Abrufen von Unternehmensfeldern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) -Endpunkt ruft Metadaten für alle Felder im Unternehmensobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können die `batchSize` -Abfrageparameter, um diese Zahl zu reduzieren. Wenn die Variable `moreResult` auf &quot;true&quot;gesetzt ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die `nextPageToken` von dieser API zurückgegebenen Daten sollten immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

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

Die Löschkriterien werden im Abschnitt `input` -Array, das eine Liste von Suchwerten enthält.  Die Löschmethode wird im Abschnitt `deleteBy` -Parameter.  Zulässige Werte sind: dedupeFields, idField.  Der Standardwert ist dedupeFields.

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
