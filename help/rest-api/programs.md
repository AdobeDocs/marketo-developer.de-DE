---
title: Programme
feature: REST API, Programs
description: Erstellen und Bearbeiten von Programminformationen.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 2%

---

# Programme

[Endpunkt-Referenz für Programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

Programme sind eine zentrale organisatorische Komponente der Marketo-Marketing-Aktivitäten. Sie können den meisten Asset-Typen übergeordnet sein und die Mitgliedschaft und den Erfolg von Leads im Kontext einzelner Marketing-Initiativen verfolgen. Programme können allen Datensatztypen übergeordnet sein, mit Ausnahme von IP-Adressen, E-Mail-Vorlagen und Dateien.

## Programmtypen

In Marketo gibt es fünf Kerntypen von Programmen:

- Standard
- Veranstaltung
- Veranstaltung mit Webinar
- Interaktion
- E-Mail

Interaktionsprogramme können übergeordnete Elemente jeder anderen Art von Programm sein, während Standard, Ereignis und Webinar nur übergeordnete Elemente von E-Mail-Programmen sein können.

Programme haben immer einen Kanal. Sie leiten die möglichen eingerichteten Programmmitgliedstaaten von dem Kanal ab, mit dem sie erstellt wurden. Diese können mit der GET Channels-API abgerufen werden. Einem Programm kann auch eine Reihe von zugehörigen Tags zugeordnet sein. Tags sind anpassbare Felder, die als optional konfiguriert oder für einen bestimmten Programmtyp erforderlich konfiguriert werden können. Dabei wird ein Wert aus einer in Marketo Admin konfigurierten Liste ausgewählt.

## Abfrage

Programme folgen dem Standardmuster für Asset-Abfragen mit einer zusätzlichen Option, um nach Tag-Typ und Werten abzufragen. Verfügbare Tags und Werte können mit &quot;[ Tag-Typen abrufen“ ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Nach ID

Der [Programm nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)-Endpunkt erfordert einen `id`.

Die Programm-ID erhalten Sie von der URL des Programms in der Benutzeroberfläche, wo die URL `https://app-\*\*\*.marketo.com/#PG1001A1` ähnelt. In dieser URL ist der `id` 1001. Der Wert liegt immer zwischen dem ersten Buchstabensatz in der URL und dem zweiten Buchstabensatz.

```
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

Der [Programm nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/)-Endpunkt erfordert einen `name` Abfrageparameter. Optionale boolesche Abfrageparameter sind `includeTags` und `includeCosts`, mit denen Programm-Tags bzw. Programmkosten zurückgegeben werden.

```
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

Mit [ Endpunkt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)Programme abrufen“ können Sie nach Programmen suchen.

Mit dem optionalen `status` können Sie nach dem Programmstatus filtern. Dieser Parameter gilt nur für Interaktions- und E-Mail-Programme. Die möglichen Werte sind „on“ und „off“ für Interaktionsprogramme und „unlocked“ für E-Mail-Programme.

Der optionale Parameter `maxReturn` steuert die Anzahl der zurückzugebenden Programme (maximal 200, standardmäßig 20). Der optionale `offset`, der für Paging-Ergebnisse verwendet wird (der Standardwert ist 0).

Beachten Sie, dass mit einem Programm verknüpfte Tags von diesem Endpunkt nicht zurückgegeben werden. Programm-Tags können entweder mit [Programme nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) oder [Programme nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) abgerufen werden.

```
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

Mit den `earliestUpdatedAt`- und `latestUpdatedAt`-Parametern für unseren [Get Programs](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5)-Endpunkt können Sie Wasserzeichen für niedrige und hohe Datums- und Uhrzeitwerte für zurückgegebene Programme festlegen, die innerhalb des angegebenen Bereichs entweder aktualisiert oder anfänglich erstellt wurden.

```
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

Der Endpunkt [Programme nach Tag abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) ruft eine Liste von Programmen ab, die dem Tag-Typ und den bereitgestellten Tag-Werten entsprechen.

Es gibt zwei erforderliche Parameter, `tagType` den Typ des Tags, nach dem gefiltert werden soll, und `tagValue` den Tag-Wert, nach dem gefiltert werden soll.  Es gibt einen optionalen ganzzahligen `maxReturn`, der die Anzahl der zurückzugebenden Programme steuert (maximal ist 200, standardmäßig 20), und einen optionalen ganzzahligen `offset`, der für Paging-Ergebnisse verwendet wird (standardmäßig 0).  Die Ergebnisse werden in zufälliger Reihenfolge zurückgegeben.

```
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

[Erstellen]https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) und [Aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) von Programmen folgt dem Standard-Asset-Muster und verfügt über `folder`, `name`, `type` und `channel` als erforderliche Parameter, wobei `description`, `costs` und `tags` optional sind. Kanal und Typ können nur bei der Programmerstellung festgelegt werden. Nur Beschreibung, Name, `tags` und `costs` können nach der Erstellung aktualisiert werden, wobei ein zusätzlicher `costsDestructiveUpdate` zulässig ist. Wenn Sie `costsDestructiveUpdate` als „true“ übergeben, werden alle vorhandenen Kosten gelöscht und durch alle im Aufruf enthaltenen Kosten ersetzt. Beachten Sie, dass Tags für einige Programmtypen in einigen Abonnements erforderlich sein können. Dies ist jedoch konfigurationsabhängig und sollte zunächst mit Tags abrufen überprüft werden, um zu sehen, ob instanzspezifische Anforderungen vorliegen.

Beim Erstellen oder Aktualisieren eines E-Mail-Programms können auch ein `startDate` und ein `endDate` übergeben werden.

### Erstellen

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Wenn Sie die Programmkosten aktualisieren, fügen Sie sie einfach Ihrem `costs`-Array hinzu, um neue Kosten anzuhängen. Um eine destruktive Aktualisierung durchzuführen, übergeben Sie Ihre neuen Kosten zusammen mit dem Parameter `costsDestructiveUpdate` auf `true` gesetzt. Um alle Kosten aus einem Programm zu löschen, übergeben Sie keinen `costs` Parameter und übergeben Sie `costsDestructiveUpdate` auf `true` gesetzt.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

E-Mail-Programme können remote genehmigt oder nicht genehmigt werden. Das führt dazu, dass das Programm zum angegebenen Startdatum ausgeführt und zum angegebenen Enddatum abgeschlossen wird. Beide müssen so eingestellt sein, dass sie das Programm genehmigen, und über die Benutzeroberfläche eine gültige und genehmigte E-Mail-Adresse sowie eine Smart-Liste konfigurieren.

### Genehmigen

```
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

```
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

[Klonen von Programmen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) folgt dem Standard-Asset-Muster, dem neuen Namen und Ordner als erforderliche Parameter und einer optionalen Beschreibung.  Der `name` muss global eindeutig sein und darf 255 Zeichen nicht überschreiten.  Der `folder` ist der übergeordnete Ordner.  Das Attribut für den `folder`-Parametertyp muss auf „Ordner“ festgelegt sein und der Zielordner muss sich im selben Arbeitsbereich wie das Programm befinden, das geklont wird.

Programme, die bestimmte Asset-Typen enthalten, können über diese API nicht geklont werden, einschließlich Push-Benachrichtigungen, In-App-Nachrichten, Berichten und Social Assets. In-App-Programme dürfen nicht über diese API geklont werden.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

```
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
