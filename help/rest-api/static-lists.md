---
title: Statische Listen
feature: REST API, Static Lists
description: Verwenden Sie Marketo-REST-APIs zum Abfragen, Erstellen, Aktualisieren und Löschen von statischen Listen mit Endpunkten für ID, Name und Durchsuchen, Ordnerumfang, Paging und Datumsfilter.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
TQID: https://experienceleague.adobe.com/DSV9h6d4F3ZrIUT-VtqlmFAnpdxOuTf05ajCqiGegqk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 360
ht-degree: 1%

---

# Statische Listen

[Endpunktreferenz für statische Listen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

Verwenden Sie die REST-APIs für statische Listen, um statische Listen abzufragen, zu erstellen, zu aktualisieren und zu löschen.

Informationen zu Lead-Datenbankvorgängen für Listenmitglieder finden Sie unter [Mitgliedschaft auflisten](list-membership.md).

## Abfrage

Statische Listen abfragen [nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) oder nach [Browsen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET).

### Nach ID

[Abfrage nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) verwendet einen statischen `id`-Pfadparameter und gibt den entsprechenden Datensatz zurück.

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

[Abfrage nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) verwendet einen `name`-Parameter der statischen Liste. Der Endpunkt führt eine exakte Übereinstimmung mit Namen statischer Listen durch und gibt den übereinstimmenden Datensatz zurück.

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

Verwenden Sie den Durchsuchen-Endpunkt, um [statische Listen in Batches abzurufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET). Der optionale Parameter `folder` erfasst die Abfrage für einen übergeordneten Ordner. Übergeben Sie den Ordner als JSON-Objekt, das `id` und `type` enthält.

Verwenden Sie `offset` und `maxReturn` für die Paginierung. Verwenden Sie `earliestUpdatedAt` und `latestUpdatedAt` als niedrige und hohe Datums-/Uhrzeitgrenzen. Diese Parameter geben Listen zurück, die innerhalb des Bereichs erstellt oder aktualisiert wurden. ISO-8601-Werte ohne Millisekunden verwenden.

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

Senden einer `application/x-www-form-urlencoded` POST-Anfrage an [Erstellen einer statischen Liste](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/createStaticListUsingPOST). Die Parameter `folder` und `name` sind erforderlich.

Übergeben Sie `folder` als JSON-Objekt, das `id` und `type` enthält. Der `name` muss eindeutig sein. Der optionale `description`-Parameter beschreibt die Liste.

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

Verwenden Sie den Update-Endpunkt, um [eine statische Liste zu ändern](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/updateStaticListUsingPOST). Der optionale `description` ändert die Beschreibung. Der optionale `name` ändert den Namen und muss eindeutig sein.

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

Um [statische Liste zu löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) übergeben Sie ihren `id` als Pfadparameter. Eine von einem Import, Export oder einem anderen Asset verwendete Liste kann nicht gelöscht werden.

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
