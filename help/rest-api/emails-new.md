---
title: E-Mails
feature: REST API
description: Verwenden Sie die Marketo Asset REST-API, um Abhängigkeiten für E-Mail-Assets abzufragen, zu erstellen, zu aktualisieren, zu klonen, zu löschen, zu genehmigen und zu überprüfen.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 5%

---

# E-Mails

[E-Mail-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails_New)

E-Mails sind Asset-Datensätze, die Nachrichtenmetadaten, die Inhaltskonfiguration, die Einstellungen und den Genehmigungsstatus definieren.

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

Sie können E-Mail-Metadaten nach Asset-`id` oder mit dem Filter-Endpunkt abrufen.

### Nach ID

#### Anfrage

```http
GET /rest/asset/v2/email/{id}
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filter

Der Filterendpunkt unterstützt die Suche innerhalb eines Arbeitsbereichs und die Eingrenzung der Ergebnisse mit zusätzlichen Abfrageparametern.

`workspaceId` ist erforderlich.

Unterstützte Abfrageparameter:

| Parameter | Beschreibung |
| - | - |
| `workspaceId` | Erforderliche Arbeitsbereichskennung, die für die Reichweite der Filteranfrage verwendet wird. |
| `folderId` | Filtert Ergebnisse in einen einzelnen Ordner. |
| `folderIds` | Wiederholter Parameter, der Ergebnisse in mehrere Ordner filtert. |
| `status` | Parameter, der nach einem oder mehreren E-Mail-Status filtert. |
| `pageIndex` | Nullbasierter Seitenindex für paginierte Ergebnisse. |
| `pageSize` | Maximale Anzahl an zurückzugebenden Ergebnissen pro Seite |
| `createdBy` | Filtert die Ergebnisse nach der Erstellung des Benutzers. |
| `createdAtStart` | Gibt Assets zurück, die am oder nach diesem Zeitstempel erstellt wurden. |
| `createdAtEnd` | Gibt Assets zurück, die am oder vor diesem Zeitstempel erstellt wurden. |
| `modifiedBy` | Filtert Ergebnisse nach dem zuletzt bearbeitenden Benutzer. |
| `modifiedAtStart` | Gibt die am oder nach diesem Zeitstempel geänderten Assets zurück. |
| `modifiedAtEnd` | Gibt Assets zurück, die am oder vor diesem Zeitstempel geändert wurden. |
| `name` | Filtert Ergebnisse nach E-Mail-Namen. |
| `sortKey` | Wählt das Feld für die Sortierung der Ergebnisse aus. |
| `sortOrder` | Legt die Sortierrichtung fest. |
| `isCreatedByMe` | Beschränkt Ergebnisse auf Assets, die vom aktuellen Benutzer erstellt wurden. |
| `isModifiedByMe` | Beschränkt Ergebnisse auf Assets, die zuletzt vom aktuellen Benutzer geändert wurden. |
| `templateId` | Filtert Ergebnisse nach Vorlagenkennung. |
| `scriptEngine` | Filtert die Ergebnisse nach der Konfiguration des Skriptmoduls. |
| `isValueNonNullable` | Filtert danach, ob der Wert nicht null werden kann. |
| `includeArchived` | Schließt archivierte Assets in die Ergebnisse ein. |

#### Anfrage

```http
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Verwenden Sie den Parameter `name` , wenn Sie eine E-Mail nach Namen suchen müssen.

## Erstellen

Erstellen Sie eine E-Mail, indem Sie eine JSON-Payload senden. `name`, `appData` und `headers` sind erforderlich. `headers.subject` ist erforderlich und `appData` muss mindestens eines der folgenden Elemente enthalten: `folderId`, `workspaceId` oder `programId`.

### Anfrage

```http
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Der Anfragetext kann auch `data`, `editorContext`, `themeId`, `appType` und `status` enthalten.

### Erstellen von Feldern

* `appData` kennzeichnet, wo die E-Mail erstellt und wie sie bearbeitet wird.
* `headers` enthält die Werte des Nachrichtenheaders, einschließlich Betreff, Absender, Antwortadresse, Preheader und optionaler CC-Empfänger.
* `settings` steuert das Betriebs- und Renderverhalten, z. B. den Nur-Text-Modus, die Web-Seitenansicht und das URL-Tracking.
* `templateId` identifiziert die E-Mail-Vorlage, die bei der Erstellung der E-Mail verwendet wird.

## Update

Aktualisieren einer E-Mail nach Asset-ID. Der Anfragetext verwendet das `UpdateEmailRequest`, und alle Eigenschaften sind optional, sodass Sie nur die Felder senden können, die Sie ändern möchten.

### Anfrage

```http
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Antwort

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Status verwalten

E-Mails verwenden einen Entwurfs- und einen genehmigten Lebenszyklus. Verwenden Sie den Endpunkt „Statusübergang“, um eine E-Mail zu genehmigen, ihre Genehmigung aufzuheben, einen Entwurf zu verwerfen oder einen neuen Entwurf zu erstellen.

Gültige `action` sind:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Anfrage

```http
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Antwort

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Klon

Verwenden Sie den Klon-Endpunkt , um eine Kopie einer vorhandenen E-Mail zu erstellen.

### Anfrage

```http
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Löschen

Löschen einer E-Mail nach Asset-ID.

### Anfrage

```http
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Dieser Endpunkt nimmt das im Pfad `id` Asset und definiert keinen Anfragetext.

## Verwendet von

Verwenden Sie den `usedby`-Endpunkt zum Abrufen von Assets, die auf eine bestimmte E-Mail verweisen.

### Anfrage

```http
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Antwort

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Hinweise

Abfrage-Endpunkte geben Metadaten für das Asset zurück. Verwenden Sie die Endpunkt-Referenz für das vollständige Schema der verfügbaren Felder und alle umgebungsspezifischen Eigenschaften.
