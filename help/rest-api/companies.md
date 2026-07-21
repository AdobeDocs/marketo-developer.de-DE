---
title: Firmen
feature: REST API
description: Verwenden Sie die Marketo Companies REST-API, um Firmendatensätze zu beschreiben, abzufragen und zu synchronisieren, Felder und Deduplizierungen nach externalCompanyId zu verwalten und CRM-Synchronisierung schreibgeschützt zu notieren.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
TQID: https://experienceleague.adobe.com/LdJYN4lx9JfcE-02zTz8ktfYXm4EdPtxMYOx9gGR0sg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 582
ht-degree: 1%

---

# Firmen

[Companies Endpoint-Referenz](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

Firmen stellen die Organisationen dar, denen Lead-Datensätze angehören. Um einen Lead zu einem Unternehmen hinzuzufügen, füllen Sie sein `externalCompanyId` mithilfe der Endpunkte [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) oder [Massenimport von Leads](bulk-lead-import.md) aus.

Sie können einen Lead nur dann aus einer Firma entfernen, wenn Sie den Lead einer anderen Firma hinzufügen. Leads, die mit einem Firmendatensatz verknüpft sind, übernehmen Werte aus diesem Datensatz, als ob die Werte im Lead-Datensatz vorhanden wären.

Unternehmens-APIs bieten schreibgeschützten Zugriff für Abonnements, bei denen [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) oder [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) aktiviert ist.

## beschreiben

Beschreiben Sie das Unternehmensobjekt, um die Informationen abzurufen, die für die Interaktion mit Firmendatensätzen erforderlich sind.

```http
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

Das Muster für [Abfrage von Unternehmen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompaniesUsingGET) folgt eng der Leads -API. Der `filterType` akzeptiert jedoch nur Felder, die im Array „searchableFields“ der Antwort „Describe Companies“ oder „dedupeFields“ aufgeführt sind.

Die Abfrageparameter sind:

- `filterType` und `filterValues`: Erforderliche Parameter.
- `fields`, `nextPageToken` und `batchSize`: Optionale Parameter, die wie die entsprechenden Parameter in den Leads- und Opportunities-APIs funktionieren.

Wenn Sie eine Liste von `fields` anfordern, hat ein angefordertes Feld, das nicht zurückgegeben wird, einen impliziten Wert von null.

Wenn Sie den Feldparameter auslassen, gibt die Antwort diese Felder standardmäßig zurück:

- id
- deduplizierte Felder
- updatedAt
- createdAt

```http
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

Der [Synchronisierungsunternehmen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST)-Endpunkt akzeptiert einen erforderlichen `input`, der ein Array von Unternehmensobjekten enthält.

Wie bei Opportunitys unterstützt der Endpunkt drei Erstellungs- und Aktualisierungsmodi: createOnly, updateOnly und createOrUpdate. Geben Sie den Modus im `action` der Anfrage an.

Die Parameter `dedupeBy` und `action` sind optional. Die Standardeinstellung ist deduplizierte Felder bzw. createOrUpdate.

```http
POST /rest/v1/companies.json
```

```text
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

Das Unternehmensobjekt enthält Felder, die durch Attribute wie Anzeigename, API-Name und Datentyp definiert sind. Zusammen werden diese Attribute als Metadaten bezeichnet.

Die folgenden Endpunkte geben Abfragefelder für das Unternehmensobjekt an. Der API-Benutzer muss über eine Rolle mit der Berechtigung `Read-Write Schema Standard Field`, der Berechtigung `Read-Write Schema Custom Field` oder beidem verfügen.

### Abfragefelder

Abfragen eines Unternehmensfelds nach API-Namen oder Abrufen aller Unternehmensfelder.

#### Nach Name

Der Endpunkt [Unternehmensfeld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldByNameUsingGET) ruft Metadaten für ein Feld im Firmenobjekt ab. Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an.

Die Antwort ähnelt der Antwort von Describe Company, enthält jedoch zusätzliche Metadaten. Beispielsweise gibt das `isCustom`-Attribut an, ob das Feld benutzerdefiniert ist.

```http
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

Der [Endpunkt Firmenfelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldsUsingGET) ruft Metadaten für alle Felder im Firmenobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Verwenden Sie den `batchSize` Abfrageparameter, um diese Zahl zu reduzieren.

Wenn das `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Rufen Sie den Endpunkt mit dem zurückgegebenen `nextPageToken` weiter auf, bis `moreResult` auf „false“ gesetzt ist.

```http
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

Geben Sie Löschkriterien als Liste von Suchwerten im `input`-Array an. Geben Sie die Löschmethode im `deleteBy` an.

Die zulässigen Werte sind deduplizierte Felder und idField. Der Standardwert ist deduplizierte Felder.

```text
Content-Type: application/json
```

```http
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

- Für Unternehmens-Endpunkte gilt eine Zeitüberschreitung von 30 s, sofern nicht anders angegeben.
- Bei Synchronisierungsunternehmen beträgt die Zeitüberschreitung 60 Sekunden.
- Firmen löschen hat eine Zeitüberschreitung von 60 Jahren.
