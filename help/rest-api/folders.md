---
title: Ordner
feature: REST API
description: 'Marketo-REST-API-Handbuch für Ordner, in dem Folgendes behandelt wird: Erstellen, Aktualisieren, Löschen, Abfrage nach ID und Namen, Massendurchsuchen mit Stamm, Workspace, maxDepth und Paginierung.'
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
TQID: https://experienceleague.adobe.com/OxCNdy8qW6jwq8u57RF9mqVKPVvH99UmuiOBjFprHCM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 806
ht-degree: 1%

---

# Ordner

[Ordner-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders)

Ordner sind die zentralen organisatorischen Assets in Marketo. Jeder andere Asset-Typ verfügt über mindestens einen übergeordneten Asset-Typ, der entweder ein Ordner oder ein Programm ist. Ein Ordner ist rein organisatorisch, während ein Programm eine funktionale Beziehung zu anderen Asset-Typen hat und auch Assets enthalten kann.

Verwenden Sie die Ordner-API, um Ordner zu erstellen, abzufragen, zu aktualisieren und zu löschen oder ihren Inhalt abzurufen. Ordnerabfragen können Programme zurückgeben, Sie müssen jedoch die Programme-API verwenden, um ein Programm zu erstellen, zu aktualisieren oder zu löschen.

## Abfrage

Ordner unterstützen die standardmäßigen Asset-Abfragemuster: [nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) und durch [Browsen](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET).

### Nach ID

```http
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

Der `type` ist erforderlich und muss `Folder` oder `Program` sein. Sie bestimmt, ob der Endpunkt nach einer Ordner-ID oder einer Programm-ID sucht. Der Endpunkt gibt einen Datensatz im Ergebnis-Array zurück.

Die `folderType` gibt an, was der Ordner enthalten kann. Ordner für Marketing-Aktivitäten haben den Typ Marketing-Ordner oder -Programm und können mehrere Asset-Typen enthalten. Design Studio-Ordner haben einen Typ, der den Assets entspricht, die sie enthalten können. Ein E-Mail-Ordner kann beispielsweise E-Mails und Unterordner mit dem Ordnertyp E-Mail oder E-Mail-Vorlage enthalten.

Zu den Ordnertypen gehören:

- E-Mail
- E-Mail-Vorlage
- Landingpage
- Landingpage-Vorlage
- Ausschnitt
- Datei

### Nach Name

Der Endpunkt [Abfrage nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) erfordert `name`, der eine exakte Übereinstimmung mit Ordnernamen durchführt und jeden übereinstimmenden Ordner zurückgibt.

Der Endpunkt akzeptiert auch diese optionalen Parameter:

- `type`: Der Ordnertyp, entweder `Folder` oder `Program`.
- `root`: Die ID des zu durchsuchenden Ordners. Wenn Sie `root` festlegen, müssen Sie auch `type` festlegen.
- `workspace`: Der Name des zu durchsuchenden Arbeitsbereichs.

```http
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

Marketing-Aktivitäten und Design Studio sind Stammordner. Rufen Sie einen der Stammordner nach Namen ab, und verwenden Sie ihn dann, um die Ordnerhierarchie in der Zielinstanz zu durchlaufen.

### Durchsuchen

Sie können auch [Ordner stapelweise abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET). Verwenden Sie den `root`, um den übergeordneten Ordner anzugeben, unter dem eine Abfrage durchgeführt werden soll. Übergeben Sie `root` als eingebettetes JSON-Objekt mit zwei Elementen:

1. `id`: Die ID des Ordners oder Programms.
1. `type`: Entweder `Folder` oder `Program`, je nach Stammordnertyp.

Wenn Sie den Stammordner nicht kennen oder alle Ordner in einem Bereich abrufen möchten, verwenden Sie die Stammordner Marketing-Aktivitäten, Design Studio oder Lead-Datenbank. Rufen Sie die Stamm-ID ab, indem Sie den Bereichsnamen an die API [Ordner nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) übergeben.

Verwenden Sie wie bei anderen Endpunkten zum Massenabruf von Assets die optionalen `offset`- und `maxReturn` für die Paginierung. Weitere optionale Parameter sind:

- `workSpace`: Der Name des Arbeitsbereichs, nach dem gefiltert werden soll.
- `maxDepth`: Die maximale Anzahl von durchlaufenen Ebenen in der Ordnerhierarchie. Bei einem Wert von 0 wird nur der von `root` angegebene Ordner zurückgegeben. Der Standardwert lautet 2.

```http
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

Die Felder `folderId` und `parent` sind JSON-Objekte, die die Ordner-ID und den Typ enthalten. Die API verwendet diesen Typ in Abfrage-, `root`- und `parent`, um Ordner- und Programmordnertypen zu unterscheiden.

Das Feld `folderType` beschreibt, wie der Ordner verwendet wird. Der Wert kann „Marketing-Ordner“, „Programm“, „E-Mail“, „E-Mail-Vorlage“, „Landingpage“, „Landingpage-Vorlage“, „Ausschnitt“, „Bild“, „Zone“ oder „Datei“ sein. Marketing-Ordner und -Programm sind in Marketing-Aktivitäten vorhanden und können mehrere Asset-Typen enthalten. Die anderen Ordnertypen enthalten nur den entsprechenden Asset-Typ, Unterordner und gegebenenfalls die Vorlagenversion dieses Asset-Typs. Zone stellt einen Stammordner in Marketing-Aktivitäten dar.

Der `path` zeigt seine Hierarchie als Pfad im Unix-Stil an. Der erste Eintrag ist immer Marketing-Aktivitäten oder Design-Studio. Wenn die Instanz über Arbeitsbereiche verfügt, ist der zweite Eintrag der besitzende Arbeitsbereichsname.

Das Feld `url` enthält die Asset-URL für die angegebene Instanz. Dies ist kein universeller Link und erfordert eine Benutzerauthentifizierung. Das Feld `isSystem` gibt an, ob es sich bei dem Ordner um einen schreibgeschützten Systemordner handelt. Sie können untergeordnete Ordner unter einem Systemordner erstellen.

## Erstellen und aktualisieren

Um [Ordner zu erstellen](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/createFolderUsingPOST) senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage mit den folgenden Parametern:

- `name`: Erforderlicher String mit dem Ordnernamen.
- `parent`: Erforderliches eingebettetes JSON-Objekt, das `id` und `type` enthält. Der Typ ist `Folder` oder `Program`, je nach übergeordnetem Element.
- `description`: Optionale Zeichenfolge mit bis zu 2.000 Zeichen.

```http
POST /rest/asset/v1/folders.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Verwenden Sie den update-Endpunkt, um die optionalen `description`-, `name`- oder `isArchive` zu ändern. Wenn Sie `isArchive` auf `true` setzen, wird der Ordner in der Marketo-Benutzeroberfläche archiviert. Wenn Sie `false` festlegen, wird der Ordner aus dem Archiv entfernt.

Mit dieser API können Sie keine Programme aktualisieren.

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
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

Sie können einen einzelnen Ordner nur löschen, wenn er keine Assets oder Unterordner enthält. Sie können diese API nicht verwenden, um ein Programm oder einen Ordner zu löschen, dessen `isSystem` Feld `true` ist.

```http
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
