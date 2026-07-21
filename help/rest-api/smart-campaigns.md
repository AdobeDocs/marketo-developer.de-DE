---
title: Intelligente Kampagnen
feature: REST API, Smart Campaigns
description: Erfahren Sie, wie Sie Marketo-REST-APIs für Smart-Kampagnen verwenden, einschließlich Abfragen nach ID oder Namen, Durchsuchen von Filtern, Erstellen und Löschen von Klonen und Planen oder Anfordern von Triggern
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
TQID: https://experienceleague.adobe.com/iysRjtqd9plkreyIMuNjAF3YVFHtDUIrc-GInB4V8mg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1009
ht-degree: 1%

---

# Intelligente Kampagnen

[Endpunkt-Referenz für intelligente Kampagnen (Asset)](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns)

[Referenz zum Kampagnen-Endpunkt (Leads)](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

Verwenden Sie die REST-APIs für intelligente Kampagnen, um intelligente Kampagnen abzufragen, zu erstellen, zu klonen und zu löschen. Sie können auch Batch-Kampagnen planen, Trigger-Kampagnen anfordern und die Kampagnenaktivierung verwalten.

## Abfrage

Abfragen von Smart[Kampagnen (](#by_id)), [nach Name](#by_name) oder durch [Browsen](#browse).

### Nach ID

Der Endpunkt [Smart-Kampagne nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) nimmt eine einzelne Smart-Kampagnen-`id` als Pfadparameter und gibt einen einzelnen Smart-Kampagnen-Datensatz zurück.

```http
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

Der Endpunkt gibt einen Datensatz an der ersten Position des `result`-Arrays zurück.

### Nach Name

Der Endpunkt [Smart-Kampagne nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) nimmt eine einzelne Smart-Kampagnen-`name` als Parameter und gibt einen einzelnen Smart-Kampagnen-Datensatz zurück.

```http
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

Der Endpunkt gibt einen Datensatz an der ersten Position des `result`-Arrays zurück.

### Durchsuchen

Der Endpunkt [Smart-Kampagnen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) unterstützt optionale Abfrageparameter für Filterung und Paginierung.

Die Parameter `earliestUpdatedAt` und `latestUpdatedAt` akzeptieren `datetimes` im ISO-8601-Format (ohne Millisekunden). Wenn beide festgelegt sind, muss „frühestensUpdatedAt“ dem „latestUpdatedAt“ vorangehen.

Der Parameter `folder` gibt den zu durchsuchenden übergeordneten Ordner an. Übergeben Sie sie als JSON-Objekt, das `id` und `type` enthält.

Die `maxReturn` Ganzzahl gibt die maximale Anzahl von Einträgen an. Der Standardwert ist 20, der Maximalwert 200.

Die `offset` Ganzzahl gibt an, wo mit dem Abrufen von Einträgen begonnen werden soll. Verwenden Sie es mit `maxReturn`. Der Standardwert ist 0.

Legen Sie den `isActive` booleschen Parameter fest, sodass nur aktive Trigger-Kampagnen zurückgegeben werden.

```http
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

Der Endpunkt gibt einen oder mehrere Datensätze im `result`-Array zurück.

## Erstellen

Senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage an den Endpunkt [Smart-Kampagne erstellen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST). Die Parameter `name` und `folder` sind erforderlich. Übergeben Sie `folder` als JSON-Objekt, das `id` und `type` enthält.

Optional können Sie die Smart-Kampagne mit dem `description` Parameter (maximal 2.000 Zeichen) beschreiben.

```http
POST /rest/asset/v1/smartCampaigns.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage an den Endpunkt [Smart-Kampagne aktualisieren](https://developer.adobe.com/marketo-apis/api/asset). Der Parameter für den Smart-Campaign-`id` ist erforderlich. Verwenden Sie `name`, um den Namen zu ändern, oder `description`, um die Beschreibung zu ändern.

```http
POST /rest/asset/v1/smartCampaign/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
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

Senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage an den Endpunkt [Klonen der Smart](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5)Kampagne). Die Parameter `id`, `name` und `folder` sind erforderlich. Sie geben die Quellkampagne, den neuen Kampagnennamen und den übergeordneten Ordner an. Übergeben Sie `folder` als JSON-Objekt, das `id` und `type` enthält.

Optional können Sie die Smart-Kampagne mit dem `description` Parameter (maximal 2.000 Zeichen) beschreiben.

```http
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Der Endpunkt [Smart-Kampagne löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) akzeptiert eine einzelne Smart-Kampagnen-`id` als Pfadparameter.

```http
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

Batch-Kampagnen werden zu einem bestimmten Zeitpunkt ausgeführt und verarbeiten gemeinsam einen definierten Satz von Leads.

## Zeitplan

Verwenden [Kampagne planen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/scheduleCampaignUsingPOST) um eine Batch-Kampagne zu planen. Der Kampagnenparameter `id` Pfad ist erforderlich. Übergeben Sie die optionalen `tokens`-, `runAt`- und `cloneToProgram`-Parameter im Textkörper der JSON-Anfrage.

Das `tokens`-Array überschreibt die vorhandenen Programm-My-Token für diese Ausführung. Marketo verwirft die Überschreibungen nach der Ausführung der Kampagne. Jedes Element enthält ein Name/Wert-Paar, und der Token-Name muss das `{{my.name}}` Format verwenden.

Der `runAt` Datums-/Uhrzeitparameter gibt an, wann die Kampagne ausgeführt werden soll. Wenn dies ausgelassen wird, wird die Kampagne fünf Minuten nach der Anfrage ausgeführt. Der Wert darf in Zukunft nicht mehr als zwei Jahre betragen.

Über diese API geplante Kampagnen warten immer mindestens fünf Minuten, bevor sie ausgeführt werden.

Der `cloneToProgram` Zeichenfolgenparameter enthält den Namen eines resultierenden Programms.  Wenn dieser Wert festgelegt ist, werden die Kampagne, das übergeordnete Programm und alle zugehörigen Assets mit dem resultierenden neuen Namen erstellt. Das übergeordnete Programm wird geklont und die neu erstellte Kampagne wird geplant. Das resultierende Programm wird unter dem übergeordneten Element erstellt. Programme mit Snippets, Push-Benachrichtigungen, In-App-Nachrichten, statischen Listen, Berichten und Social-Media-Assets können auf diese Weise nicht geklont werden. Bei Verwendung ist dieser Endpunkt auf 20 Aufrufe pro Tag beschränkt. Der [Klonprogramm](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5)-Endpunkt ist die empfohlene Alternative.

```http
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

Trigger-Smart-Kampagnen verarbeiten jeweils nur eine Person als Reaktion auf ein Ereignis.

### Anfrage

Verwenden [Kampagne anfragen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/triggerCampaignUsingPOST) um Leads durch den Fluss einer Trigger-Kampagne zu leiten. Die Kampagne muss einen angeforderten Campaign-Trigger mit der Web-Service-API als Quelle verwenden.

Der Pfadparameter der `id` und ein `leads` ganzzahliges Array von Lead-IDs sind erforderlich. Jeder Aufruf akzeptiert maximal 100 Leads.

Optional kann der Parameter „Array `tokens`&quot; verwendet werden, um „Meine Token lokal“ für das übergeordnete Programm der Kampagne zu überschreiben. `tokens` akzeptiert maximal 100 Token. Jedes `tokens` Array-Element enthält ein Name/Wert-Paar. Der Name des Tokens muss als &quot;`{{my.name}}`&quot; formatiert sein. Wenn Sie den Ansatz [Hinzufügen eines System-Tokens als Link in einer E-Mail](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) verwenden, um das System-Token „viewAsWebpageLink“ hinzuzufügen, können Sie es nicht mit `tokens` überschreiben. Verwenden Sie stattdessen [&#x200B; Ansatz „Ansicht als Webseitenlink zu E-Mail hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) mit dem Sie „viewAsWebPageLink“ mit `tokens` überschreiben können.

Übergeben Sie die `leads` und `tokens` Parameter im Text der JSON-Anfrage.

```http
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

Der [Activate Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST)-Endpunkt ist einfach. Ein `id` ist erforderlich. Damit die Aktivierung erfolgreich ist, muss für die Kampagne Folgendes zutreffen:

- Die Kampagne ist deaktiviert.
- Die Kampagne weist mindestens einen Trigger und einen Flussschritt auf.
- Die Kampagne verfügt über fehlerfreie Trigger, Filter und Flussschritte.

```http
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

Die [Smart-Kampagne deaktivieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) ist unkompliziert. Ein `id` ist erforderlich. Damit die Deaktivierung erfolgreich ist, muss die Kampagne aktiviert werden.

```http
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
