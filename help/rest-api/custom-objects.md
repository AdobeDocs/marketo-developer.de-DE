---
title: Benutzerdefinierte Objekte
feature: REST API, Custom Objects
description: Erfahren Sie, wie Sie benutzerdefinierte Marketo-Objekte über die REST-API erstellen und verwalten, einschließlich Auflisten und Beschreiben von Endpunkten, Metadaten, Beziehungen, Feldern und Abfragen.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
TQID: https://experienceleague.adobe.com/NWm9CjFVqQdVDJRrnE4nA299-Lg53-JR7xvY-82dUqY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ea4e3ff5-e7b9-4b4c-a5a0-dc27cc3f4275
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2938
ht-degree: 0%

---

# Benutzerdefinierte Objekte

[**Benutzerdefinierter Objekt-Endpunktverweis**](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

Benutzerdefinierte Marketo-Objekte können sich auf Marketo-Standardobjekte wie Leads und Unternehmen oder auf andere benutzerdefinierte Marketo-Objekte beziehen. Erstellen Sie benutzerdefinierte Marketo-Objekte in der [Marketo](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)Benutzeroberfläche oder mithilfe der in diesem Dokument beschriebenen API für benutzerdefinierte Objektmetadaten.

Für den Zugriff auf die Metadaten-API für benutzerdefinierte Objekte ist ein geeigneter Marketo-Abonnementtyp erforderlich. Weitere Informationen erhalten Sie von Ihrem CSM.

## Liste

Zusätzlich zu den standardmäßigen Aufrufen zum Beschreiben, Abfragen, Aktualisieren und Löschen für Lead-Datenbankobjekte bieten benutzerdefinierte Objekte einen [Listenaufruf](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET). Der Endpunkt gibt die benutzerdefinierten Objekte zurück, die in der Zielinstanz verfügbar sind, sowie Metadaten zu den einzelnen Objekten.

```http
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

Die Antwort listet die Beziehungen für jedes Objekt auf. Jede Beziehung enthält:

- `field`: Das Feld im -Objekt, das den Verknüpfungswert enthält.
- `type`: Ob das verknüpfte Objekt ein übergeordnetes oder ein untergeordnetes Objekt ist.
- `relatedTo`: Der Name des zugehörigen Objekts und sein Verknüpfungsfeld.

## beschreiben

Der [Describe call](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) für benutzerdefinierte Objekte folgt demselben Muster wie Opportunities und Unternehmen und erhält zwei Ergänzungen:

- Der `apiName` Pfadparameter gibt den API-Namen des benutzerdefinierten Objekttyps an, der beschrieben werden soll.
- Die Antwort enthält ein `relationships`-Array, das die für den benutzerdefinierten Objekttyp verfügbaren Beziehungen auflistet.

```http
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
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
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Abfrage

[Abfrage benutzerdefinierter Objekte](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET) unterscheidet sich geringfügig von der Abfrage anderer Lead-Datenbankobjekte. Wie im Beispiel von Describe nimmt die Anfrage einen `apiName` Pfadparameter an.

Senden Sie für einen normalen filterType eine GET-Anfrage mit den erforderlichen `filterType`- und `filterValues`. Sie können auch die optionalen Parameter `**fields**`, `batchSize` und `nextPageToken` einbeziehen.

Wenn Sie eine Liste von Feldern anfordern, hat ein angefordertes Feld, das nicht zurückgegeben wird, einen impliziten Wert von null.

```http
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

Bei Abfragen mit zusammengesetzten Schlüsseln verhält sich die API wie die Opportunity-Rollen-API und akzeptiert eine POST-Anfrage mit einem JSON-Hauptteil. Der Textkörper kann mit Ausnahme von `filterValues` dieselben Mitglieder wie eine GET-Abfrage enthalten.

Geben Sie anstelle von Filterwerten ein `input` Array von Objekten an. Jedes -Objekt enthält einen -Member für jedes Feld im `dedupeFields` des -Objekttyps.

```http
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Erstellen und aktualisieren

Verwenden Sie den Endpunkt [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) um benutzerdefinierte Objekte zu erstellen oder zu aktualisieren. Geben Sie den Vorgang mit dem Parameter `action` an. Jeder Aufruf kann bis zu 300 Datensätze erstellen oder aktualisieren.

Stützen Sie die Werte im `input`-Array auf die vom Endpunkt [Describe Custom Objects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1) zurückgegebenen Informationen. Im Beispiel-Car-Objekt ist das einzige Deduplizierungsfeld `vin`. Wenn Sie den dedupeFields-Modus verwenden, um Datensätze zu erstellen oder zu aktualisieren, schließen Sie mindestens ein `vin` Feld in jedes Objekt im Eingabe-Array ein.

```http
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Beim Aktualisieren von Datensätzen im `idField` wird die `idField` immer `marketoGUID`. Fügen Sie in jeden Datensatz ein `marketoGUID` ein.

Da dieses Feld vom System verwaltet wird, ist `idField` nur für den Aktionstyp updateOnly gültig. Das Ergebnis-Array enthält den **Status** jedes Datensatzes. Sie enthält außerdem entweder eine `marketoGUID` für einen erfolgreichen Vorgang oder ein `reasons`-Array für einen fehlgeschlagenen Vorgang.

## Löschen

Um [Datensätze zu löschen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) wählen Sie einen `deleteBy` entweder `idField` oder `dedupeFields` aus. Schließen Sie die entsprechenden Felder in jeden Datensatz im `input`-Array ein. Jeder Aufruf erlaubt maximal 300 Datensätze.

```http
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Wie bei Aktualisierungen enthält das Ergebnis einen Status für jeden Datensatz. Sie enthält außerdem entweder einen `marketoGUID` für eine erfolgreiche Löschung oder ein `reasons`-Array für eine fehlgeschlagene Löschung.

## Benutzerdefinierte Objekttypen

Mit der Metadaten-API für benutzerdefinierte Objekte können Sie benutzerdefinierte Objekt-Schemata remote verwalten. Verwenden Sie ihn, um einen benutzerdefinierten Objekttyp zu erstellen oder einen vorhandenen zu ändern. Nachdem Sie einen Typ erstellt oder geändert haben, genehmigen Sie ihn vor der Verwendung.

Weitere Informationen finden Sie unter [Produktdokumentation zu benutzerdefinierten Objekten](https://experienceleague.adobe.com/de/docs/marketo/using/home).

- Benutzerdefinierte Objekttypen, die von der API in der Marketo-Benutzeroberfläche erstellt wurden, können nicht geändert werden.
- Die maximale Anzahl benutzerdefinierter Objekttypen ist 10.
- Die maximale Anzahl benutzerdefinierter Objektfelder beträgt 50 pro Typ.
- API-Namen und Anzeigenamen für benutzerdefinierte Objekttypen können alphanumerische Zeichen und den Unterstrich „_“ enthalten.

### Abfragetyp

Sie können benutzerdefinierte Metadaten des Objekttyps auf eine der folgenden Arten abrufen:

- Benutzerdefinierten Objekttyp beschreiben gibt einen benutzerdefinierten Objekttyp-Datensatz zurück und unterstützt das Filtern nach Genehmigungsstatus.
- Benutzerdefinierte Objekttypen auflisten gibt alle benutzerdefinierten Objekttypen im Abonnement zurück und unterstützt das Filtern nach Name und Genehmigungsstatus.

### Typ beschreiben

Der Endpunkt [Describe Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) gibt Metadaten für einen benutzerdefinierten Objekttyp zurück. Der erforderliche `apiName`-Pfadparameter gibt den API-Namen des zu beschreibenden Typs an.

Wenn eine genehmigte Version vorhanden ist, gibt der Endpunkt sie zurück. Andernfalls wird die Entwurfsversion zurückgegeben. Verwenden Sie den optionalen `state`, um `draft`, `approved` oder `approvedWithDraft` anzufordern.

```http
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Die Antwort enthält:

- Metadaten: Status, displayName, Beschreibung, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Beziehungen.
- Standardfelder: marketoGUID, createdAt, updatedAt.
- Benutzerdefinierte Felder: leadId, vin, make, model, year.

### Listentypen

Der Endpunkt [Benutzerdefinierte Objekttypen auflisten](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) gibt Metadaten für alle benutzerdefinierten Objekttypen zurück, die in der Zielinstanz verfügbar sind. Sie ähnelt &quot;[&#x200B; benutzerdefinierte Objekte auflisten](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=de), enthält jedoch zusätzliche Metadaten wie Status, Beziehungen und Felder.

Wenn eine genehmigte Version vorhanden ist, gibt der Endpunkt sie zurück. Andernfalls wird die Entwurfsversion zurückgegeben.

Die optionalen Parameter sind:

- **state**: Gibt die zurückzugebende Version an. Gültige Werte sind **Entwurf**, **genehmigt** und **ApprovedWithDraft**.
- **names**: Gibt die benutzerdefinierten Objekttypen an, die als kommagetrennte Liste von API-Namen zurückgegeben werden sollen.

```http
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it is a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

### Typ erstellen und aktualisieren

#### Typ erstellen

Verwenden Sie den Endpunkt [Benutzerdefinierter Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST), um einen benutzerdefinierten Objekttyp zu erstellen oder zu aktualisieren.

Die Attribute sind:

- **action**: Ein optionales Attribut, das den Datensatzvorgang steuert. Gültige Werte sind **createOnly**, **createOrUpdate** und **updateOnly**. Der Standardwert ist createOrUpdate.
- **displayName** und **apiName**: Erforderlich, es sei denn, die Aktion ist „updateOnly“. Beide müssen eindeutig sein, um Konflikte mit vom Kunden bereitgestellten Typen zu vermeiden. LaunchPoint-Partner sollten einen repräsentativen Namespace voranstellen. Verwenden Sie für apiName „lowercase“ oder „camelCase“, um es von anderen Textzeichenfolgen zu unterscheiden.
- **pluralName**: Ein optionales Attribut, das die Pluralform von displayName angibt.
- **description**: Ein optionales Attribut, das den benutzerdefinierten Objekttyp beschreibt.
- **showInLeadDetail**: Ein optionales boolesches Attribut, das benutzerdefinierte Objektdaten auf der Seite „Lead-Datenbank“ der Marketo-Benutzeroberfläche aktiviert. Der Standardwert lautet „false“.

Wählen Sie benutzerdefinierte Objektnamen sorgfältig aus. Jedem neuen benutzerdefinierten Objektnamen wird eine Zeichenfolge vorangestellt, die Ihr Unternehmen identifiziert. Das Präfix kann alphanumerische Zeichen oder Unterstriche enthalten. Diese Konvention erleichtert das Auffinden des Objekts in der MLM-Benutzeroberfläche und stellt sicher, dass sein Name eindeutig ist.

Im folgenden Beispiel wird ein benutzerdefinierter Objekttyp mit dem API-Namen „Transaktion“ erstellt.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

Die folgende Anfrage beschreibt den neu erstellten Typ.

```http
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Die Antwort enthält:

- Metadaten: Status, displayName, Beschreibung, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Beziehungen.
- Standardfelder: marketoGUID, createdAt, updatedAt.

#### Update-Typ

Im folgenden Beispiel wird die Beschreibung eines vorhandenen Typs aktualisiert, dessen API-Name „Transaktion“ lautet. Das **apiName**-Attribut ist erforderlich. Da der Typ bereits vorhanden ist, verwendet die Anfrage updateOnly für das optionale Attribut **action**.

Neben **apiName** können Sie die bei der Erstellung verfügbaren Attribute aktualisieren.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Genehmigung des Typs

Validieren Sie benutzerdefinierte Objekttypen, bevor Sie sie verwenden. Wenn Sie einen Typ mit dem Endpunkt [Benutzerdefinierter Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST) erstellt Marketo eine Entwurfsversion. Nachdem Sie benutzerdefinierte Felder hinzugefügt haben, genehmigen Sie den Entwurf. Genehmigung erstellt eine genehmigte Version und löscht den Entwurf.

Wenn Sie einen vorhandenen Typ mit dem Endpunkt Benutzerdefinierter Objekttyp synchronisieren oder Benutzerdefinierter Objekttyp hinzufügen/aktualisieren/löschen ändern, erstellt Marketo einen Entwurf. Änderungen am Typ oder den zugehörigen Feldern wirken sich nur auf die Entwurfsversion aus. Nachdem Sie Änderungen vorgenommen haben, genehmigen Sie den Entwurf. Genehmigung ersetzt die genehmigte Version durch den Entwurf und löscht den Entwurf.

Weitere Informationen finden Sie in der [Dokumentation zur benutzerdefinierten Objektgenehmigung](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Nachdem ein benutzerdefinierter Objekttyp genehmigt wurde, können Sie nicht mehr:

- Aktualisieren Sie die `displayName` oder `apiName`.
- Ein Verknüpfungsfeld hinzufügen oder entfernen.
- Ein Deduplizierungsfeld hinzufügen oder entfernen.

Planen Sie das Schema und die Namenskonvention sorgfältig, bevor Sie den Typ genehmigen.

### Typ genehmigen

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp genehmigen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST), um einen Entwurf als neue genehmigte Version zu veröffentlichen. Der einzige erforderliche Parameter ist der Pfadparameter **apiName**.

Sie können einen Typ nur genehmigen, wenn er sich im Entwurfsstatus befindet und die dokumentierten [Validierungsregeln) &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```http
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Verwerfungstyp

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp Entwurf verwerfen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) zum Löschen einer Entwurfsversion. Der einzige erforderliche Parameter ist der `apiName`.

Sie können nur einen Typ im Entwurfsstatus verwerfen. Ein genehmigter Typ kann nicht verworfen werden.

```http
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Typ löschen

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp löschen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST), um eine genehmigte Version zu löschen. Der einzige erforderliche Parameter ist der `apiName`.

Dieser Vorgang ist zerstörerisch und kann nicht rückgängig gemacht werden. Bevor Sie einen Typ löschen, entfernen Sie seine Verwendung aus Assets wie Triggern und Filtern. Verwenden Sie den Endpunkt Benutzerdefinierte objektabhängige Assets abrufen , um die abhängigen Assets für einen Typ abzurufen.

POST /rest/v1/customobjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Benutzerdefinierte Objektfelder

Standardmäßig enthalten alle benutzerdefinierten Objekttypen die folgenden Standardfelder:

- Marketo-GUID: Eindeutige Kennung für den benutzerdefinierten Objekttyp.
- Erstellt am: Datum/Uhrzeit der Erstellung des benutzerdefinierten Objekttyps.
- Aktualisiert am: Zeitpunkt der letzten Aktualisierung des benutzerdefinierten Objekttyps.

Verwenden Sie die folgenden Endpunkte, um benutzerdefinierte Felder hinzuzufügen, zu ändern oder zu löschen.

- Die maximale Anzahl von Feldern ist 50.
- Nachdem ein benutzerdefiniertes Objekt genehmigt wurde, können Sie maximal 20 zusätzliche Felder hinzufügen.
- Mindestens ein Deduplizierungsfeld ist erforderlich. Es sind maximal drei Deduplizierungsfelder zulässig.
- Feld-API-Namen und Anzeigenamen können alphanumerische Zeichen und den Unterstrich „_“ enthalten.

Weitere Informationen finden Sie in der [Dokumentation zu benutzerdefinierten Objektfeldern](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Felder hinzufügen

Verwenden Sie den [Endpunkt Benutzerdefinierte Objekttypfelder hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST), um ein oder mehrere Felder zu einem benutzerdefinierten Objekt hinzuzufügen. Der Anfragetext enthält ein `input`-Array mit einem oder mehreren Elementen. Jedes Element ist ein JSON-Objekt mit Attributen, die ein Feld beschreiben.

Die Feldattribute sind:

- `name`: Erforderlich. Der API-Name des Felds, der für das benutzerdefinierte Objekt eindeutig sein muss. Verwenden Sie Kleinbuchstaben oder Binnenmajuskeln, um den Namen von anderen Textzeichenfolgen zu unterscheiden.
- `displayName`: Erforderlich. Der für Menschen lesbare Feldname, der für das benutzerdefinierte Objekt eindeutig sein muss.
- `dataType`: Erforderlich. Der Datentyp des Felds. Verwenden Sie den [Endpunkt Benutzerdefinierte Objekttyp-Felddatentypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) um die zulässigen Datentypen abzurufen.
- `description`: Optional. Die Feldbeschreibung.
- `isDedupeField`: Optionaler boolescher Wert, der angibt, ob das Feld während Aktualisierungsvorgängen für benutzerdefinierte Objekte für die Deduplizierung verwendet wird. Der Standardwert lautet „false“. Für 1-zu-viele-Beziehungen ist ein Deduplizierungsfeld erforderlich.
- `relatedTo`: Optionales Objekt, das ein Verknüpfungsfeld angibt. Bei einer Eins-zu-Viele-Beziehung identifiziert `name` das „Verknüpfungsobjekt“ oder das übergeordnete Objekt und `field` das „Verknüpfungsfeld“ oder Schlüsselfeld im übergeordneten Objekt.

Benutzerdefinierte Objekte können Felder mit dem Datentyp „link“ enthalten. Verknüpfungsfelder stellen Beziehungen zwischen benutzerdefinierten Objekten und anderen Objekttypen wie Lead und Unternehmen her. Einzelheiten zu [&#x200B; finden Sie in der &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields) zu benutzerdefinierten Objektfeldern . Verwenden Sie den Endpunkt [Benutzerdefinierte Objekte verknüpfbar](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET), um die zulässigen Link-Objekte abzurufen.

Ein benutzerdefiniertes Objekt kann nicht mit einem anderen benutzerdefinierten Objekt verknüpft werden, das über ein vorhandenes Verknüpfungsfeld verfügt. Weitere Informationen finden Sie in der [Dokumentation zu Verknüpfungsfeldern](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Eins-zu-viele-Beziehung

Bei einer Eins-zu-viele-Objektstruktur können Sie ein Verknüpfungsfeld verwenden, um ein benutzerdefiniertes Objekt mit einem standardmäßigen Lead- oder Firmenobjekt zu verbinden. Der folgende Workflow verwendet das [Beispiel für Fahrzeugbesitzer](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) um ein benutzerdefiniertes Objekt zu erstellen, das Autoinformationen speichert und eine Verbindung zu Leads herstellt.

1. Erstellen Sie ein **Car**-Objekt.
1. Fügen Sie Felder zum **Car**-Objekt hinzu: dedupe on **VIN** und link to **Lead**&#x200B;**/Lead ID**.
1. Genehmigen Sie das **Car**-Objekt.

Erstellen Sie zunächst den benutzerdefinierten Objekttyp, der autospezifische Informationen enthält.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

Fügen Sie anschließend Felder zum benutzerdefinierten Objekttyp „Auto“ hinzu. Verwenden Sie ein Verknüpfungsfeld, um sowohl das Objekt als auch das Feld für die Verbindung anzugeben. In diesem Beispiel ist das Verknüpfungsobjekt Lead und das Verknüpfungsfeld ist ID.

Verwenden Sie ein Zeichenfolgenfeld für die Deduplizierung (FIN). Fügen Sie drei weitere Felder hinzu, um die Attribute „Marke“, „Modell“ und „Jahr“ zu speichern.

```http
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Genehmigen Sie abschließend den benutzerdefinierten Objekttyp.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Viele-zu-viele-Beziehung

Bei einer Viele-zu-viele-Beziehung wird ein benutzerdefiniertes Objekt „Brücke“ zwischen einem Standardobjekt wie Lead oder Unternehmen und einem benutzerdefinierten Objekt „Kante“ verwendet. Das Edge-Objekt ist die primäre Entität und enthält beschreibende Felder.

Das Bridge-Objekt löst die Beziehung mit zwei Verknüpfungsfeldern auf. Ein Feld verweist auf das übergeordnete Standardobjekt, wie in einer Eins-zu-Viele-Beziehung. Der andere zeigt auf das Edge-Objekt, das ein benutzerdefiniertes Objekt ohne Verknüpfungen ist. Das Bridge-Objekt kann auch beschreibende Felder enthalten.

Der folgende Workflow verwendet das [Beispiel für eine Registrierung für einen College-Kurs](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure). Es werden ein Kurs-Edge-Objekt und ein Registrierungsbrücken-Objekt erstellt, das Kurse mit Leads verbindet.

1. Erstellen Sie **Edge** Objekt „Kurs“.
1. Fügen Sie Felder zu **Kurs:** Deduplizierung auf **Kurs-ID)**.
1. Approve **COURSE**.
1. Erstellen Sie ein **Enrollment**-Brückenobjekt.
1. Fügen Sie Felder zu **Registrierung:** Deduplizierung für **Registrierungs-ID**, einen Link zum **Kurs**&#x200B;**/Kurs-ID**-Feld und einen Link zu **Lead**&#x200B;**/Lead-ID** hinzu.
1. Genehmigen **Registrierung**.

