---
title: Intelligente Kampagnen
feature: REST API, Smart Campaigns
description: Erfahren Sie, wie Sie Marketo-REST-APIs für Smart-Kampagnen verwenden, einschließlich Abfragen nach ID oder Namen, Durchsuchen von Filtern, Erstellen und Löschen von Klonen und Planen oder Anfordern von Triggern
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 1%

---

# Intelligente Kampagnen

[Endpunkt-Referenz für Smart-Kampagnen (Asset)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Referenz zum Kampagnen-Endpunkt (Leads)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von Vorgängen für Smart-Kampagnen. Diese APIs folgen dem Standard-Schnittstellenmuster für Asset-APIs und bieten Optionen für Abfragen, Erstellen, Klonen und Löschen. Außerdem können Sie die Ausführung intelligenter Kampagnen verwalten, indem Sie Batch-Kampagnen planen oder Trigger-Kampagnen anfordern.

## Abfrage

Die Abfrage von Smart-Kampagnen folgt den Standardabfragetypen für Assets von [nach ID](#by_id), [nach Name](#by_name) und [Browsen](#browse).

### Nach ID

Der Endpunkt [Smart-Kampagne nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) nimmt eine einzelne Smart-Kampagnen-`id` als Pfadparameter und gibt einen einzelnen Smart-Kampagnen-Datensatz zurück.

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

Bei diesem Endpunkt befindet sich immer ein einzelner Datensatz an der ersten Position des `result`-Arrays.

### Nach Name

Der Endpunkt [Smart-Kampagne nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) nimmt eine einzelne Smart-Kampagnen-`name` als Parameter und gibt einen einzelnen Smart-Kampagnen-Datensatz zurück.

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

Bei diesem Endpunkt befindet sich immer ein einzelner Datensatz an der ersten Position des `result`-Arrays.

### Durchsuchen

Der Endpunkt [Smart-Kampagnen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) funktioniert wie andere Asset-API-Browser-Endpunkte und ermöglicht es mehreren optionalen Abfrageparametern, Filterkriterien anzugeben.

Die Parameter `earliestUpdatedAt` und `latestUpdatedAt` akzeptieren `datetimes` im ISO-8601-Format (ohne Millisekunden). Wenn beide festgelegt sind, muss „frühestensUpdatedAt“ dem „latestUpdatedAt“ vorangehen.

Der Parameter `folder` gibt den übergeordneten Ordner an, unter dem durchsucht werden soll. Das Format ist ein JSON-Block mit `id` und `type` Attributen.

Der `maxReturn` ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt. Der Standardwert ist 20. Maximal 200.

Der `offset` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann zusammen mit `maxReturn` verwendet werden. Der Standardwert ist 0.

Der `isActive` ist ein boolescher Wert, der angibt, dass nur aktive Trigger-Kampagnen zurückgegeben werden sollen.

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

Bei diesem Endpunkt gibt es einen oder mehrere Datensätze im `result`-Array.

## Erstellen

Der [Create Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST)-Endpunkt wird mit einem application/x-www-form-urlencoded POST mit zwei erforderlichen Parametern ausgeführt. Der Parameter `name` gibt den Namen der zu erstellenden Smart-Kampagne an. Der Parameter `folder` gibt den übergeordneten Ordner an, in dem die intelligente Kampagne erstellt wird. Das Format ist ein JSON-Block mit `id` und `type` Attributen.

Optional können Sie die Smart-Kampagne mit dem `description` Parameter (maximal 2.000 Zeichen) beschreiben.

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

## Update

Der [Update Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/)-Endpunkt wird mit einem application/x-www-form-urlencoded POST ausgeführt. Eine einzelne Smart-Campaign-`id` ist als Pfadparameter erforderlich. Sie können den `name`-Parameter verwenden, um den Namen der intelligenten Kampagne zu aktualisieren, oder den `description`-Parameter, um die Beschreibung der intelligenten Kampagne zu aktualisieren.

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

## Klon

Der Endpunkt [Klonen der Smart](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)Kampagne) wird mit einem application/x-www-form-urlencoded POST mit drei erforderlichen Parametern ausgeführt. Dazu benötigt man einen `id`, der die zu klonende Smart-Kampagne angibt, einen `name`, der den Namen der neuen Smart-Kampagne angibt, und einen `folder`, der den übergeordneten Ordner angibt, in dem die neue Smart-Kampagne erstellt wird. Das Format ist ein JSON-Block mit `id` und `type` Attributen.

Optional können Sie die Smart-Kampagne mit dem `description` Parameter (maximal 2.000 Zeichen) beschreiben.

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

Der Endpunkt [Smart-Kampagne löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) akzeptiert eine einzelne Smart-Kampagnen-`id` als Pfadparameter.

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

## Batch

Batch-Kampagnen werden zu einem bestimmten Zeitpunkt gestartet und wirken sich auf eine bestimmte Gruppe von Leads gleichzeitig aus.

## Zeitplan

Verwenden Sie den Endpunkt [Kampagne planen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) um eine Batch-Kampagne so zu planen, dass sie entweder sofort oder zu einem späteren Zeitpunkt ausgeführt wird. Der Kampagnenparameter `id` ein erforderlicher Pfadparameter. Optionale Parameter sind `tokens`, `runAt` und `cloneToProgram`, die im Anfragetext als application/json übergeben werden.

Der Parameter Token-Array ist ein Array von „Meine Token“, das vorhandene Programm-Token überschreibt. Nach der Ausführung der Kampagne werden die Token verworfen.  Jedes Token-Array-Element enthält Name/Wert-Paare. Der Name des Tokens muss als &quot;{{my.name}}&quot; formatiert sein.

Der Parameter „runAt“ datetime gibt an, wann die Kampagne ausgeführt werden soll. Wenn nichts angegeben wird, wird die Kampagne 5 Minuten nach dem Aufruf des Endpunkts ausgeführt. Der Datums-/Uhrzeitwert darf nicht mehr als zwei Jahre in der Zukunft liegen.

Über diese API geplante Kampagnen warten immer mindestens fünf Minuten, bevor sie ausgeführt werden.

Der `cloneToProgram` Zeichenfolgenparameter enthält den Namen eines resultierenden Programms.  Wenn dieser Wert festgelegt ist, werden die Kampagne, das übergeordnete Programm und alle zugehörigen Assets mit dem resultierenden neuen Namen erstellt. Das übergeordnete Programm wird geklont und die neu erstellte Kampagne wird geplant. Das resultierende Programm wird unter dem übergeordneten Element erstellt. Programme mit Snippets, Push-Benachrichtigungen, In-App-Nachrichten, statischen Listen, Berichten und Social-Media-Assets können auf diese Weise nicht geklont werden. Bei Verwendung ist dieser Endpunkt auf 20 Aufrufe pro Tag beschränkt. Der [Klonprogramm](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)-Endpunkt ist die empfohlene Alternative.

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

Trigger-Smart-Kampagnen wirken sich auf der Grundlage eines ausgelösten Ereignisses jeweils auf eine Person aus.

### Anfrage

Verwenden Sie den [Kampagne anfragen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST)-Endpunkt, um eine Reihe von Leads an eine Trigger-Kampagne zu übergeben, die den Kampagnenfluss durchläuft. Die Kampagne muss einen Trigger „Kampagne ist angefordert“ mit „Webservice-API“ als Quelle haben.

Dieser Endpunkt erfordert einen Campaign-`id` als Pfadparameter und einen `leads` ganzzahligen Array-Parameter mit Lead-IDs . Pro Aufruf sind maximal 100 Leads zulässig.

Optional kann der Parameter „Array `tokens`&quot; verwendet werden, um „Meine Token lokal“ für das übergeordnete Programm der Kampagne zu überschreiben. `tokens` akzeptiert maximal 100 Token. Jedes `tokens` Array-Element enthält ein Name/Wert-Paar. Der Name des Tokens muss als &quot;{{my.name}}&quot; formatiert sein. Wenn Sie den Ansatz [Hinzufügen eines System-Tokens als Link in einer E-Mail](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) verwenden, um das System-Token „viewAsWebpageLink“ hinzuzufügen, können Sie es nicht mit `tokens` überschreiben. Verwenden Sie stattdessen [&#x200B; Ansatz „Ansicht als Webseitenlink zu E-Mail hinzufügen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) mit dem Sie „viewAsWebPageLink“ mit `tokens` überschreiben können.

Die Parameter `leads` und `tokens` werden im Anfragetext als application/json übergeben.

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

Der [Activate Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST)-Endpunkt ist einfach. Ein `id` ist erforderlich. Damit die Aktivierung erfolgreich ist, muss für die Kampagne Folgendes zutreffen:

- Muss deaktiviert werden
- Muss mindestens einen Trigger und einen Flussschritt haben
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

Die [Smart-Kampagne deaktivieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) ist unkompliziert. Ein `id` ist erforderlich. Damit die Deaktivierung erfolgreich ist, muss die Kampagne aktiviert werden.

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
