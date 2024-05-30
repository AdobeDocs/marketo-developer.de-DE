---
title: "Ordner"
feature: REST API
description: '"Bearbeiten von Ordnern mit der Marketo-API".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 1%

---


# Ordner

[Ordner-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Ordner sind das zentrale organisatorische Asset in Marketo und jeder andere Asset-Typ hat mindestens einen Ordner als übergeordneten Ordner. Dieser übergeordnete Ordner kann entweder ein Ordner sein, der rein organisatorisch ist, oder ein Programm, das eine funktionale Beziehung zu anderen Asset-Typen aufweist und auch über andere Assets verfügen kann. Ordner können über die API erstellt, abgefragt, aktualisiert und gelöscht werden und außerdem das Abrufen einer Liste ihrer Inhalte zulassen. Programme können zwar durch Abfrage der Ordner-API zurückgegeben werden, das Erstellen, Aktualisieren und Löschen von Programmen muss jedoch über die Programm-API erfolgen.

## Anfrage

Die Abfrage von Ordnern folgt den standardmäßigen Abfragetypen für Assets von [by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET), und [Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

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

Der Typparameter ist erforderlich und muss &quot;Ordner&quot;oder &quot;Programm&quot;lauten.  Der Typ bestimmt, ob die Suche nach dem Ordner mit einer Ordner-ID oder einer Programm-ID erfolgt. Für diesen Endpunkt wird nur ein einzelner Datensatz im Ergebnisarray zurückgegeben. Notieren Sie den Parameter folderType in der Antwort. Dies kann auf viele verschiedene Ordnertypen hinweisen. Marketo-Aktivitätsordner verfügen entweder über einen Typ von Marketing-Ordner oder Programm, der viele verschiedene Asset-Typen enthalten kann, während Design Studio-Ordner einen Typ haben, der dem Asset-Typ entspricht, den sie enthalten können. Beispielsweise kann ein Ordner mit dem Ordnertyp &quot;E-Mail&quot;nur E-Mails oder andere Unterordner enthalten, die möglicherweise einen Ordner vom Typ E-Mail oder E-Mail-Vorlage haben. Zu den Typen können gehören:

- E-Mail
- E-Mail-Vorlage
- Landingpage
- Landing Page-Vorlage
- Ausschnitt
- Datei

### Nach Name

[Abfrage nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) ist auch erlaubt. Die Abfrage nach Name-Endpunkt hat den Namen als einzigen erforderlichen Parameter. Der Name führt eine exakte Zeichenfolgenübereinstimmung mit dem Namensfeld von Ordnern in der Instanz durch und gibt Ergebnisse für jeden Ordner zurück, der mit diesem Namen übereinstimmt. Es enthält auch die optionalen Abfrageparameter &quot;type&quot;, die Ordner oder Programm sein können, die Kennung des Ordners, der durchsucht werden soll, &quot;root&quot; oder den Namen des Arbeitsbereichs, in dem gesucht werden soll, &quot;workspace&quot;. Wenn der Root-Parameter festgelegt ist, muss auch der type-Parameter festgelegt werden.

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

Bei der Suche nach Namen ist es wichtig zu beachten, dass sowohl Marketing Activities als auch Design Studio eigene Stammordner sind, damit sie nach Namen abgerufen und verwendet werden können, um den Rest der Ordnerhierarchie in einer Zielinstanz zu durchlaufen.

### Durchsuchen

Ordner können auch [stapelweise abgerufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). Der Parameter &quot;root&quot;kann verwendet werden, um den übergeordneten Ordner anzugeben, unter dem die Abfrage ausgeführt wird, und ist als JSON-Objekt formatiert, das als Wert für den Abfrageparameter eingebettet ist. Der Stamm hat zwei Mitglieder:

1. id - Die ID des Ordners oder Programms.
1. type - Entweder Ordner oder Programm, je nach Typ des zu Browser gehörigen Stammordners.

Wenn der Stammordner nicht bekannt ist oder der Zweck darin besteht, alle Ordner in einem bestimmten Bereich abzurufen, kann der Stammordner als die Bereiche &quot;Marketing-Aktivitäten&quot;, &quot;Design Studio&quot;oder &quot;Lead-Datenbank&quot;angegeben werden. Die IDs für jede dieser IDs können über die [Ordner nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) API erstellen und den Namen des gewünschten Bereichs angeben.

Wie andere Massen-Asset-Abruf-Endpunkte sind Offset und maxReturn optionale Parameter für das Paging.   Andere optionale Parameter sind:

- workSpace - Der Name des Arbeitsbereichs, nach dem gefiltert werden soll.
- maxDepth - Die maximale Anzahl von Ebenen, die in der Ordnerhierarchie durchlaufen werden sollen. Wenn der Wert auf 0 gesetzt ist, wird nur der im Stammverzeichnis angegebene Ordner zurückgegeben. Wenn kein Wert angegeben wird, ist der Standardwert 2.

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

## Reaktionsstruktur

Ein Großteil der Ordnerantwort-Struktur ist selbsterklärend, aber es lohnt sich, einzelne Felder zu beachten. Die `folderId` und übergeordnete Felder sind JSON-Objekte, die die explizite Kennung und den Typ des Ordners selbst enthalten. Dieser Typ wird in Abfragen, Stamm- und übergeordneten Parametern von der API verwendet, um eine ordnungsgemäße Trennung zwischen Ordnern- und Programmtypen sicherzustellen. `folderType` spiegelt die Nutzung des Ordners wider, bei dem es sich möglicherweise um &quot;Marketing-Ordner&quot;, &quot;Programm&quot;, &quot;E-Mail&quot;, &quot;E-Mail-Vorlage&quot;, &quot;Landingpage-Vorlage&quot;, &quot;Snippet&quot;, &quot;Bild&quot;, &quot;Zone&quot;oder &quot;Datei&quot;handelt.  Die Typen Marketing-Ordner und Programm geben an, dass sie in Marketingaktivitäten vorhanden sind und mehrere Asset-Typen enthalten können. Die anderen Typen weisen darauf hin, dass sie ggf. nur diesen Typ von Asset, Unterordnern und die Vorlagenversion dieses Typs enthalten dürfen. Der Typ Zone stellt Ordner der Stammebene dar, die in Marketing-Aktivitäten zu finden sind.

Der Pfad eines Ordners zeigt seine Hierarchie in der Ordnerstruktur an, ähnlich wie bei einem Unix-Pfad. Der erste Eintrag im Pfad lautet immer Marketing Activities oder Design Studio. Wenn die Zielinstanz über Arbeitsbereiche verfügt, ist der zweite Eintrag im Pfad der Name des eigenen Arbeitsbereichs. Die `url` zeigt die explizite URL des Assets in der angegebenen Instanz an. Dies ist kein universeller Link und muss als Benutzer authentifiziert werden, damit er ordnungsgemäß funktioniert. `isSystem` gibt an, ob der Ordner ein Systemordner ist. Wenn dies auf &quot;true&quot;gesetzt ist, ist der Ordner selbst schreibgeschützt, obwohl Ordner als untergeordnete Elemente erstellt werden können.

## Erstellen und Aktualisieren

[Ordner erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) ist einfach und wird mit einer anwendungs-/x-www-form-urlencoded -POST ausgeführt, die zwei erforderliche Parameter aufweist: &quot;name&quot;, &quot;name&quot;, eine Zeichenfolge und &quot;parent&quot;, das übergeordnete Element, in dem der Ordner erstellt werden soll. Hierbei handelt es sich um ein eingebettetes JSON-Objekt mit zwei Mitgliedern, ID und Typ, je nach Zielordnertyp entweder &quot;folder&quot;oder &quot;program&quot;. Optional kann auch &quot;description&quot;, eine Zeichenfolge, einbezogen werden und bis zu 2000 Zeichen umfassen.

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

Aktualisierungen von Ordnern erfolgen über einen separaten Endpunkt und eine Beschreibung, einen Namen und `isArchive` sind optionale Parameter für die Aktualisierung. Wenn `isArchive` durch eine Aktualisierung geändert wird, führt dies dazu, dass der Ordner in der Benutzeroberfläche von Marketo archiviert wird, wenn er in &quot;true&quot;geändert wird, oder dass die Archivierung in &quot;false&quot;geändert wird. Programme können mit dieser API nicht aktualisiert werden.

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

Löschungen können für einzelne Ordner vorgenommen werden, wenn sie leer sind, d. h., sie enthalten keine Assets oder Unterordner. Wenn ein Ordner vom Typ Programm ist oder das Feld isSystem auf &quot;true&quot;gesetzt ist, kann er mit dieser API nicht gelöscht werden.

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
