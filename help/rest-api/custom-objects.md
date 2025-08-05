---
title: Benutzerdefinierte Objekte
feature: REST API, Custom Objects
description: Erstellen und bearbeiten Sie benutzerdefinierte Marketo-Objekte.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '2909'
ht-degree: 0%

---

# Benutzerdefinierte Objekte

[**Benutzerdefinierte Objektendpunktreferenz**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Mit Marketo können Benutzende benutzerdefinierte Marketo-Objekte definieren, die mit Marketo-Standardobjekten (Leads, Unternehmen) oder anderen benutzerdefinierten Marketo-Objekten verknüpft sind.  Benutzerdefinierte Marketo-Objekte können mithilfe der Benutzeroberfläche von Marketo wie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) beschrieben oder mithilfe der API für benutzerdefinierte Objektmetadaten wie unten beschrieben erstellt werden.

Für den Zugriff auf die Metadaten-API für benutzerdefinierte Objekte ist ein geeigneter Marketo-Abonnementtyp erforderlich.  Einzelheiten entnehmen Sie bitte Ihrem CSM.

## Liste

Zusätzlich zu den standardmäßigen Beschreib-, Abfrage-, Aktualisierungs- und Löschaufrufen, die für Lead-Datenbankobjekte verfügbar sind, verfügen benutzerdefinierte Objekte über einen [Listenaufruf](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET).  Der Aufruf dieses Endpunkts gibt eine Antwort mit einer Liste von benutzerdefinierten Objekten zurück, die in der Zielinstanz verfügbar sind, sowie mit zusätzlichen Metadaten zu den Objekten.

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

Die Antwort gibt eine Liste der Beziehungen an, die für jedes Objekt vorhanden sind.  Eine Beziehung weist ein `field`-Element auf, das das Feld auf dem Objekt angibt, das den Verknüpfungswert enthält, ein `type`-Element, das angibt, ob die Beziehung zu einem Objekt vom Typ „Übergeordnetes Element“ oder „Untergeordnetes Element“ ist, und ein `relatedTo`-Objekt, das den Namen des zugehörigen Objekts angibt, sowie das Verknüpfungsfeld auf diesem Objekt.

## beschreiben

Der [describe-Aufruf](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) für benutzerdefinierte Objekte folgt demselben Muster wie der von Opportunities und Unternehmen, mit dem Hinzufügen des `relationships`-Arrays in der Antwort und eines `apiName` Pfadparameters im URI, der den API-Namen des zu beschreibenden benutzerdefinierten Objekttyps akzeptiert.  Wie beim Listenaufruf werden hier alle Beziehungen aufgelistet, die für diesen benutzerdefinierten Objekttyp verfügbar sind.

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

[Abfrage benutzerdefinierter Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) unterscheidet sich geringfügig von anderen Lead-Datenbank-APIs und verwendet `apiName` Pfadparameter wie beschrieben.  Bei normalen filterType-Parametern handelt es sich bei der Abfrage um eine einfache GET wie Abfragen für andere Datensatztypen. Sie erfordert eine `filterType` und `filterValues`.  Optional werden `**fields**`-, `batchSize`- und `nextPageToken` akzeptiert.  Wenn beim Anfordern einer Liste von Feldern ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, ist der Wert impliziert null.

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

Alternativ verhält sich die API bei Abfragen mit zusammengesetzten Schlüsseln wie die Opportunity-Rollen-API und akzeptiert eine POST mit einem JSON-Hauptteil.  Der JSON-Text kann mit Ausnahme von `filterValues` dieselben Mitglieder wie eine GET-Abfrage haben.  Anstelle von Filterwerten gibt es ein `input`-Array, das Objekte aufnimmt, die ein Element namens für jede `dedupeFields` des Objekttyps enthalten.

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

## Erstellen und aktualisieren

