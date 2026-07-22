---
title: Mitgliedschaft in der Liste (statische Listen)
feature: REST API, Static Lists
description: Verwenden Sie die Marketo Lead-Datenbank-REST-APIs, um Leads zu statischen Listen hinzuzufügen, Leads zu entfernen, Listenmitglieder abzurufen und die Mitgliedschaft in der Checkliste zu überprüfen.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 6%

---

# Mitgliedschaft in der Liste (statische Listen)

[Referenz zum List Membership Endpoint](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

Die APIs für die Listenmitgliedschaft stellen Endpunkte für Lead-Datenbanken zum Verwalten statischer Listenmitglieder bereit. Verwenden Sie diese Endpunkte für Folgendes:

- Hinzufügen von Leads zu einer Liste.
- Leads aus einer Liste entfernen.
- Abrufen von Mitgliedern einer Liste.
- Bestimmen, ob Leads Mitglieder einer Liste sind.

## Endpunkte

| Endpunkt | Methode | Pfad |
| --- | --- | --- |
| Hinzufügen zur Liste | POST | `/rest/v1/lists/{listId}/leads.json` |
| Entfernen aus Liste | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Leads nach Listen-ID abrufen | GET | `/rest/v1/lists/{listId}/leads.json` |
| Listenmitglied | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Hinzufügen zur Liste

Verwenden Sie den [Zu Liste hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST)-Endpunkt, um ein oder mehrere Mitglieder zu einer Liste hinzuzufügen. Übergeben Sie den erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält. Die maximale Anzahl von Lead-IDs ist 300.

Die Antwort enthält ein `result`-Array mit dem Status jeder Lead-ID in der Anfrage.

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

Verwenden Sie den Endpunkt [Aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE), um ein oder mehrere Mitglieder aus einer Liste zu entfernen. Übergeben Sie den erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält. Die maximale Anzahl von Lead-IDs ist 300.

Die Antwort enthält ein `result`-Array mit dem Status jeder Lead-ID in der Anfrage.

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

Verwenden [&#x200B; Endpunkts „Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) um Mitglieder einer Liste abzurufen. Übergeben Sie den erforderlichen `listId`. Sie können auch optionale Abfrageparameter übergeben, um Filterkriterien anzugeben.

Die optionalen Abfrageparameter sind:

- `batchSize`: Gibt die Anzahl der Lead-Datensätze an, die in einem Aufruf zurückgegeben werden sollen. Der Standard- und Höchstwert ist 300.
- `nextPageToken`: Paginiert durch große Ergebnismengen. Diesen Parameter aus dem ersten Aufruf auslassen und in nachfolgenden Aufrufen einschließen.
- `fields`: Gibt eine kommagetrennte Liste der zurückzugebenden Feldnamen an. Wenn Sie diesen Parameter nicht angeben, enthält die Antwort `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` und `id`.

Die Antwort enthält ein `result`-Array mit den in der Anfrage angegebenen Lead-Feldern.

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

Verwenden Sie den [Member of List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET)-Endpunkt, um festzustellen, ob ein oder mehrere Leads Mitglieder einer Liste sind. Übergeben Sie den erforderlichen `listId`-Pfadparameter und mindestens einen `id` Abfrageparameter, der Lead-IDs enthält. Die maximale Anzahl von Lead-IDs ist 300.

Die Antwort enthält ein `result`-Array mit dem Status jeder Lead-ID in der Anfrage.

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