Erstellen Sie zunächst den Edge-Objekttyp, der kursspezifische Informationen enthält:

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

Fügen Sie als Nächstes vier benutzerdefinierte Felder hinzu, um einen College-Kurs zu modellieren: Kurs-ID, Kursleiter, Kursort und Kursname. Bestimmen Sie die Kurs-ID als Deduplizierungsfeld, da mindestens ein Deduplizierungsfeld erforderlich ist.

```http
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Genehmigen Sie den Kantenobjekttyp, damit Sie ihn beim Verknüpfen mit dem Brückenobjekttyp referenzieren können. Ein benutzerdefinierter Objekttyp muss genehmigt werden, bevor er als Link-Objekt ausgewählt werden kann.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Erstellen Sie nach Abschluss des Edge-Objekts den Bridge-Objekttyp, der registrierungsspezifische Informationen enthält.

```http
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Fügen Sie zwei Verknüpfungsfelder zum Bridge-Objekttyp hinzu: eines, das mit dem Lead-Objekt verknüpft ist, und eines, das mit dem Kursobjekt verknüpft ist. Verwenden Sie das Feld Lead-ID , um eine Verknüpfung mit dem Lead herzustellen, und das Feld Kurs-ID , um eine Verknüpfung mit dem Kurs herzustellen.