Verwenden Sie den Endpunkt [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) um benutzerdefinierte Objekte zu erstellen oder zu aktualisieren. Sie können den Vorgang mithilfe des `action`-Parameters angeben.  Mit einem Aufruf können bis zu 300 Datensätze erstellt oder aktualisiert werden.  Die im `input`-Array verwendeten Werte basieren größtenteils auf den Informationen, die vom Endpunkt [Describe Custom Objects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1) zurückgegeben werden. In einem Car-Beispielobjekt gibt es nur ein Deduplizierungsfeld, `vin`.  Um Datensätze bei Verwendung des Modus dedupeFields zu aktualisieren oder zu erstellen, muss jeder Datensatz in unserem Eingabe-Array mindestens ein `vin` Feld enthalten.

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

Bei der Durchführung von Aktualisierungen über den `idField`-Modus ist die `idField` immer `marketoGUID`, sodass für jeden Datensatz immer ein `marketoGUID` erforderlich ist.  Beachten Sie, dass `idField` nur für den Aktionstyp updateOnly gültig ist, da dieses Feld immer vom System verwaltet wird.  Ihre Antwort enthält den **Status** jedes einzelnen Datensatzes im Ergebnis-Array sowie entweder ein `marketoGUID`- oder ein `reasons`-Array, je nachdem, ob der Vorgang für einen einzelnen Datensatz erfolgreich war oder nicht.

## Löschen

[Datensätze löschen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) ist sehr einfach.  Wählen Sie einfach den `deleteBy`-Modus aus, entweder `idField` oder `dedupeFields`, und schließen Sie die entsprechenden Felder in jeden der Datensätze in Ihrem `input`-Array ein. Pro Aufruf sind maximal 300 Datensätze zulässig.

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

Wie bei einer Aktualisierung enthält Ihr Ergebnis einen Status für jeden einzelnen Datensatz und je nachdem, ob der Löschvorgang erfolgreich war, entweder ein `marketoGUID` oder ein `reasons`-Array.

## Benutzerdefinierte Objekttypen

Mit der Metadaten-API für benutzerdefinierte Objekte können Sie benutzerdefinierte Objekt-Schemata remote verwalten.  Mit der -API können Sie einen neuen benutzerdefinierten Objekttyp erstellen oder einen vorhandenen ändern.  Nachdem der benutzerdefinierte Objekttyp erstellt oder geändert wurde, muss er zur Verwendung genehmigt werden.  Weitere Informationen zu benutzerdefinierten Objekten finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/home).

* Benutzerdefinierte Objekttypen, die von der API erstellt wurden, können nicht über die Marketo-Benutzeroberfläche geändert werden
* Die maximal zulässige Anzahl benutzerdefinierter Objekttypen ist 10
* Die maximal zulässige Anzahl benutzerdefinierter Objektfelder beträgt 50 pro Typ
* API-Namen und Anzeigenamen für benutzerdefinierte Objekttypen können alphanumerische Zeichen und den Unterstrich „_“ enthalten

### Abfragetyp

Es gibt zwei Möglichkeiten, Metadaten von benutzerdefinierten Objekttypen abzurufen: Beschreibung von benutzerdefinierten Objekttypen, die zurückgeben  Der Datensatz für einen einzelnen benutzerdefinierten Objekttyp ist filterbar nach Genehmigungsstatus und listet benutzerdefinierte Objekttypen auf. Dadurch wird eine Liste aller benutzerdefinierten Objekttypen im Abonnement zurückgegeben, die nach Name und Genehmigungsstatus gefiltert werden kann.

### Typ beschreiben

Der Endpunkt [Describe Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) gibt Metadaten für einen einzelnen benutzerdefinierten Objekttyp zurück. Der erforderliche `apiName`-Pfadparameter ist der API-Name des benutzerdefinierten Objekttyps, der beschrieben wird.  Wenn eine genehmigte Version vorhanden ist, wird sie zurückgegeben.  Andernfalls wird die Entwurfsversion zurückgegeben.  Der optionale Parameter `state` wird verwendet, um die zurückzugebende Version anzugeben: `draft`, `approved` oder `approvedWithDraft`.

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

