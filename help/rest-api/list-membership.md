---
title: Mitgliedschaft in der Liste (statische Listen)
feature: REST API, Static Lists
description: Verwenden Sie die Marketo Lead-Datenbank-REST-APIs, um Leads zu statischen Listen hinzuzufügen, Leads zu entfernen, Listenmitglieder abzurufen und die Mitgliedschaft in der Checkliste zu überprüfen.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 5%

---

# Mitgliedschaft in der Liste (statische Listen)

[Referenz zum List Membership Endpoint](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Die APIs für die Listenmitgliedschaft stellen Endpunkte für die Lead-Datenbank zum Arbeiten mit statischen Listenmitgliedern bereit. Diese Endpunkte können verwendet werden, um Leads zu einer Liste hinzuzufügen, Leads aus einer Liste zu entfernen, Mitglieder einer Liste abzurufen und festzustellen, ob ein oder mehrere Leads Mitglieder einer Liste sind.

## Endpunkte

| Endpunkt | Methode | Pfad |
| --- | --- | --- |
| Hinzufügen zur Liste | POST | `/rest/v1/lists/{listId}/leads.json` |
| Entfernen aus Liste | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Leads nach Listen-ID abrufen | GET | `/rest/v1/lists/{listId}/leads.json` |
| Listenmitglied | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Hinzufügen zur Liste

Der [Zu Liste hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST)-Endpunkt wird verwendet, um ein oder mehrere Mitglieder zu einer Liste hinzuzufügen. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```http
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

## Entfernen aus Liste

Der [Aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE)-Endpunkt wird verwendet, um ein oder mehrere Mitglieder aus einer Liste zu entfernen. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```http
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
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

## Leads nach Listen-ID abrufen

Der Endpunkt [Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) wird zum Abrufen von Mitgliedern einer Liste verwendet. Der Endpunkt akzeptiert einen erforderlichen `listId` und ermöglicht es mehreren optionalen Abfrageparametern, Filterkriterien anzugeben.

Mit dem Parameter `batchSize` wird die Anzahl der Lead-Datensätze angegeben, die in einem einzelnen Aufruf zurückgegeben werden sollen. Der Standardwert und das Maximum sind 300.

Der `nextPageToken`-Parameter wird verwendet, um durch große Ergebnismengen zu paginieren. Dieser Parameter wird nicht im ersten Aufruf übergeben, sondern nur in nachfolgenden Aufrufen zur Paginierung.

Der `fields` enthält eine kommagetrennte Liste von Feldnamen, die in der Antwort zurückgegeben werden sollen. Wenn der `fields` nicht in dieser Anfrage enthalten ist, werden die folgenden Standardfelder zurückgegeben: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` und `id`.

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten besteht, welche die in der Anfrage angegebenen Lead-Felder enthalten.

```http
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

## Listenmitglied

Der Endpunkt [Mitglied der Liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) wird verwendet, um zu sehen, ob ein oder mehrere Leads Mitglieder einer Liste sind. Der Endpunkt benötigt einen erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält (maximal zulässig ist 300).

Die Antwort enthält ein `result`-Array, das aus JSON-Objekten mit dem Status für jede Lead-ID besteht, die in der Anfrage angegeben wurde.

```http
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
