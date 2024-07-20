---
title: Intelligente Kampagnen
feature: REST API, Smart Campaigns
description: Übersicht über Smart-Kampagnen
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---

# Intelligente Kampagnen

[Endpoint-Referenz für Smart-Kampagnen (Asset)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Kampagnen-Endpunktverweis (Leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von Vorgängen für Smart-Kampagnen. Diese APIs entsprechen dem Standardmuster der Benutzeroberfläche für Asset-APIs, die Optionen zum Abfragen, Erstellen, Klonen und Löschen bereitstellen. Außerdem können Sie die Ausführung von Smart-Kampagnen verwalten, indem Sie Batch-Kampagnen planen oder Trigger-Kampagnen anfordern.

## Anfrage

Beim Abfragen von Smart-Kampagnen werden die standardmäßigen Abfragetypen für Assets von [by id](#by_id), [by name](#by_name) und [browsing](#browse) verwendet.

### Nach ID

Der Endpunkt [Smart-Kampagne mit ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) verwendet eine einzelne Smart-Kampagne `id` als Pfadparameter und gibt einen einzelnen Smart-Campaign-Datensatz zurück.

```
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

Bei diesem Endpunkt befindet sich immer ein einzelner Datensatz an der ersten Position des `result` -Arrays.

### Nach Name

Der Endpunkt [Smart-Kampagne nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) akzeptiert eine einzelne Smart-Kampagne `name` als Parameter und gibt einen einzelnen Smart-Campaign-Datensatz zurück.

```
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

Bei diesem Endpunkt befindet sich immer ein einzelner Datensatz an der ersten Position des `result` -Arrays.

### Durchsuchen

Der Endpunkt [Smart-Kampagnen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) funktioniert wie andere Asset-API-Durchsuchen-Endpunkte und ermöglicht es mehreren optionalen Abfrageparametern, Filterkriterien anzugeben.

Die Parameter `earliestUpdatedAt` und `latestUpdatedAt` akzeptieren `datetimes` im ISO-8601-Format (ohne Millisekunden). Wenn beide festgelegt sind, muss &quot;frühestUpdatedAt&quot;aktuellerUpdatedAt vorangehen.

Der Parameter `folder` gibt den übergeordneten Ordner an, unter dem Sie suchen möchten. Das Format ist ein JSON-Block mit den Attributen `id` und `type`.

Der Parameter `maxReturn` ist eine Ganzzahl, die die maximale Anzahl an zurückzugebenden Einträgen angibt. Der Standardwert ist 20. Maximal 200.

Der Parameter `offset` ist eine Ganzzahl, die angibt, wo Einträge abgerufen werden sollen. Kann zusammen mit `maxReturn` verwendet werden. Der Standardwert ist 0.

Der Parameter `isActive` ist ein boolescher Wert, der angibt, dass nur aktive Trigger-Kampagnen zurückgegeben werden.

```
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

Mit diesem Endpunkt befinden sich ein oder mehrere Datensätze im Array `result` .

## Erstellen

Der Endpunkt [Smart-Kampagne erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) wird mit einer application/x-www-form-urlencoded -POST mit zwei erforderlichen Parametern ausgeführt. Der Parameter `name` gibt den Namen der zu erstellenden Smart-Kampagne an. Der Parameter `folder` gibt den übergeordneten Ordner an, in dem die Smart-Kampagne erstellt wird. Das Format ist ein JSON-Block mit den Attributen `id` und `type`.

Optional können Sie die Smart-Kampagne mit dem Parameter `description` (maximal 2.000 Zeichen) beschreiben.

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Aktualisierung

Der Endpunkt [Smart-Kampagne aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/) wird mit einer anwendungs-/x-www-form-urlencoded -POST ausgeführt. Es nimmt eine einzelne Smart-Kampagne `id` als Pfadparameter an. Sie können den Parameter `name` verwenden, um den Namen der Smart-Kampagne zu aktualisieren, oder den Parameter `description` , um die Beschreibung der Smart-Kampagne zu aktualisieren.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Klonen

Der Endpunkt [Smart-Kampagne klonen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) wird mit einer application/x-www-form-urlencoded -POST mit drei erforderlichen Parametern ausgeführt. Es werden ein Parameter `id` benötigt, der die zu klonende Smart-Kampagne angibt, ein Parameter `name`, der den Namen der neuen Smart-Kampagne angibt, und ein Parameter `folder` , um den übergeordneten Ordner anzugeben, in dem die neue Smart-Kampagne erstellt wird. Das Format ist ein JSON-Block mit den Attributen `id` und `type`.

Optional können Sie die Smart-Kampagne mit dem Parameter `description` (maximal 2.000 Zeichen) beschreiben.

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Löschen

Der Endpunkt [Smart-Kampagne löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) verwendet eine einzelne Smart-Kampagne `id` als Pfadparameter.

```
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Stapel

Batch-Smart-Kampagnen starten zu einem bestimmten Zeitpunkt und wirken sich auf einen bestimmten Satz von Leads alle auf einmal aus.

## Zeitplan

Verwenden Sie den Endpunkt [Planung der Kampagne](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) , um die Ausführung einer Batch-Kampagne entweder sofort oder zu einem späteren Zeitpunkt zu planen. Die Kampagne `id` ist ein erforderlicher Pfadparameter. Optionale Parameter sind `tokens`, `runAt` und `cloneToProgram`, die im Anfrageinhalt als application/json übergeben werden.

Der Token-Array-Parameter ist ein Array von My Tokens , das vorhandene Programm-Token außer Kraft setzt. Nach Ausführung der Kampagne werden die Token verworfen.  Jedes Token-Array-Element enthält Name/Wert-Paare. Der Name des Tokens muss als &quot;{{my.name}}&quot; formatiert sein.

Der Parameter runAt datetime gibt an, wann die Kampagne ausgeführt werden soll. Wenn kein Wert angegeben wird, wird die Kampagne fünf Minuten nach dem Aufruf des Endpunkts ausgeführt. Der Wert für den Datum/Uhrzeit darf nicht länger als zwei Jahre in die Zukunft sein.

Über diese API geplante Kampagnen warten immer mindestens fünf Minuten, bevor sie ausgeführt werden.

Der String-Parameter `cloneToProgram` enthält den Namen eines resultierenden Programms.  Wenn diese Einstellung festgelegt ist, werden die Kampagne, das übergeordnete Programm und alle Assets mit dem resultierenden neuen Namen erstellt. Das übergeordnete Programm wird geklont und die neu erstellte Kampagne wird geplant. Das resultierende Programm wird unter dem übergeordneten Element erstellt. Programme mit Snippets, Push-Benachrichtigungen, In-App-Nachrichten, statischen Listen, Berichten und Social-Assets werden möglicherweise nicht auf diese Weise geklont. Bei Verwendung ist dieser Endpunkt auf 20 Aufrufe pro Tag beschränkt. Der Endpunkt [clone program](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) ist die empfohlene Alternative.

```
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Auslöser

Smart-Kampagnen für Trigger wirken sich auf Basis eines ausgelösten Ereignisses auf eine Person aus.

### Anfrage

Verwenden Sie den Endpunkt [Kampagne anfordern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) , um eine Reihe von Leads an eine Trigger-Kampagne zu übergeben, damit sie durch den Kampagnenfluss ausgeführt werden kann. Die Kampagne muss über den Trigger &quot;Kampagne ist angefordert&quot;verfügen, wobei &quot;Web Service-API&quot;als Quelle dient.

Dieser Endpunkt erfordert eine Kampagne `id` als Pfadparameter und einen `leads` Integer-Array-Parameter, der Lead-IDs enthält. Pro Aufruf sind maximal 100 Leads zulässig.

Optional kann der Array-Parameter `tokens` verwendet werden, um My Tokens local in das übergeordnete Programm der Kampagne zu überschreiben. `tokens` akzeptiert maximal 100 Token. Jedes `tokens` -Array-Element enthält ein Name/Wert-Paar. Der Name des Tokens muss als &quot;{{my.name}}&quot; formatiert sein. Wenn Sie [ein System-Token als Link in einem E-Mail-Ansatz hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) verwenden, um das System-Token &quot;viewAsWebpageLink&quot;hinzuzufügen, können Sie es nicht mit `tokens` überschreiben. Verwenden Sie stattdessen den Ansatz [Als Webseitenlink anzeigen für einen E-Mail-Ansatz](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) , mit dem Sie &quot;viewAsWebPageLink&quot;mit `tokens` überschreiben können.

Die Parameter `leads` und `tokens` werden im Anfrageinhalt als application/json übergeben.

```
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Aktivieren

Der Endpunkt [Smart-Kampagne aktivieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) ist unkompliziert. Ein `id` -Pfadparameter ist erforderlich. Damit die Aktivierung erfolgreich ist, muss für die Kampagne Folgendes zutreffen:

- Muss deaktiviert werden
- Muss mindestens einen Trigger und einen Flussschritt enthalten
- Muss fehlerfreie Trigger, Filter und Flussschritte aufweisen

```
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Deaktivieren

Die Funktion [Smart-Kampagne deaktivieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) ist unkompliziert. Ein `id` -Pfadparameter ist erforderlich. Damit die Deaktivierung erfolgreich ist, muss die Kampagne aktiviert werden.

```
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