Hier können Sie die folgenden Attribute anzeigen:

* Metadaten: Status, displayName, Beschreibung, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Beziehungen
* Standardfelder: marketoGUID, createdAt, updatedAt
* Benutzerdefinierte Felder: LeadId, Vin, Make,  Modell, Jahr

### Listentypen

Der Endpunkt [Benutzerdefinierte Objekttypen auflisten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) gibt Metadaten für alle benutzerdefinierten Objekttypen zurück, die in der Zielinstanz verfügbar sind.  Beachten Sie, dass dieser Endpunkt [ „Benutzerdefinierte Objekte auflisten](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=de) ähnelt, jedoch umfassender ist und zusätzliche Metadaten wie Status, Beziehungen und Felder enthält. Wenn eine genehmigte Version vorhanden ist, wird sie zurückgegeben.  Andernfalls wird die Entwurfsversion zurückgegeben.  Der optionale **state**-Parameter wird verwendet, um die Version des zurückzugebenden benutzerdefinierten Objekttyps anzugeben: **draft**, **Approved** oder **ApprovedWithDraft**.  Der optionale Parameter **names** wird verwendet, um bestimmte Namen von benutzerdefinierten Objekttypen anzugeben, die zurückgegeben werden sollen. Er ist als kommagetrennte Liste von API-Namen strukturiert.

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

Der [Endpunkt Benutzerdefinierter Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) wird verwendet, um einen benutzerdefinierten Objekttyp zu erstellen oder zu aktualisieren.  Der auszuführende Datensatzvorgang wird durch das optionale Attribut **action** gesteuert, das **createOnly**, **createOrUpdate** oder **updateOnly** sein kann.  Die Standardeinstellung ist createOrUpdate. Die Attribute **displayName** und **apiName** sind erforderlich, außer bei Verwendung von updateOnly als Aktion.   Beide müssen eindeutig sein, um Konflikte mit vom Kunden bereitgestellten Typen zu vermeiden.  Wenn Sie ein LaunchPoint-Partner sind, sollten Sie diesen Namen einen repräsentativen Namespace voranstellen.  Für apiName besteht die Konvention darin, Kleinbuchstaben oder Binnenbuchstaben zu verwenden, um zwischen anderen Textzeichenfolgen zu unterscheiden. Das optionale **pluralName**-Attribut gibt die Pluralform von displayName an.  Das optionale **description**-Attribut wird verwendet, um den benutzerdefinierten Objekttyp zu beschreiben.  Das optionale boolesche **showInLeadDetail**-Attribut wird verwendet, um die Anzeige benutzerdefinierter Objektdaten auf der Seite „Lead-Datenbank“ der Marketo-Benutzeroberfläche zu ermöglichen.  Die Standardeinstellung ist false.

Gehen Sie beim Benennen benutzerdefinierter Objekte mit Bedacht vor. Beim Erstellen eines neuen benutzerdefinierten Objekts wird empfohlen, dem Namen eine Zeichenfolge voranzustellen, die den Namen Ihres Unternehmens angibt (alphanumerisch oder Unterstrich zulässig). Dadurch lässt sich das benutzerdefinierte Objekt in der MLM-Benutzeroberfläche einfach durchsuchen, und es kann auch unsicher sein, ob der Name eindeutig ist.

Im Folgenden finden Sie ein Beispiel für die Erstellung eines neuen benutzerdefinierten Objekttyps mit der -API  Name „Transaktion“.

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

Hier können wir die folgenden benutzerdefinierten objektbezogenen Daten sehen:

* Metadaten: Status, displayName, Beschreibung, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, Beziehungen
* Standardfelder: marketoGUID, createdAt, updatedAt

#### Update-Typ

