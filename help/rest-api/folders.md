---
title: Ordner
feature: REST API
description: Bearbeiten von Ordnern mit der Marketo-API.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 1%

---

# Ordner

[Ordner-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Ordner sind das zentrale organisatorische Asset in Marketo, und jeder andere Asset-Typ hat mindestens einen Ordner als übergeordneten Asset. Dieser übergeordnete Ordner kann entweder ein rein organisatorischer Ordner oder ein Programm sein, das eine funktionale Beziehung zu anderen Asset-Typen hat und auch das übergeordnete Element anderer Assets sein kann. Ordner können über die API erstellt, abgefragt, aktualisiert und gelöscht werden und es kann auch eine Liste ihrer Inhalte abgerufen werden. Obwohl Programme über die Abfrage der Ordner-API zurückgegeben werden können, müssen das Erstellen, Aktualisieren und Löschen von Programmen über die Programme-API durchgeführt werden.

## Abfrage

Die Ordnerabfrage folgt den Standardabfragetypen für Assets von [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) und [Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

### Nach ID

```
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

Der Typparameter ist erforderlich und muss einer der Werte „Folder“ oder „Program“ sein.  Der Typ bestimmt, ob die Suche nach dem Ordner für eine Ordner-ID oder eine Programm-ID erfolgt. Für diesen Endpunkt wird im Ergebnis-Array nur ein einzelner Datensatz zurückgegeben. Beachten Sie den Parameter „folderType“ in der Antwort. Dies kann auf viele verschiedene Ordnertypen hinweisen. Marketo-Aktivitätsordner weisen entweder einen Typ von Marketing-Ordner oder -Programmen auf, der viele verschiedene Arten von Assets enthalten kann, während Design Studio-Ordner einen Typ haben, der dem Asset-Typ entspricht, den sie speichern können. Beispielsweise kann ein Ordner mit „folderType“ nur E-Mails oder andere Unterordner enthalten, die möglicherweise den „folderType“ E-Mail oder E-Mail-Vorlage haben. Mögliche Typen sind:

- E-Mail
- E-Mail-Vorlage
- Landingpage
- Landing Page-Vorlage
- Ausschnitt
- Datei

### Nach Name

[Abfrage nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) ist ebenfalls zulässig. Der Endpunkt „Abfrage nach Name“ hat den Namen als einzigen erforderlichen Parameter. Name führt eine exakte Zeichenfolgenübereinstimmung mit dem Namensfeld der Ordner in der Instanz durch und gibt Ergebnisse für jeden Ordner zurück, der diesem Namen entspricht. Darüber hinaus verfügt es über die optionalen Abfrageparameter „type“, d. h. Ordner oder Programm, „root“, die ID des zu durchsuchenden Ordners oder „workspace“ und den Namen des zu durchsuchenden Arbeitsbereichs. Wenn der Stammparameter festgelegt ist, muss auch der Typparameter festgelegt werden.

```
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Bei der Suche nach Namen ist es wichtig zu beachten, dass sowohl Marketing-Aktivitäten als auch Design Studio ihre eigenen Stammordner sind, sodass sie nach Namen abgerufen und verwendet werden können, um den Rest der Ordnerhierarchie in einer Zielinstanz zu durchlaufen.

### Durchsuchen

Ordner können auch [stapelweise abgerufen) ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). Mit dem Parameter „root“ kann der übergeordnete Ordner angegeben werden, unter dem die Abfrage ausgeführt wird, und er ist als JSON-Objekt formatiert, das als Wert für den Abfrageparameter eingebettet ist. Root hat zwei Mitglieder:

1. id : Die ID des Ordners oder Programms.
1. type - Entweder Ordner oder Programm, je nach dem Typ des Stammordners, der im Browser gespeichert werden soll.

Wenn der Stammordner nicht bekannt ist oder alle Ordner in einem bestimmten Bereich abgerufen werden sollen, kann der Stamm als Bereich „Marketing-Aktivitäten“, „Design Studio“ oder „Lead-Datenbank“ angegeben werden. Die IDs für jede dieser Variablen können über die API [Ordner nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) und den Namen des gewünschten Bereichs angeben.

Wie andere Endpunkte für den Massenabruf von Assets sind Offset und maxReturn optionale Parameter für das Paging.   Weitere optionale Parameter sind:

- workSpace : Der Name des Arbeitsbereichs, nach dem gefiltert werden soll.
- maxDepth : Die maximale Anzahl von Ebenen, die in der Ordnerhierarchie durchlaufen werden sollen. Wenn auf 0 gesetzt, wird nur der im Stammverzeichnis angegebene Ordner zurückgegeben. Wenn kein Wert angegeben ist, ist der Standardwert 2.

```
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Antwortstruktur

Ein Großteil der Antwortstruktur des Ordners ist selbsterklärend, aber einige Felder sind einzeln erwähnenswert. Die `folderId` und übergeordneten Felder sind JSON-Objekte, die die explizite ID und den Typ des Ordners selbst enthalten. Dieser Typ wird von der API in Abfragen, Stamm- und übergeordneten Parametern verwendet, um eine ordnungsgemäße Abgrenzung zwischen Ordner- und Programmtypen von Ordnern sicherzustellen. `folderType` spiegelt die Verwendung des Ordners wider, der einer der folgenden sein kann: „Marketing-Ordner“, „Programm“, „E-Mail“, „E-Mail-Vorlage“, „Landingpage“, Landingpage-Vorlage, „Ausschnitt“, „Bild“, „Zone“ oder „Datei“.  Die Typen Marketing-Ordner und -Programm geben an, dass sie in Marketing-Aktivitäten vorhanden sind und mehrere Arten von Assets enthalten können. Die anderen Typen geben an, dass sie möglicherweise nur diesen Asset-Typ, Unterordner und die Vorlagenversion dieses Typs enthalten. Der Zonentyp stellt die Ordner auf Stammebene dar, die in Marketing-Aktivitäten zu finden sind.

Der Pfad eines Ordners zeigt seine Hierarchie in der Ordnerstruktur an, ähnlich wie ein Pfad im Unix-Stil. Der erste Eintrag im Pfad ist immer Marketing-Aktivitäten oder Design Studio. Wenn die Zielinstanz über Arbeitsbereiche verfügt, ist der zweite Eintrag im Pfad der Name des zugehörigen Arbeitsbereichs. Das Feld `url` zeigt die explizite URL des Assets in der angegebenen Instanz an. Dies ist kein universeller Link und muss als Benutzer authentifiziert werden, damit er ordnungsgemäß funktioniert. `isSystem` gibt an, ob der Ordner ein Systemordner ist. Wenn dies auf „true“ festgelegt ist, ist der Ordner selbst schreibgeschützt, obwohl Ordner als untergeordnete Elemente davon erstellt werden können.

## Erstellen und aktualisieren

[Erstellen von Ordnern](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) ist einfach und wird mit einer application/x-www-form-urlencoded-POST ausgeführt, die zwei erforderliche Parameter, „name“, eine Zeichenfolge und „parent“, das übergeordnete Element zum Erstellen des Ordners, enthält. Dies ist ein eingebettetes JSON-Objekt mit zwei Elementen, „id“ und „type“, entweder „Folder“ oder „Program“, je nach Typ des Zielordners. Optional kann auch „description“, eine Zeichenfolge, eingeschlossen werden, die bis zu 2.000 Zeichen lang sein kann.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Die Aktualisierung von Ordnern erfolgt über einen separaten Endpunkt. Beschreibung, Name und `isArchive` sind optionale Parameter für die Aktualisierung. Wenn `isArchive` durch eine Aktualisierung geändert wird, führt dies dazu, dass der Ordner in der Marketo-Benutzeroberfläche archiviert wird, wenn er in „true“ geändert wurde, oder zu „unarchived“, wenn er in „false“ geändert wurde. Programme können mit dieser API nicht aktualisiert werden.

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

### Löschen

Für einzelne Ordner können Löschungen durchgeführt werden, wenn sie leer sind, d. h. wenn sie keine Assets oder Unterordner enthalten. Wenn ein Ordner vom Typ „Programm“ ist oder das Feld „isSystem“ auf „true“ gesetzt ist, kann er mit dieser API nicht gelöscht werden.

```
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
