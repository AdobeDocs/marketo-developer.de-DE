---
title: Fragmente
feature: REST API
description: Verwenden Sie die Marketo Asset REST-API, um Abhängigkeiten für Fragmente abzufragen, zu erstellen, zu aktualisieren, zu klonen, zu löschen, zu genehmigen und zu überprüfen.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 9%

---

# Fragmente

Fragmente sind wiederverwendbare Inhalts-Assets, die von anderen Assets, z. B. E-Mails, referenziert werden können.

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

Sie können Fragmentmetadaten nach Asset-ID oder mit dem Filter-Endpunkt abrufen.

### Nach ID

#### Anfrage

```http
GET /rest/asset/v2/fragment/{id}
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filter

Der Filterendpunkt sucht in einem Arbeitsbereich. Der `workspaceId` ist erforderlich.

Verwenden Sie diese optionalen Parameter, um die Ergebnisse einzugrenzen oder zu organisieren:

* Ordner: `folderId` oder wiederholte `folderIds`
* Status: wiederholte `status`
* Paginierung: `pageIndex` und `pageSize`
* Erstellung: `createdBy`, `createdAtStart`, `createdAtEnd` und `isCreatedByMe`
* Änderung: `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd` und `isModifiedByMe`
* Fragmenteigenschaften: `name`, `fragmentType`, `scriptEngine` und `isValueNonNullable`
* Sortierung: `sortKey` und `sortOrder`
* Archivierter Inhalt: `includeArchived`

#### Anfrage

```http
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Verwenden Sie den `name`, wenn Sie ein Fragment anhand des Namens suchen müssen.

## Erstellen

Erstellen Sie ein Fragment durch Senden einer JSON-Payload. `name`, `appData` und `settings` sind erforderlich. `settings` müssen `fragmentType` und `supportedChannels` enthalten.

### Anfrage

```http
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

Der Anfragetext kann auch `data`, `editorContext`, `themeId`, `appType` und `status` enthalten.

### Erstellen von Feldern

* `appData` gibt an, wo das Fragment gespeichert wird und wie es bearbeitet wird.
* `settings.fragmentType` identifiziert die Fragmentkategorie, z. B. ein E-Mail-Fragment.
* `settings.fragmentSubType` kann verwendet werden, um das Fragmentformat weiter zu definieren.
* `settings.supportedChannels` werden die Kanäle aufgelistet, in denen das Fragment verwendet werden kann.

## Update

Aktualisieren eines Fragments nach Asset-ID.

### Anfrage

```http
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Status verwalten

Fragmente verwenden einen Entwurfs- und einen genehmigten Lebenszyklus. Verwenden Sie den Endpunkt „Statusübergang“, um ein Fragment zu genehmigen, seine Genehmigung aufzuheben, einen Entwurf zu verwerfen oder einen neuen Entwurf zu erstellen.

Gültige `action` sind:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Anfrage

```http
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Antwort

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Klon

Verwenden Sie den Klon-Endpunkt , um eine Kopie eines vorhandenen Fragments zu erstellen.

### Anfrage

```http
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Löschen

Löschen eines Fragments nach Asset-ID.

### Anfrage

```http
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Dieser Endpunkt übernimmt die Fragment-ID im Pfad und definiert keinen Anfragetext.

## Verwendet von

Verwenden Sie den `usedby`-Endpunkt zum Abrufen von Assets, die auf ein bestimmtes Fragment verweisen.

### Anfrage

```http
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