Im Folgenden finden Sie ein Beispiel für die Aktualisierung der Beschreibung für einen vorhandenen Typ, dessen API-Name „Transaktion“ lautet.  Das **apiName**-Attribut ist erforderlich.  Hier wird davon ausgegangen, dass der Typ bereits vorhanden ist, und updateOnly für das optionale Attribut **action** verwendet.  Abgesehen von **apiName** können die für die Erstellung verfügbaren Attribute aktualisiert werden.

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

Benutzerdefinierte Objekttypen müssen genehmigt werden, bevor sie verwendet werden können. Wenn ein neuer benutzerdefinierter Objekttyp mit dem Endpunkt [Benutzerdefinierter Objekttyp synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST) erstellt wird, wird er als Entwurfsversion erstellt. Wenn Sie das Hinzufügen benutzerdefinierter Felder abgeschlossen haben, müssen Sie die Entwurfsversion genehmigen. Dadurch wird eine genehmigte Version erstellt und die Entwurfsversion gelöscht. Wenn ein vorhandener benutzerdefinierter Objekttyp mit dem Endpunkt Benutzerdefinierter Objekttyp synchronisieren oder mit einem der Endpunkte vom Typ Benutzerdefiniertes Objekttyp hinzufügen/aktualisieren/löschen geändert wird, wird eine Entwurfsversion erstellt. Alle Änderungen am Typ oder an den zugehörigen Feldern wirken sich nur auf die Entwurfsversion aus. Wenn Sie die Änderungen abgeschlossen haben, müssen Sie die Entwurfsversion genehmigen. Dadurch wird die genehmigte Version durch die Entwurfsversion ersetzt und die Entwurfsversion wird gelöscht. Weitere Informationen zur benutzerdefinierten Objektgenehmigung finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Nachdem ein benutzerdefinierter Objekttyp genehmigt wurde, können Sie nicht mehr:

* `displayName` oder `apiName` aktualisieren
* Hinzufügen oder Entfernen eines Verknüpfungsfelds
* Hinzufügen oder Entfernen eines Deduplizierungsfelds

Aus diesen Gründen ist es wichtig, das Schema und die Namenskonvention, die Sie verwenden möchten, sorgfältig zu durchdenken.

### Typ genehmigen

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp genehmigen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST), um eine Entwurfsversion als neue genehmigte Version zu veröffentlichen.  **apiName** ist der einzige erforderliche Parameter als Pfadparameter.  Ein Typ kann nur genehmigt werden, wenn er sich im Entwurfsstatus befindet und einen Satz von Validierungsregeln erfüllt, die [hier) ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

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

### Verwerfungstyp

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp Entwurf verwerfen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) zum Löschen einer Entwurfsversion. `apiName` ist der einzige erforderliche Parameter als Pfadparameter. Ein Typ muss sich im Entwurfsstatus befinden, damit er verworfen werden kann, d. h. ein genehmigter Typ kann nicht verworfen werden.

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

Verwenden Sie den Endpunkt [Benutzerdefinierten Objekttyp löschen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST), um eine genehmigte Version zu löschen.  `apiName` ist der einzige erforderliche Parameter als Pfadparameter.  Beachten Sie, dass dies ein destruktiver Vorgang ist; er kann nicht rückgängig gemacht werden.  Ein Typ kann erst gelöscht werden, nachdem er von Assets wie Triggern oder Filtern aus der Verwendung entfernt wurde.  Der Endpunkt Benutzerdefiniertes Objekt abrufen von Assets kann zum Abrufen einer Liste von abhängigen Assets für einen bestimmten Typ verwendet werden.

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

* Marketo GUID - Eindeutige Kennung für benutzerdefinierten Objekttyp
* Erstellt zu - Zeitpunkt, zu dem der benutzerdefinierte Objekttyp erstellt wurde
* Aktualisiert am - Zeitpunkt der letzten Aktualisierung des benutzerdefinierten Objekttyps