Fügen Sie die Registrierungs-ID als Deduplizierungsfeld hinzu, da mindestens ein Deduplizierungsfeld erforderlich ist. Fügen Sie dann ein Feld Note hinzu, um die Leistung des Schülers zu verfolgen.

```http
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Genehmigen Sie abschließend das Bridge-Objekt.

```http
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Programmgesteuertes Füllen von benutzerdefinierten Objektdatensätzen mithilfe von [Benutzerdefiniertes Objekt synchronisieren](#create_and_update) oder [Massenimport benutzerdefinierter Objekte](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=de). Alternativ können Sie [Benutzerdefinierte Objektdaten importieren](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) in der Marketo-Benutzeroberfläche verwenden.

## Feld aktualisieren

Verwenden Sie den [Endpunkt Benutzerdefiniertes Objekttyp-Feld aktualisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST), um ein Feld in einem benutzerdefinierten Entwurfsobjekt zu aktualisieren.

Die erforderlichen Pfadparameter sind:

- `apiName`: Der API-Name des benutzerdefinierten Objekttyps.
- `fieldAPIName`: Der API-Name des Felds für den benutzerdefinierten Objekttyp.

Der Anfragetext enthält ein JSON-Objekt mit Schlüssel/Wert-Paaren, die die zu aktualisierenden Feldattribute angeben.

```http
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Felder löschen

Verwenden Sie den Endpunkt [Benutzerdefinierte Objekttypfelder löschen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST), um ein oder mehrere Felder aus einem benutzerdefinierten Objekt zu löschen. Der erforderliche `apiName`-Pfadparameter gibt den API-Namen des benutzerdefinierten Objekttyps an.

Der Anfragetext enthält ein JSON-Objekt mit einem `input` Array aus einem oder mehreren Elementen. Jedes Element ist ein JSON-Objekt, dessen `name` den API-Namen eines zu löschenden Felds angibt.

```http
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Auflisten von Felddatentypen

Der Endpunkt [Abrufen benutzerdefinierter Objekttyp-Felddatentypen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) gibt alle zulässigen Felddatentypen zurück. Verwenden Sie diesen Endpunkt, um die benutzerdefinierten Felddatentypen zu identifizieren, die beim Modellieren eines benutzerdefinierten Objekttyps verfügbar sind.

```http
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Auflisten verknüpfbarer benutzerdefinierter Objekte

Der Endpunkt [Benutzerdefinierte Objekte verknüpfbar](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) gibt alle zulässigen Link-Objekte und deren Verknüpfungsfelder zurück. Die Antwort enthält Standardobjekte wie Lead und Unternehmen sowie alle in der Instanz erstellten benutzerdefinierten Objekte.

```http
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Abrufen benutzerdefinierter objektabhängiger Assets

Der Endpunkt [Benutzerdefiniertes objektabhängiges Assets abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) gibt die abhängigen Assets eines benutzerdefinierten Objekttyps und ihre Speicherorte in der -Instanz zurück. Verwenden Sie ihn beim Entfernen einer Integration, um überall dort zu identifizieren, wo ein benutzerdefinierter Objekttyp verwendet wird.

```http
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```

```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Zeitüberschreitungen

- Bei Endpunkten für benutzerdefinierte Objekte beträgt die maximale Wartezeit 30 Sekunden, sofern nicht anders angegeben.
- Die maximale Wartezeit für die Synchronisierung benutzerdefinierter Objekte beträgt 120 Sekunden.
- Die maximale Wartezeit für das Löschen benutzerdefinierter Objekte beträgt 60 Sekunden.
