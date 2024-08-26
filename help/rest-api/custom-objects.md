---
title: Benutzerdefinierte Objekte
feature: REST API, Custom Objects
description: Erstellen und bearbeiten Sie benutzerdefinierte Marketo-Objekte.
source-git-commit: f1c9c5ba07013c2fdccb9f799b5c0077eb789a4b
workflow-type: tm+mt
source-wordcount: '2910'
ht-degree: 0%

---


# Benutzerdefinierte Objekte

[**Custom Object Endpoint Reference**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Mit Marketo können Benutzer benutzerdefinierte Marketo-Objekte definieren, die mit Marketo Standard-Objekten (Leads, Unternehmen) oder anderen benutzerdefinierten Objekten von Marketo verknüpft sind.  Benutzerdefinierte Marketo-Objekte können mithilfe der Marketo-Benutzeroberfläche wie [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) beschrieben oder mithilfe der API für benutzerdefinierte Objektmetadaten wie unten beschrieben erstellt werden.

Für den Zugriff auf die API für benutzerdefinierte Objektmetadaten ist ein entsprechender Marketo-Abonnementtyp erforderlich.  Weitere Informationen erhalten Sie in Ihrem CSM.

## Liste

Zusätzlich zu den standardmäßigen Aufrufen zum Beschreiben, Abfragen, Aktualisieren und Löschen für Lead-Datenbankobjekte steht für benutzerdefinierte Objekte ein [Listenaufruf](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) zur Verfügung.  Der Aufruf dieses Endpunkts gibt eine Antwort mit einer Liste von benutzerdefinierten Objekten zurück, die in der Zielinstanz verfügbar sind, sowie zusätzliche Metadaten zu den Objekten.

```
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

Die Antwort gibt eine Liste der Beziehungen an, die auf den einzelnen Objekten vorhanden sind.  Eine Beziehung hat ein `field` -Element, das das Feld des Objekts anzeigt, das den Link-Wert enthält, ein `type` -Element, das angibt, ob die Beziehung zu einem übergeordneten Objekt oder einem untergeordneten Objekt besteht, und ein `relatedTo` -Objekt, das den Namen des zugehörigen Objekts angibt, sowie das Verknüpfungsfeld für dieses Objekt.

## Beschreibung

Der [beschreiben-Aufruf](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) für benutzerdefinierte Objekte folgt demselben Muster wie der von Chancen und Unternehmen, wobei das `relationships` -Array in der Antwort und ein `apiName` -Pfadparameter im URI hinzugefügt werden, der den API-Namen des zu beschreibenden benutzerdefinierten Objekttyps verwendet.  Wie beim Listenaufruf werden hier alle Beziehungen aufgeführt, die für diesen benutzerdefinierten Objekttyp verfügbar sind.

```
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

[Die Abfrage benutzerdefinierter Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) unterscheidet sich geringfügig von anderen Lead-Datenbank-APIs und nimmt den Pfad-Parameter `apiName` wie beschrieben an.  Bei normalen Filtertypparametern handelt es sich bei der Abfrage um eine einfache GET wie Abfragen für andere Datensatztypen. Sie erfordert `filterType` und `filterValues`.  Optional werden die Parameter `**fields**`, `batchSize` und `nextPageToken` akzeptiert.  Wenn beim Anfordern einer Feldliste ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
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

Alternativ verhält sich die API bei der Abfrage mit ebenenübergreifenden Schlüsseln wie die Opportunity Roles-API und akzeptiert eine POST mit einem JSON-Hauptteil.  Der JSON-Hauptteil kann dieselben Mitglieder wie eine GET-Abfrage haben, mit Ausnahme von `filterValues`.  Anstelle von Filterwerten gibt es ein `input` -Array, das Objekte verwendet, die ein Element mit dem Namen für die `dedupeFields` des Objekttyps enthalten.

```
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

## Erstellen und Aktualisieren

Verwenden Sie den Endpunkt [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) , um benutzerdefinierte Objekte zu erstellen oder zu aktualisieren. Sie können den Vorgang mit dem Parameter `action` angeben.  In einem Aufruf können bis zu 300 Datensätze erstellt oder aktualisiert werden.  Die im Array `input` verwendeten Werte basieren größtenteils auf den vom Endpunkt [Benutzerdefinierte Objekte beschreiben](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/descriptionUsingGET_1) zurückgegebenen Informationen. In einem Beispiel-Objekt für ein Auto gibt es nur ein dedupliziertes Feld, `vin`.  Um bei Verwendung des dedupeFields-Modus Datensätze zu aktualisieren oder zu erstellen, muss jeder Datensatz in unserem Eingabe-Array mindestens ein `vin` -Feld enthalten.

```
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

Bei der Durchführung von Aktualisierungen über den Modus `idField` ist der `idField` immer `marketoGUID`, sodass jeder Datensatz immer ein Feld `marketoGUID` benötigt.  Beachten Sie, dass `idField` nur für den Aktionstyp updateOnly gültig ist, da dieses Feld immer vom System verwaltet wird.  Ihre Antwort enthält den **Status** jedes einzelnen Datensatzes im Ergebnis-Array sowie entweder ein `marketoGUID` - oder ein `reasons` -Array, je nachdem, ob der Vorgang für einen einzelnen Datensatz erfolgreich war oder nicht.

## Löschen

[Das Löschen von Datensätzen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) ist sehr einfach.  Wählen Sie einfach Ihren `deleteBy` -Modus, entweder `idField` oder `dedupeFields`, und fügen Sie die entsprechenden Felder in jeden Datensatz in Ihrem `input` -Array ein. Pro Aufruf sind maximal 300 Datensätze zulässig.

```
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

Wie beim Aktualisieren enthält Ihr Ergebnis einen Status für jeden einzelnen Datensatz und entweder ein `marketoGUID` - oder ein `reasons` -Array, je nachdem, ob der Löschvorgang erfolgreich war.

## Benutzerdefinierte Objekttypen

Mit der API für benutzerdefinierte Objektmetadaten können Sie benutzerdefinierte Objektschemata remote verwalten.  Mit der API können Sie einen neuen benutzerdefinierten Objekttyp erstellen oder einen vorhandenen ändern.  Nachdem der benutzerdefinierte Objekttyp erstellt oder geändert wurde, muss er zur Verwendung genehmigt werden.  Weitere Informationen zu benutzerdefinierten Objekten finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/home).

* Benutzerdefinierte Objekttypen, die von der API erstellt wurden, können nicht über die Marketo-Benutzeroberfläche geändert werden
* Die maximal zulässige Anzahl benutzerdefinierter Objekttypen beträgt 10
* Die maximal zulässige Anzahl benutzerdefinierter Objektfelder beträgt 50 pro Typ
* API-Namen und Anzeigenamen des benutzerdefinierten Objekttyps können alphanumerische Zeichen und Unterstriche &quot;_&quot;enthalten.

### Abfragetyp

Es gibt zwei Möglichkeiten, benutzerdefinierte Metadaten vom Objekttyp abzurufen: Beschreibung des benutzerdefinierten Objekttyps, der  den Datensatz für einen einzelnen benutzerdefinierten Objekttyp, der nach Genehmigungsstatus und List Custom Object Types gefiltert werden kann, der eine Liste aller benutzerdefinierten Objekttypen im Abonnement zurückgibt und nach Name und Genehmigungsstatus gefiltert werden kann.

### Typ beschreiben

Der Endpunkt [Custom Object Type beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) gibt Metadaten für einen einzelnen benutzerdefinierten Objekttyp zurück. Der erforderliche Pfadparameter `apiName` ist der API-Name des benutzerdefinierten Objekttyps, der beschrieben wird.  Wenn eine genehmigte Version vorhanden ist, wird sie zurückgegeben.  Andernfalls wird die Entwurfsversion zurückgegeben.  Der optionale Parameter `state` wird verwendet, um die zurückzugebende Version anzugeben: `draft`, `approved` oder `approvedWithDraft`.

```
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

Hier können wir die folgenden Attribute sehen:

* Metadaten: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Standardfelder: marketoGUID, createdAt, updatedAt
* Benutzerdefinierte Felder leadId, vin, make, make  Modell, Jahr

### Listentypen

Der Endpunkt [Liste benutzerdefinierter Objekttypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) gibt Metadaten für alle benutzerdefinierten Objekttypen zurück, die in der Zielinstanz verfügbar sind.  Beachten Sie, dass dieser Endpunkt [Benutzerdefinierte Objekte auflisten](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en) ähnelt, jedoch umfassender ist und zusätzliche Metadaten wie Status, Beziehungen und Felder enthält. Wenn eine genehmigte Version vorhanden ist, wird sie zurückgegeben.  Andernfalls wird die Entwurfsversion zurückgegeben.  Der optionale Parameter **state** wird verwendet, um die Version des benutzerdefinierten Objekttyps anzugeben, der zurückgegeben werden soll: **draft**, **authorised** oder **authorisedWithDraft**.  Der optionale Parameter **names** wird verwendet, um bestimmte Namen benutzerdefinierter Objekttypen anzugeben, die zurückgegeben werden sollen. Er ist als kommagetrennte Liste von API-Namen strukturiert.

```
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
            "description": "No really, it's a car!",
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

Der Endpunkt [Benutzerdefinierten Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) wird zum Erstellen oder Aktualisieren eines benutzerdefinierten Objekttyps verwendet.  Der auszuführende Datensatzvorgang wird durch das optionale Attribut **action** gesteuert, das **createOnly**, **createOrUpdate** oder **updateOnly** sein kann.  Die Standardeinstellung ist createOrUpdate. Die Attribute **displayName** und **apiName** sind erforderlich, außer bei Verwendung von updateOnly als Aktion.   Beide müssen eindeutig sein, um Kollisionen mit kundenbereitgestellten Typen zu vermeiden.  Wenn Sie ein LaunchPoint-Partner sind, sollten Sie diesen Namen einen repräsentativen Namespace voranstellen.  Für apiName besteht die Konvention darin, Kleinbuchstaben oder Binnenmajuskel-Groß-/Kleinschreibung zu verwenden, um zwischen anderen Textzeichenfolgen zu unterscheiden. Das optionale Attribut **pluralName** gibt das Plural-Formular von displayName an.  Das optionale Attribut **description** wird verwendet, um den benutzerdefinierten Objekttyp zu beschreiben.  Das optionale boolesche Attribut **showInLeadDetail** wird verwendet, um die Anzeige benutzerdefinierter Objektdaten auf der Seite &quot;Lead-Datenbank&quot;der Marketo-Benutzeroberfläche zu ermöglichen.  Die Standardeinstellung ist &quot;false&quot;.

Gehen Sie beim Benennen von benutzerdefinierten Objekten vorsichtig vor. Bei der Erstellung eines neuen benutzerdefinierten Objekts wird empfohlen, dem Namen eine Zeichenfolge vorzugeben, die Ihren Unternehmensnamen angibt (alphanumerische Zeichen oder Unterstriche sind zulässig). Dadurch kann das benutzerdefinierte Objekt in der MLM-Benutzeroberfläche einfach durchsucht werden und es kann auch nicht sicher sein, ob der Name eindeutig ist.

Hier ist ein Beispiel für die Erstellung eines neuen benutzerdefinierten Objekttyps mit API  Name: &quot;transaction&quot;.

```
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

Im Folgenden finden Sie einen nachfolgenden Aufruf zur Beschreibung des neu erstellten Typs.

```
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

Hier können wir die folgenden benutzerspezifischen objektbezogenen Daten sehen:

* Metadaten: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Standardfelder: marketoGUID, createdAt, updatedAt

#### Aktualisierungstyp

Im Folgenden finden Sie ein Beispiel für die Aktualisierung der Beschreibung für einen vorhandenen Typ, dessen API-Name &quot;Transaktion&quot;lautet.  Das Attribut **apiName** ist erforderlich.  In unserem Beispiel gehen wir davon aus, dass der Typ bereits vorhanden ist, und verwenden updateOnly für das optionale Attribut **action** .  Neben **apiName** können auch die zur Erstellung verfügbaren Attribute aktualisiert werden.

```
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

Benutzerdefinierte Objekttypen müssen genehmigt werden, bevor sie verwendet werden können. Wenn ein neuer benutzerdefinierter Objekttyp mit dem Endpunkt [Benutzerdefinierter Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST) erstellt wird, wird er als Entwurfsversion erstellt. Wenn Sie mit dem Hinzufügen benutzerdefinierter Felder fertig sind, müssen Sie die Entwurfsversion genehmigen. Dadurch wird eine genehmigte Version erstellt und die Entwurfsversion gelöscht. Wenn ein vorhandener benutzerdefinierter Objekttyp mithilfe des Endpunkts Benutzerdefinierter Objekttyp synchronisieren oder mithilfe eines Endpunkts Benutzerdefiniertes Objekttyp hinzufügen/aktualisieren/löschen geändert wird, wird eine Entwurfsversion erstellt. Alle Änderungen am Typ oder an seinen Feldern wirken sich nur auf die Entwurfsversion aus. Wenn Sie die Änderung abgeschlossen haben, müssen Sie den Entwurf genehmigen. Dadurch wird die genehmigte Version durch die Entwurfsversion ersetzt und der Entwurf gelöscht. Weitere Informationen zur Genehmigung benutzerdefinierter Objekte finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Nachdem ein benutzerdefinierter Objekttyp genehmigt wurde, können Sie Folgendes nicht mehr tun:

* Aktualisieren Sie die `displayName` oder `apiName`
* Link-Feld hinzufügen oder entfernen
* Hinzufügen oder Entfernen eines Deduplizierungsfelds

Aus diesen Gründen ist es wichtig, sorgfältig über das Schema und die Benennungskonvention nachzudenken, die Sie verwenden möchten.

### Typ genehmigen

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp genehmigen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) , um eine Entwurfsversion als neue genehmigte Version zu veröffentlichen.  **apiName** ist der einzige erforderliche Parameter als Pfadparameter.  Ein Typ kann nur genehmigt werden, wenn er sich im Entwurfsstatus befindet und eine Reihe von Validierungsregeln erfüllt, die [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object) beschrieben sind.