Sie können benutzerdefinierte Felder mithilfe der unten beschriebenen Endpunkte hinzufügen/ändern/löschen.

* Die maximal zulässige Anzahl von Feldern ist 50
* Nachdem ein benutzerdefiniertes Objekt genehmigt wurde, können Sie dem benutzerdefinierten Objekt maximal 20 zusätzliche Felder hinzufügen
* Mindestens 1 Deduplizierungsfeld ist erforderlich, maximal 3 sind zulässig
* Feld-API-Namen und Anzeigenamen können alphanumerische Zeichen und den Unterstrich „_“ enthalten

Weitere Informationen zu benutzerdefinierten Objektfeldern finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Felder hinzufügen

Mit [ Endpunkt „Benutzerdefinierte Objekttypfelder hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) können Sie ein oder mehrere Felder zu Ihrem benutzerdefinierten Objekt hinzufügen.  Der Anfragetext enthält ein `input`-Array mit einem oder mehreren Elementen.  Jedes Element ist ein JSON-Objekt mit Attributen, die ein Feld beschreiben. Das erforderliche `name`-Attribut ist der API-Name des Felds und muss für das benutzerdefinierte Objekt eindeutig sein.   Die Konvention besagt, dass Kleinbuchstaben oder Binnenmajuskeln verwendet werden, um zwischen anderen Textzeichenfolgen zu unterscheiden. Das erforderliche `displayName` ist der für Menschen lesbare Name des Felds und muss für das benutzerdefinierte Objekt eindeutig sein. Das erforderliche `dataType`-Attribut ist der Datentyp des Felds.  A  Eine Liste der zulässigen Datentypen kann durch Aufrufen des Endpunkts [Abrufen benutzerdefinierter Objekttypfelddatentypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) abgerufen werden.  Benutzerdefinierte Objekte können Felder mit dem Datentyp „link“ enthalten.  Verknüpfungsfelder werden verwendet, um Beziehungen zwischen benutzerdefinierten Objekten und anderen Objekttypen im System, z. B. Lead, Unternehmen, herzustellen.  Weitere Informationen zu Link-Feldern finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Das optionale `description` ist die Beschreibung des Felds. Das optionale boolesche Attribut `isDedupeField` gibt an, ob das Feld während Aktualisierungsvorgängen für benutzerdefinierte Objekte für die Deduplizierung verwendet wird.  Die Standardeinstellung ist false.  Bei Eins-zu-Viele-Beziehungen ist ein Deduplizierungsfeld erforderlich. Das optionale `relatedTo`-Objektattribut gibt ein Verknüpfungsfeld an.  Bei Eins-zu-Viele-Beziehungen enthält dieses Objekt ein `name`-Attribut, das das „Verknüpfungsobjekt“ oder übergeordnete Objekt ist, mit dem eine Verknüpfung hergestellt werden soll, und ein `field`-Attribut, das „Verknüpfungsfeld“ lautet.  oder das Feld innerhalb des übergeordneten Objekts, das als Schlüsselattribut verwendet werden soll.  Rufen Sie [ Endpunkt „Benutzerdefinierte Objekte für verknüpfbare ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) abrufen“ auf, um eine Liste der zulässigen Link-Objekte abzurufen.  Weitere Informationen zu Verknüpfungsfeldern finden Sie in der Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Ein benutzerdefiniertes Objekt kann nicht mit einem anderen benutzerdefinierten Objekt verknüpft werden, das über ein vorhandenes Verknüpfungsfeld verfügt.

### Eins-zu-viele-Beziehung

Bei einer Eins-zu-viele-Objektstruktur können Sie ein Verknüpfungsfeld in einem benutzerdefinierten Objekt verwenden, um es mit einem Standardobjekt zu verbinden: Lead oder Unternehmen. Mithilfe des Beispiels für Fahrzeugbesitzer aus der Marketo-Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) erstellen wir ein benutzerdefiniertes Objekt, das autobezogene Informationen enthält, um eine Verbindung mit Leads herzustellen.

