---
title: Statische Listen
feature: REST API, Static Lists
description: Durchführen von CRUD-Vorgängen für statische Listen.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# Statische Listen

[Endpunkt-Referenz für statische Listen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Referenz zum Mitgliedschaftsendpunkt auflisten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von CRUD-Vorgängen für statische Listen. Diese APIs folgen dem Standard-Schnittstellenmuster für Asset-APIs und bieten Optionen zum Abfragen, Erstellen, Aktualisieren und Löschen.

## Abfrage

Die Abfrage statischer Listen folgt den Standardabfragetypen für Assets von [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) und [Durchsuchen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Nach ID

[Abfrage nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) verwendet eine einzelne statische `id` als Pfadparameter und gibt einen einzelnen statischen Listensatz zurück.

```
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Nach Name

[Abfrage nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) akzeptiert eine statische `name` als Parameter und gibt einen einzelnen statischen Listensatz zurück. Es wird eine exakte Zeichenfolgenübereinstimmung für alle statischen Listennamen in der Instanz durchgeführt und ein Ergebnis für die statische Liste zurückgegeben, die mit diesem Namen übereinstimmt.

```
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Durchsuchen

Statische Listen können auch [in Stapeln abgerufen) ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). Mit dem Parameter `folder` können Sie den übergeordneten Ordner angeben, unter dem die Abfrage ausgeführt wird, und ist als JSON-Objekt mit der ID und dem Typ formatiert. Wie andere Endpunkte für den Massenabruf von Assets sind `offset` und `maxReturn` optionale Parameter, die für das Paging verwendet werden können. Mit den Parametern `earliestUpdatedAt` und `latestUpdatedAt` können Sie Wasserzeichen für niedrige und hohe Datums-/Uhrzeitwerte festlegen, um statische Listen zurückzugeben, die innerhalb des angegebenen Bereichs erstellt oder aktualisiert wurden. Datumszeitwerte müssen gültige ISO-8601-Zeichenfolgen sein und sollten keine Millisekunden enthalten

```
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Erstellen und aktualisieren

[Erstellen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) wird mit einer application/x-www-form-urlencoded-POST mit zwei erforderlichen Parametern ausgeführt. Der Parameter `folder` wird verwendet, um den übergeordneten Ordner anzugeben, unter dem die statische Liste erstellt wird, und ist als JSON-Objekt mit der ID und dem Typ formatiert. Der Parameter `name` wird zum Benennen der statischen Liste verwendet und muss eindeutig sein. Optional kann der `description` Parameter zur Beschreibung der statischen Liste verwendet werden.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

[Aktualisierungen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) werden über einen separaten Endpunkt mit zwei optionalen Parametern vorgenommen. Der Parameter `description` kann verwendet werden, um die statische Listenbeschreibung zu aktualisieren. Der Parameter `name` kann verwendet werden, um den statischen Listennamen zu aktualisieren, und muss eindeutig sein.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

### Löschen

[Löschen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) verwendet eine einzelne statische Liste `id` als Pfadparameter. Statische Listen, die von einem Import- oder Exportvorgang verwendet werden oder die von anderen Assets verwendet werden, können nicht gelöscht werden.

```
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```

## Mitgliedschaft auflisten

Die Listenmitgliedschafts-Endpunkte bieten die Möglichkeit, statische Listenmitglieder hinzuzufügen, zu entfernen und abzufragen. Darüber hinaus können Sie die Zugehörigkeit zu einer statischen Liste abfragen.

### Zu Liste hinzufügen

Der [Zu Liste hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST)-Endpunkt wird verwendet, um ein oder mehrere Mitglieder zu einer Liste hinzuzufügen. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen ID-Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Aus Liste entfernen

Der [Aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE)-Endpunkt wird verwendet, um ein oder mehrere Mitglieder aus einer Liste zu entfernen. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

### Abfrageliste

Der Endpunkt [Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) wird zum Abrufen von Mitgliedern einer Liste verwendet. Der Endpunkt akzeptiert einen erforderlichen `listId` und ermöglicht es mehreren optionalen Abfrageparametern, Filterkriterien anzugeben.

Mit dem Parameter `batchSize` wird die Anzahl der Lead-Datensätze angegeben, die in einem einzelnen Aufruf zurückgegeben werden sollen (Standard und Maximum ist 300).

Der `nextPageToken`-Parameter wird verwendet, um durch große Ergebnismengen zu paginieren. Dieser Parameter wird nicht im ersten Aufruf übergeben, sondern nur in nachfolgenden Aufrufen für die Paginierung.

Der `fields` enthält eine kommagetrennte Liste von Feldnamen, die in der Antwort zurückgegeben werden sollen. Wenn der Feldparameter in dieser Anfrage nicht enthalten ist, werden die folgenden Standardfelder zurückgegeben: email, updatedAt, createdAt, lastName, firstName und id.

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten besteht, welche die in der Anfrage angegebenen Lead-Felder enthalten.

```
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

#### Abfragelistenmitgliedschaft nach Lead-ID

Der Endpunkt [Mitglied der Liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) wird verwendet, um zu sehen, ob ein oder mehrere Leads Mitglieder einer Liste sind. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