```
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Verwerfen Typ

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp verwerfen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) , um eine Entwurfsversion zu löschen. `apiName` ist der einzige erforderliche Parameter als Pfadparameter. Ein Typ muss sich im Entwurfsstatus befinden, damit er verworfen werden kann, d. h. ein genehmigter Typ kann nicht verworfen werden.

```
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

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp löschen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) , um eine genehmigte Version zu löschen.  `apiName` ist der einzige erforderliche Parameter als Pfadparameter.  Beachten Sie, dass es sich um einen destruktiven Vorgang handelt, der nicht rückgängig gemacht werden kann.  Ein Typ kann nicht gelöscht werden, es sei denn, er wurde aus der Verwendung durch Assets wie Trigger oder Filter entfernt.  Mit dem Endpunkt Benutzerdefinierte Objekte abrufen von Assets können Sie eine Liste der abhängigen Assets für einen bestimmten Typ abrufen.

POST /rest/v1/customsobjekte/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Benutzerdefinierte Objektfelder

Standardmäßig enthalten alle benutzerdefinierten Objekttypen die folgenden Standardfelder:

* Marketo GUID - Eindeutige Kennung für benutzerdefinierten Objekttyp
* Erstellt am - Zeitpunkt, zu dem der benutzerdefinierte Objekttyp erstellt wurde
* Aktualisiert am - Datum, an dem der benutzerdefinierte Objekttyp zuletzt aktualisiert wurde