1. Erstellen eines **Car**-Objekts
1. Felder zu **Auto** Objekt: deduplizieren auf **VIN**, Link zu **Lead**&#x200B;**/Lead-ID**
1. Objekt **Car** genehmigen

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

Fügen Sie jetzt Felder zum benutzerdefinierten Objekttyp „Auto“ hinzu. In einem Verknüpfungsfeld geben wir sowohl das Objekt als auch das Feld an, mit dem die Verbindung hergestellt werden soll. In diesem Fall ist das Verknüpfungsobjekt Lead und das Verknüpfungsfeld ist ID. Verwenden Sie ein Zeichenfolgenfeld für die Deduplizierung (FIN).  Wir fügen drei weitere Felder hinzu, um zusätzliche Auto-Attribute zu speichern (Marke, Modell, Jahr).

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

Genehmigen Sie abschließend den benutzerdefinierten Objekttyp.

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

Viele-zu-viele-Beziehungen werden mithilfe eines benutzerdefinierten „Brückenobjekts“, oder einer Zwischenstation, zwischen einem benutzerdefinierten Standardobjekt wie Lead oder Unternehmen und einem benutzerdefinierten „Edge“-Objekt dargestellt. Das Edge-Objekt ist die primäre Entität, die beschreibende Attribute (Felder) enthält. Das Bridge-Objekt enthält die Daten zur Auflösung der Objektbeziehungen mithilfe von zwei Verknüpfungsfeldern.  Ein Verknüpfungsfeld verweist genau wie in einem auf das übergeordnete Standardobjekt zurück  Eins-zu-viele-Beziehungskonfiguration.  Das Feld Andere Verknüpfung zeigt auf das Edge-Objekt, das ein benutzerdefiniertes Objekt ohne Verknüpfungen ist.  Das Bridge-Objekt kann auch beschreibende Attribute (Felder) enthalten. Unter Verwendung des Beispiels für die Registrierung für College-Kurse aus der Marketo-Produktdokumentation [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) erstellen wir ein benutzerdefiniertes Edge-Objekt, das kursbezogene Informationen enthält, und ein Registrierungsbrückenobjekt, das zum Verbinden von Kursen mit Leads verwendet wird. Im Folgenden finden Sie die Schritte:

1. Erstellen eines **Kurs** Edge-Objekts
1. Felder zu **Kurs:** Deduplizierung auf **Kurs-ID** hinzufügen
1. Approve **COURSE**
1. Erstellen eines **Enrollment**-Brückenobjekts
1. Fügen Sie Felder zu **Enrollment:** dedupe on **Enrollment ID**, link to **Course**&#x200B;**/Course ID** field and link to **Lead**/**/Lead ID**
1. Approve **Enrollment**

Erstellen Sie zunächst den Edge-Objekttyp, um kursspezifische Informationen zu enthalten:

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

Als Nächstes fügen wir dem Edge-Objekttyp benutzerdefinierte Felder hinzu.  In diesem Beispiel fügen wir die folgenden vier benutzerdefinierten Felder hinzu, um einen College-Kurs zu modellieren: Kurs-ID, Kursleiter, Ort des Kurses, Kursname.  Beachten Sie, dass wir die Kurs-ID als unser Deduplizierungsfeld festlegen, da mindestens ein Deduplizierungsfeld erforderlich ist.

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

Jetzt müssen wir den Kantenobjekttyp genehmigen, damit wir später beim Verknüpfen mit dem Brückenobjekttyp darauf verweisen können.  Beachten Sie, dass benutzerdefinierte Objekttypen genehmigt sein müssen, damit sie als Link-Objekt ausgewählt werden können.

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

