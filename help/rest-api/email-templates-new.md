---
title: E-Mail-Vorlagen
feature: REST API
description: Verwenden Sie die Marketo Asset REST-API, um Abhängigkeiten für E-Mail-Vorlagen abzufragen, zu erstellen, zu aktualisieren, zu klonen, zu löschen, zu genehmigen und zu überprüfen.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: 0e0a3e5a08e81f349044cbc327d1aba963ab30e4
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# E-Mail-Vorlagen

[Endpunktreferenz für E-Mail-Vorlage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates)

E-Mail-Vorlagen definieren die Struktur und das wiederverwendbare Layout, die beim Erstellen von E-Mails verwendet werden.

## Zugriff

Die in diesem Artikel beschriebenen Endpunkte erfordern ein Zugriffs-Token:

```text
?access_token=<access_token>
```

Für Anfragen ist außerdem die `x-app-type` Kopfzeile erforderlich:

```text
x-app-type: <app-type>
```

## Abfrage

Sie können Metadaten von E-Mail-Vorlagen nach Asset-ID oder mit dem Filter-Endpunkt abrufen.

### Nach ID

#### Anfrage

```text
GET /rest/asset/v2/emailtemplate/{id}
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filter

Der Filterendpunkt unterstützt die Suche innerhalb eines Arbeitsbereichs und die Eingrenzung der Ergebnisse mit zusätzlichen Abfrageparametern. `workspaceId` ist erforderlich.

Zu den unterstützten Filtern gehören `folderId`, wiederholte `folderIds`, wiederholte `status`, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`,, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` und `includeArchived`.

#### Anfrage

```text
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Verwenden Sie den `name`, wenn Sie eine Vorlage anhand des Namens finden müssen.

## Erstellen

Erstellen Sie eine E-Mail-Vorlage, indem Sie eine JSON-Payload senden. `name` und `appData` sind erforderlich. `appData` muss mindestens `folderId` oder `workspaceId` enthalten.

### Anfrage

```text
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Der Anfragetext kann auch `data`, `editorContext`, `appType` und `status` enthalten.

### Erstellen von Feldern

`appData` gibt an, wo die Vorlage gespeichert und wie sie bearbeitet wird.

`themeId` können ggf. verwendet werden, um der Vorlage ein Design zuzuordnen.

## Update

Aktualisieren einer Vorlage anhand der Asset-ID.

### Anfrage

```text
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Status verwalten

E-Mail-Vorlagen verwenden einen Entwurfs- und einen genehmigten Lebenszyklus. Verwenden Sie den Endpunkt „Statusübergang“, um eine Vorlage zu genehmigen, ihre Genehmigung aufzuheben, einen Entwurf zu verwerfen oder einen neuen Entwurf zu erstellen.

Gültige `action` sind:

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Anfrage

```text
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Antwort

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Klon

Verwenden Sie den Klon-Endpunkt, um eine Kopie einer vorhandenen Vorlage zu erstellen.

### Anfrage

```text
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Löschen

Löschen einer Vorlage anhand der Asset-ID.

### Anfrage

```text
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Dieser Endpunkt übernimmt die Vorlagen-ID im Pfad und definiert keinen Anfragetext.

## Verwendet von

Verwenden Sie den `usedby`-Endpunkt zum Abrufen von Assets, die auf eine bestimmte Vorlage verweisen.

### Anfrage

```text
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Hinweise

Abfrage-Endpunkte geben Metadaten für das Asset zurück. Verwenden Sie die Endpunkt-Referenz für das vollständige Antwortschema und alle umgebungsspezifischen Eigenschaften.
