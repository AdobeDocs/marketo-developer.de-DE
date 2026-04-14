---
title: Statische Listen
feature: REST API, Static Lists
description: Verwenden Sie Marketo-REST-APIs zum Abfragen, Erstellen, Aktualisieren und Löschen von statischen Listen mit Endpunkten für ID, Name und Durchsuchen, Ordnerumfang, Paging und Datumsfilter.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---

# Statische Listen

[Endpunktreferenz für statische Listen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von CRUD-Vorgängen für statische Listen. Diese APIs folgen dem Standard-Schnittstellenmuster für Asset-APIs und bieten Optionen zum Abfragen, Erstellen, Aktualisieren und Löschen.

Informationen zu Lead-Datenbankvorgängen für Listenmitglieder finden Sie unter [Mitgliedschaft auflisten](list-membership.md).

## Abfrage

Die Abfrage statischer Listen folgt den Standardabfragetypen für Assets von [nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) und [Durchsuchen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET).

### Nach ID

[Abfrage nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) verwendet eine einzelne statische `id` als Pfadparameter und gibt einen einzelnen statischen Listensatz zurück.

```http
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

[Abfrage nach Namen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) akzeptiert eine statische `name` als Parameter und gibt einen einzelnen statischen Listensatz zurück. Es wird eine exakte Zeichenfolgenübereinstimmung für alle statischen Listennamen in der Instanz durchgeführt und ein Ergebnis für die statische Liste zurückgegeben, die mit diesem Namen übereinstimmt.

```http
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

Statische Listen können auch [in Stapeln abgerufen) ](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET). Mit dem Parameter `folder` können Sie den übergeordneten Ordner angeben, unter dem die Abfrage ausgeführt wird, und werden als JSON-Objekt formatiert, das `id` und `type` enthält. Wie andere Endpunkte für den Massenabruf von Assets sind `offset` und `maxReturn` optionale Parameter, die für das Paging verwendet werden können. Mit den Parametern `earliestUpdatedAt` und `latestUpdatedAt` können Sie Wasserzeichen für niedrige und hohe Datums-/Uhrzeitwerte festlegen, um statische Listen zurückzugeben, die innerhalb des angegebenen Bereichs erstellt oder aktualisiert wurden. Datetime-Werte müssen gültige ISO-8601-Zeichenfolgen sein und sollten keine Millisekunden enthalten.

```http
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

[Erstellen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/createStaticListUsingPOST) wird mit einem `application/x-www-form-urlencoded` POST mit zwei erforderlichen Parametern ausgeführt. Der Parameter `folder` wird verwendet, um den übergeordneten Ordner anzugeben, unter dem die statische Liste erstellt wird, und ist als JSON-Objekt mit `id` und `type` formatiert. Der Parameter `name` wird zum Benennen der statischen Liste verwendet und muss eindeutig sein. Optional kann der `description` Parameter zur Beschreibung der statischen Liste verwendet werden.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

[Eine statische Liste wird aktualisiert](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/updateStaticListUsingPOST) erfolgt über einen separaten Endpunkt mit zwei optionalen Parametern. Der Parameter `description` kann verwendet werden, um die statische Listenbeschreibung zu aktualisieren. Der Parameter `name` kann verwendet werden, um den statischen Listennamen zu aktualisieren, und muss eindeutig sein.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

## Löschen

[Löschen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) verwendet eine einzelne statische Liste `id` als Pfadparameter. Statische Listen, die von einem Import- oder Exportvorgang verwendet werden oder die von anderen Assets verwendet werden, können nicht gelöscht werden.

```http
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