Um benutzerdefinierte Felder zum Brücken-Objekttyp hinzuzufügen, fügen Sie zwei Verknüpfungsfelder hinzu: ein Feld, das mit dem Lead-Objekt verknüpft ist, und ein weiteres, das mit dem soeben erstellten Kursobjekt verknüpft ist. Um eine Verknüpfung mit dem Lead-Objekt herzustellen, verwenden Sie das Feld Lead-ID . Um eine Verknüpfung mit dem Kursobjekt herzustellen, verwenden Sie das Feld Kurs-ID .  Fügen Sie als Nächstes eine eindeutige Registrierungs-ID als Deduplizierungsfeld hinzu, da mindestens ein Deduplizierungsfeld erforderlich ist. Fügen Sie abschließend ein Feld Note hinzu, um zu verfolgen, wie es der Schüler gemacht hat.

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

Sie können benutzerdefinierte Objektdatensätze programmgesteuert mithilfe von [Benutzerdefiniertes Objekt synchronisieren](#create_and_update) oder [Massenimport benutzerdefinierter Objekte) ](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=de). Alternativ können Sie die Marketo-Benutzeroberflächenfunktion ([ von benutzerdefinierten Objektdaten importieren) ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Feld aktualisieren

Der Endpunkt [Benutzerdefiniertes Objekttypfeld aktualisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) ermöglicht es Ihnen, ein Feld in Ihrem benutzerdefinierten Entwurfsobjekt zu aktualisieren.  Der erforderliche Pfadparameter `apiName` ist der API-Name des benutzerdefinierten Objekttyps.  Der erforderliche Pfadparameter `fieldAPIName` ist der API-Name des Felds für den benutzerdefinierten Objekttyp.  Der Anfragetext enthält ein JSON-Objekt, das Schlüssel/Wert-Paare enthält, die die zu aktualisierenden Feldattribute angeben.

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

Mit [ Endpunkt „Benutzerdefinierte Objekttypfelder löschen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) können Sie ein oder mehrere Felder aus Ihrem benutzerdefinierten Objekt löschen.  Der erforderliche Pfadparameter `apiName` ist der API-Name des benutzerdefinierten Objekttyps.  Der Anfragetext enthält ein JSON-Objekt mit einem `input`-Array mit einem oder mehreren Elementen.  Jedes Element ist ein JSON-Objekt mit einem `name` , das den API-Namen des zu löschenden Felds angibt.

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

## Auflisten von Felddatentypen

Der Endpunkt [Abrufen benutzerdefinierter Objekttyp-Felddatentypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) gibt die Liste aller zulässigen Felddatentypen zurück. Dies ist beim Modellieren Ihres benutzerdefinierten Objekttyps nützlich, um die unterstützten Datentypen für benutzerdefinierte Felder zu identifizieren.

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

## Auflisten verknüpfbarer benutzerdefinierter Objekte

Der Endpunkt [Benutzerdefinierte Objekte für verknüpfbare ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) abrufen“ gibt eine Liste aller zulässigen Link-Objekte und ihrer Verknüpfungsfelder zurück.  Die Liste enthält Standardobjekte (Lead, Unternehmen) und alle benutzerdefinierten Objekte, die in der Instanz erstellt wurden.

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

## Abrufen benutzerdefinierter objektabhängiger Assets

Der Endpunkt [Benutzerdefiniertes objektabhängiges Assets abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) gibt eine Liste von abhängigen Assets eines benutzerdefinierten Objekttyps zurück, einschließlich ihres Speicherorts in der Instanz.  Dies ist nützlich, wenn Sie eine Integration entfernen, und Sie müssen überall dort identifizieren, wo ein benutzerdefinierter Objekttyp verwendet wird.

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

## Zeitüberschreitungen

* Bei Endpunkten für benutzerdefinierte Objekte beträgt die maximale Wartezeit 30 Sekunden, sofern unten nicht anders angegeben
   * Benutzerdefinierte Objekte synchronisieren: 120s
   * Benutzerdefinierte Objekte löschen: 60er