Sie können benutzerdefinierte Felder mithilfe der unten beschriebenen Endpunkte hinzufügen/ändern/löschen.

* Die maximal zulässige Anzahl von Feldern ist 50
* Nachdem ein benutzerdefiniertes Objekt genehmigt wurde, können Sie dem benutzerdefinierten Objekt maximal 20 weitere Felder hinzufügen
* Es ist mindestens 1 Deduplizierungsfeld erforderlich, maximal 3 sind zulässig.
* Feld-API-Namen und Anzeigenamen können alphanumerische Zeichen und Unterstriche &quot;_&quot;enthalten.

Weitere Informationen zu benutzerdefinierten Objektfeldern finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Felder hinzufügen

Mit dem Endpunkt [Benutzerdefinierte Objekttypfelder hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) können Sie einem benutzerdefinierten Objekt ein oder mehrere Felder hinzufügen.  Der Anfrageinhalt enthält ein `input` -Array mit einem oder mehreren Elementen.  Jedes Element ist ein JSON-Objekt mit Attributen, die ein Feld beschreiben. Das erforderliche Attribut `name` ist der API-Name des Felds und muss für das benutzerdefinierte Objekt eindeutig sein.   Die Konvention besteht darin, Kleinbuchstaben oder Binnenmajuskel-Groß-/Kleinschreibung zu verwenden, um zwischen anderen Textzeichenfolgen zu unterscheiden. Das erforderliche Attribut `displayName` ist der für Menschen lesbare Name des Felds und muss für das benutzerdefinierte Objekt eindeutig sein. Das erforderliche Attribut `dataType` ist der Datentyp des Felds.  A  Eine Liste der zulässigen Datentypen erhalten Sie, indem Sie den Endpunkt [Felddatentypen für benutzerdefinierte Objekttypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) aufrufen.  Benutzerdefinierte Objekte können Felder mit dem Datentyp &quot;link&quot;enthalten.  Verknüpfungsfelder werden verwendet, um Beziehungen zwischen benutzerdefinierten Objekten und anderen Objekttypen im System herzustellen, z. B. Lead, Unternehmen.  Weitere Informationen zu Link-Feldern finden Sie [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Das optionale Attribut `description` ist die Beschreibung des Felds. Das optionale boolesche Attribut `isDedupeField` gibt an, ob das Feld bei Aktualisierungsvorgängen für benutzerdefinierte Objekte zur Deduplizierung verwendet wird.  Die Standardeinstellung ist &quot;false&quot;.  Für 1:n-Beziehungen ist ein Deduplizierungsfeld erforderlich. Das optionale Objektattribut `relatedTo` gibt ein Verknüpfungsfeld an.  Bei 1:n-Beziehungen enthält dieses Objekt ein `name` -Attribut, das das zu verknüpfende &quot;Link-Objekt&quot;oder übergeordnete Objekt ist, und ein `field` -Attribut, das das &quot;Verknüpfungsfeld&quot;ist.  oder das Feld innerhalb des übergeordneten Objekts, das als Schlüsselattribut verwendet werden soll.  Rufen Sie den Endpunkt [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) auf, um eine Liste der zulässigen Link-Objekte abzurufen.  Weitere Informationen zu Link-Feldern finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Ein benutzerdefiniertes Objekt kann nicht mit einem anderen benutzerdefinierten Objekt verknüpft werden, das über ein vorhandenes Verknüpfungsfeld verfügt.

### Eins-zu-viele-Beziehung

Verwenden Sie für eine 1:n-benutzerdefinierte Objektstruktur ein Verknüpfungsfeld in einem benutzerdefinierten Objekt, um es mit einem Standardobjekt zu verbinden: Lead oder Firma. Unter Verwendung des Beispiels des Autoinhabers aus der Marketo-Produktdokumentation [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) erstellen wir ein benutzerdefiniertes Objekt, das autobezogene Informationen für die Verbindung mit Leads enthält.

1. Erstellen eines **Auto** -Objekts
1. Felder zum Objekt **Auto** hinzufügen: Deduplizierung bei **VIN**, Verknüpfung zu **Lead****/Lead-ID**
1. Genehmigen des Objekts **Auto**

Erstellen Sie zunächst den benutzerdefinierten Objekttyp, der autospezifische Informationen enthält.

```
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

Fügen Sie nun dem benutzerdefinierten Objekttyp &quot;Auto&quot;Felder hinzu. Wir verwenden ein Verknüpfungsfeld, um sowohl das Objekt als auch das Feld anzugeben, mit dem eine Verbindung hergestellt werden soll. In diesem Fall ist das Linkobjekt &quot;Lead&quot;und das Linkfeld &quot;ID&quot;. Verwenden Sie ein Zeichenfolgenfeld zur Deduplizierung (VIN).  Es werden drei weitere Felder hinzugefügt, um zusätzliche Autokomponenten zu speichern (Marke, Modell, Jahr).

```
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

Validieren Sie abschließend den benutzerdefinierten Objekttyp.

```
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

Viele-zu-viele-Beziehungen werden durch ein &quot;Bridge&quot;- oder ein zwischengeschaltetes, benutzerdefiniertes Objekt zwischen einem standardmäßigen benutzerdefinierten Objekt, wie z. B. &quot;Lead&quot;oder &quot;Unternehmen&quot;, und einem benutzerdefinierten &quot;Edge&quot;-Objekt dargestellt. Das edge -Objekt ist die primäre Entität, die beschreibende Attribute (Felder) enthält. Das bridge -Objekt enthält die Daten, um die Objektbeziehungen mithilfe von 2 Verknüpfungsfeldern aufzulösen.  Ein Verknüpfungsfeld verweist wie in einem  Eins-zu-viele-Beziehungskonfiguration.  Das andere Verknüpfungsfeld verweist auf das edge-Objekt, bei dem es sich um ein benutzerdefiniertes Objekt ohne Links handelt.  Das Bridge-Objekt kann auch beschreibende Attribute (Felder) enthalten. Mithilfe des Beispiels für die Kursregistrierung aus der Marketo-Produktdokumentation [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) erstellen wir ein Edge-benutzerdefiniertes Objekt, das Kursinformationen enthält, sowie ein Registrierungsbrücken-Objekt, das zum Verbinden von Kursen mit Leads verwendet wird. Gehen Sie wie folgt vor:

1. Erstellen eines Kantenobjekts **Kurs**
1. Felder zu **Kurs:** Deduplizierung bei **Kurse-ID** hinzufügen
1. Genehmigen **Kurs**
1. Erstellen eines Brückenobjekts **Enrollment**
1. Fügen Sie Felder zu **Registrierung:** Deduplizierung bei **Registrierungs-ID** hinzu, verknüpfen Sie zum Feld **Kurs****/Kurs-ID** und verknüpfen Sie zum Feld **Lead***/Lead-ID**.
1. Genehmigen **Einschreibung**

Erstellen Sie zunächst den Kantenobjekttyp, der kursspezifische Informationen enthält:

```
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

Als Nächstes fügen wir benutzerdefinierte Felder zum Edge-Objekttyp hinzu.  In diesem Beispiel fügen wir die folgenden vier benutzerdefinierten Felder hinzu, um einen Schulungskurs zu modellieren: Kurse-ID, Kursleiter, Ort des Kurses, Kursname.  Beachten Sie, dass wir die Kurs-ID als unser Deduplizierungsfeld bezeichnen, da mindestens ein Deduplizierungsfeld erforderlich ist.

```
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

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Nun müssen wir den Kantenobjekttyp genehmigen, damit wir ihn später bei der Verknüpfung mit dem Bridge-Objekttyp referenzieren können.  Beachten Sie, dass benutzerdefinierte Objektarten genehmigt werden müssen, damit sie als Link-Objekt ausgewählt werden können.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Das Kantenobjekt ist fertig.  Erstellen wir nun den Bridge-Objekttyp, der registrierungsspezifische Informationen enthält.

```
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

Um dem Bridge-Objekttyp benutzerdefinierte Felder hinzuzufügen, fügen Sie zwei Verknüpfungsfelder hinzu: ein Verknüpfungsfeld mit dem Lead-Objekt und ein weiteres Verknüpfen mit dem soeben erstellten Kurs-Objekt. Verwenden Sie das Feld Lead-ID , um eine Verknüpfung zum Lead-Objekt herzustellen. Verwenden Sie das Feld Kurs-ID , um eine Verknüpfung zum Kursobjekt herzustellen.  Fügen Sie als Nächstes eine eindeutige Kennung für die Registrierungs-ID als unser Deduplizierungsfeld hinzu, da mindestens ein Deduplizierungsfeld erforderlich ist. Fügen Sie abschließend ein Feld für die Bewertung hinzu, um zu verfolgen, wie es der Student getan hat.

```
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

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Sie können benutzerdefinierte Objektdatensätze programmgesteuert mit [Benutzerdefiniertes Objekt synchronisieren](#create_and_update) oder [Massen-Importieren benutzerdefinierter Objekte](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en) füllen. Alternativ können Sie die Marketo-UI-Funktion [Benutzerdefinierte Objektdaten importieren](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) verwenden.

## Feld aktualisieren

Mit dem Endpunkt [Benutzerdefiniertes Objekttypfeld aktualisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) können Sie ein Feld in Ihrem Entwurf eines benutzerdefinierten Objekts aktualisieren.  Der erforderliche Pfadparameter `apiName` ist der API-Name des benutzerdefinierten Objekttyps.  Der erforderliche Pfadparameter `fieldAPIName` ist der API-Name des Felds für den benutzerdefinierten Objekttyp.  Der Anfrageinhalt enthält ein JSON-Objekt mit Schlüssel/Wert-Paaren, die die zu aktualisierenden Feldattribute angeben.

```
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

Mit dem Endpunkt [Benutzerdefinierte Objekttypfelder löschen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) können Sie ein oder mehrere Felder aus Ihrem benutzerdefinierten Objekt löschen.  Der erforderliche Pfadparameter `apiName` ist der API-Name des benutzerdefinierten Objekttyps.  Der Anfrageinhalt enthält ein JSON-Objekt mit einem `input` -Array mit einem oder mehreren Elementen.  Jedes Element ist ein JSON-Objekt mit dem Attribut `name` , das den API-Namen des zu löschenden Felds angibt.

```
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

## Felddatentypen auflisten

Der Endpunkt [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) gibt die Liste aller zulässigen Felddatentypen zurück. Dies ist bei der Modellierung Ihres benutzerdefinierten Objekttyps nützlich, um die unterstützten benutzerdefinierten Felddatentypen zu identifizieren.

```
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

## Verknüpfbare benutzerdefinierte Objekte auflisten

Der Endpunkt [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) gibt eine Liste aller zulässigen Link-Objekte und deren Verknüpfungsfelder zurück.  Die Liste enthält Standardobjekte (Lead, Firma) und alle benutzerdefinierten Objekte, die in der Instanz erstellt wurden.

```
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

## Benutzerdefinierte objektabhängige Assets abrufen

Der Endpunkt &quot;[Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET)&quot;gibt eine Liste der abhängigen Assets eines benutzerdefinierten Objekttyps zurück, einschließlich des zugehörigen In-Instanz-Speicherorts.  Dies ist beim Entfernen einer Integration nützlich. Sie müssen überall identifizieren, wo ein benutzerdefinierter Objekttyp verwendet wird.

```
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

## Timeouts

* Benutzerdefinierte Objekte-Endpunkte haben eine Zeitüberschreitung von 30 s, sofern nicht unten angegeben
   * Benutzerdefinierte Objekte synchronisieren: 120s 
   * Benutzerdefinierte Objekte löschen: 60 s
