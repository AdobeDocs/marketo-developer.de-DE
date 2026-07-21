---
title: Programme
feature: REST API, Programs
description: Marketo-Programmhandbuch für die Asset-REST-API, das Typen, Kanäle, Tags, Mitgliedschaftsstatus und Endpunkte abdeckt, um nach ID oder Namen zu erhalten, zu durchsuchen und nach Status zu filtern.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
TQID: https://experienceleague.adobe.com/5ILyahSn3Pp-lF6YPogVnkXjXP-QLtEmyLm7iKMIgo0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 741
ht-degree: 2%

---

# Programme

[Endpunkt-Referenz für Programme](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs)

Programme organisieren Marketo-Marketing-Aktivitäten und verfolgen die Lead-Mitgliedschaft und den Erfolg einzelner Marketing-Initiativen. Ein Programm kann die meisten Asset-Typen enthalten, mit Ausnahme von Landingpages, E-Mail-Vorlagen und Dateien.

## Programmtypen

In Marketo gibt es fünf Kerntypen von Programmen:

- Standard
- Ereignis
- Veranstaltung mit Webinar
- Interaktion
- E-Mail

Interaktionsprogramme können jeden anderen Programmtyp enthalten. Standard-, Ereignis- und Ereignisprogramme mit Webinar-Programmen können nur E-Mail-Programme enthalten.

Jedes Programm hat einen Kanal. Der Kanal definiert die verfügbaren Programmmitgliedsstatus und kann mit der Get Channels-API abgerufen werden.

Ein Programm kann auch über Tags verfügen. Tags sind anpassbare Felder, die für einen Programmtyp optional oder erforderlich sein können. Jedes Tag verwendet einen Wert aus einer Liste, die in Marketo Admin konfiguriert ist.

## Abfrage

Abfragen von Programmen nach ID, Name, Browser oder Tag-Typ und Wert. Verwenden Sie [Tag-Typen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags/operation/getTagTypesUsingGET) um verfügbare Tags und Werte abzurufen.

### Nach ID

Der [Programm nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5)-Endpunkt erfordert einen `id`.

Sie können die Programm-ID über die URL der Benutzeroberfläche abrufen, z. B. `https://app-\*\*\*.marketo.com/#PG1001A1`. In diesem Beispiel wird die ID zwischen dem ersten und dem zweiten Buchstabensatz `1001`.

```http
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Nach Name

Der [Programm nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset)-Endpunkt erfordert einen `name` Abfrageparameter. Legen Sie die optionalen booleschen Parameter `includeTags` und `includeCosts` fest, um Tags bzw. Kosten zurückzugeben.

```http
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Durchsuchen

Verwenden Sie den Endpunkt [Programme abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) um Programme zu durchsuchen.

Der optionale `status` filtert Interaktions- und E-Mail-Programme nach Status. Gültige Werte sind `on` und `off` für Interaktionsprogramme und `unlocked` für E-Mail-Programme.

Der optionale Parameter `maxReturn` steuert die Anzahl der zurückgegebenen Programme. Der Standardwert ist 20, der Maximalwert 200. Verwenden Sie den optionalen `offset` für die Paginierung. Der Standardwert ist 0.

Dieser Endpunkt gibt keine Programm-Tags zurück. Rufen Sie Tags mit [Programme nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) oder [Programme nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) ab.

```http
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Nach Datumsbereich

Verwenden Sie die `earliestUpdatedAt`- und `latestUpdatedAt` mit [Programme abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5), um niedrige und hohe Datums-/Uhrzeitgrenzen festzulegen. Der Endpunkt gibt Programme zurück, die innerhalb des Bereichs erstellt oder aktualisiert wurden.

```http
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Nach Tag-Typ

Der Endpunkt [Programme nach Tag abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramListByTagUsingGET) gibt Programme zurück, die dem angegebenen Tag-Typ und -Wert entsprechen.

Die Parameter `tagType` und `tagValue` sind erforderlich. Die optionale Ganzzahl `maxReturn` steuert die Anzahl der zurückgegebenen Programme; der Standardwert ist 20 und der Höchstwert ist 200. Verwenden Sie den optionalen ganzzahligen `offset` für die Paginierung. Der Standardwert ist 0. Die Ergebnisse werden in zufälliger Reihenfolge zurückgegeben.

```http
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Erstellen und aktualisieren

[Zum &#x200B;](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/createProgramUsingPOST) eines Programms sind `folder`, `name`, `type` und `channel` erforderlich. Die optionalen Parameter sind `description`, `costs` und `tags`. Einige Abonnements erfordern Tags für bestimmte Programmtypen. Verwenden Sie Tags abrufen , um die Instanzanforderungen zu überprüfen.

Beim [Aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST) können Sie nur die Beschreibung, den Namen, die `tags` und die `costs` ändern. Sie können den Kanal und den Typ nur während der Erstellung festlegen. Wenn Sie `costsDestructiveUpdate` auf `true` setzen, werden alle bestehenden Kosten gelöscht und durch die in der Anfrage enthaltenen Kosten ersetzt.

Beim Erstellen oder Aktualisieren eines E-Mail-Programms können `startDate` und `endDate` auch als UTC-Datum/-Uhrzeit übergeben werden:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Erstellen

```http
POST /rest/asset/v1/programs.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Update

Fügen Sie die Programmkosten zum `costs`-Array hinzu. Um bestehende Kosten zu ersetzen, übergeben Sie die neuen Kosten und setzen `costsDestructiveUpdate` auf `true`. Um alle Kosten zu löschen, lassen Sie `costs` weg und setzen `costsDestructiveUpdate` auf `true`.

```http
POST /rest/asset/v1/program/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Genehmigung

Sie können E-Mail-Programme aus der Ferne genehmigen oder die Genehmigung aufheben. Ein genehmigtes Programm läuft `startDate` und endet an seiner `endDate`.

Legen Sie vor der Genehmigung in der Benutzeroberfläche beide Datumswerte fest und konfigurieren Sie eine gültige, genehmigte E-Mail-Adresse und Smart-Liste.

### Genehmigen

```http
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Genehmigung aufheben

```http
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Klonen

[Klonen von Programmen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/cloneProgramUsingPOST) erfordert einen neuen Namen und einen übergeordneten Ordner. Die Beschreibung ist optional. Der `name` muss global eindeutig sein und darf 255 Zeichen nicht überschreiten.

Setzen Sie das Typattribut des `folder` auf `Folder`. Der Zielordner muss sich im selben Arbeitsbereich wie das Quellprogramm befinden.

Sie können diese API nicht verwenden, um In-App-Programme oder Programme zu klonen, die Push-Benachrichtigungen, In-App-Nachrichten, Berichte oder soziale Assets enthalten.

```http
POST /rest/asset/v1/program/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Programm löschen

Das Löschen von Programmen erfolgt nach dem standardmäßigen Asset-Löschmuster.

```http
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```
