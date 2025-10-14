---
title: Intelligente Listen
feature: REST API
description: Erfahren Sie, wie Sie mit Marketo-REST-APIs benutzerdefinierte Smart Lists abfragen, klonen und löschen können, einschließlich Endpunkten nach ID, Name, Kampagne und Programm mit Regeln.
exl-id: 4ba37e57-ee56-48c3-bb2b-b4ec8e907911
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 1%

---

# Intelligente Listen

[Endpunkt-Referenz für Smart Lists](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von Vorgängen für Smart-Listen. Diese APIs folgen dem Standard-Schnittstellenmuster für Asset-APIs und bieten Optionen für Abfrage, Löschen und Klonen.

Hinweis: Diese APIs werden nur für benutzerdefinierte Smart Lists unterstützt. Sie können nicht für [Integrierte/System-Smart-Listen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/using-smart-lists/use-built-in-system-smart-lists) verwendet werden.

## Abfrage

Die Abfrage von Smart-Listen folgt den Standardabfragetypen für Assets von [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) und [Durchsuchen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET).

### Nach ID

[Abfrage nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET) verwendet einen einzelnen Smart-Listen-`id` als Pfadparameter und gibt einen einzelnen Smart-Listen-Datensatz zurück. Optional können Sie den `includeRules` booleschen Parameter übergeben, um Smart-Listen-Regeln in die Antwort aufzunehmen.

![SmartList-Regeln](assets/smartlist-rules.png)

```
GET /rest/asset/v1/smartList/{id}.json?includeRules=true
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default",
            "rules": {
                "filterMatchType": "all",
                "triggers": [],
                "filters": [
                    {
                        "id": 459,
                        "name": "Visited Web Page",
                        "ruleTypeId": 1,
                        "ruleType": "Activity",
                        "operator": "occurs",
                        "conditions": [
                            {
                                "activityAttributeId": 1,
                                "activityAttributeName": "Web Page",
                                "operator": "is",
                                "values": [
                                    "Program Test.Landing Page Test 01"
                                ],
                                "isPrimary": true
                            },
                            {
                                "activityAttributeId": 6,
                                "activityAttributeName": "Browser",
                                "operator": "is",
                                "values": [
                                    "Chrome"
                                ],
                                "isPrimary": false
                            },
                            {
                                "activityAttributeId": -101,
                                "activityAttributeName": "Date of Activity",
                                "operator": "in past",
                                "values": [
                                    "30 days"
                                ],
                                "isPrimary": false
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

### Nach Smart-Kampagnen-ID

[Abfrage nach Smart-Kampagnen-](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartListBySmartCampaignIdUsingGET): Verwendet eine einzelne Smart-Kampagnen-`id` als Pfadparameter und gibt einen einzelnen Smart-Listen-Datensatz zurück. Optional können Sie den `includeRules` booleschen Parameter übergeben, um Smart-Listen-Regeln in die Antwort aufzunehmen.

```
GET /rest/asset/v1/smartCampaign/{smartCampaignId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Nach Programm-ID

[Abfrage nach Programm-ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getSmartListByProgramIdUsingGET) verwendet ein einzelnes E-Mail-Programm `id` als Pfadparameter und gibt einen einzelnen Smart-Listen-Datensatz zurück. Optional können Sie den `includeRules` booleschen Parameter übergeben, um Smart-Listen-Regeln in die Antwort aufzunehmen.

```
GET /rest/asset/v1/program/{programId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Nach Name

[Abfrage nach &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET)) akzeptiert eine Smart-Listen-`name` als Parameter und gibt einen einzelnen Smart-Listen-Datensatz zurück.  Es wird eine exakte Zeichenfolgenübereinstimmung für alle Smart-Listennamen in der Instanz durchgeführt und ein Ergebnis für die Smart-Liste zurückgegeben, die mit diesem Namen übereinstimmt.

```
GET /rest/asset/v1/smartList/byName.json?name=2018 Leads
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "115d7#16423bc13b4",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

### Durchsuchen

Smart-Listen können auch [in Batches abgerufen) &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET). Mit dem Parameter `folder` wird der übergeordnete Ordner angegeben, unter dem die Abfrage ausgeführt wird. Sie ist als JSON-Objekt formatiert, das `id` und `type` enthält. Wie andere Endpunkte für den Massenabruf von Assets sind `offset` und `maxReturn` optionale Parameter, die für das Paging verwendet werden können. Die optionalen `earliestUpdatedAt`- und `latestUpdatedAt` Datums-/Uhrzeitparameter können verwendet werden, um die Ergebnisse nach dem Datumsbereich UpdatedAt zu filtern.

```
GET /rest/asset/v1/smartLists.json?folder={"id":31,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "9aa4#16423c0e969",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 299697,
            "name": "Active Prospects",
            "createdAt": "2008-10-17T02:09:49Z+0000",
            "updatedAt": "2010-03-27T18:27:46Z+0000",
            "url": "https://app-abm.marketo.com/#SL299697A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 400517,
            "name": "Leads by Score",
            "createdAt": "2009-01-07T18:52:52Z+0000",
            "updatedAt": "2010-04-13T15:36:09Z+0000",
            "url": "https://app-abm.marketo.com/#SL400517A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Klon

[Klonen einer Smart-](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/cloneSmartListUsingPOST)) wird mit einem application/x-www-form-urlencoded POST ausgeführt. Die zu klonende Smart-Liste wird im `id` angegeben. Der Parameter `folder` wird verwendet, um den übergeordneten Ordner anzugeben, unter dem die Smart-Liste erstellt wird, und ist als JSON-Objekt mit der ID und dem Typ formatiert. Der übergeordnete Ordner muss entweder ein Programm- oder ein Smart List-Ordner sein. Der `name` Parameter wird zum Benennen der neuen Smart-Liste verwendet und muss eindeutig sein. Optional kann der `description` Parameter zur Beschreibung der Smart-Liste verwendet werden.

```
POST /rest/asset/v1/smartList/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":31,"type":"Folder"}&name=2018 Leads Qualified
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a672#16423d755ed",
    "result": [
        {
            "id": 788645,
            "name": "2018 Leads Qualified",
            "createdAt": "2018-06-21T19:34:32Z+0000",
            "updatedAt": "2018-06-21T19:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL788645A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Löschen

[Löschen einer Smart](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/deleteSmartListByIdUsingPOST)Liste) akzeptiert eine einzelne Smart-Listen-`id` als Pfadparameter.

```
POST /rest/asset/v1/smartList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "8f5#16423dd0fbe",
    "result": [
        {
            "id": 788645
        }
    ]
}
```
