---
title: Dynamische Inhalte
feature: REST API, Dynamic Content
description: Konfigurieren Sie dynamische Marketo-Inhalte auf Abschnittsebene über REST-APIs mithilfe von Segmentierungen, um E-Mails, Landingpages und Snippets mit Endpunkten und Beispielen zu personalisieren
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
TQID: https://experienceleague.adobe.com/MwfPxu74qk0bPZMr6yuxQi--e3gMvP1tXQZ5iMil02o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 329
ht-degree: 3%

---

# Dynamische Inhalte

Verwenden Sie Lead-Segmentierungen, um dynamische Inhalte in diesen Asset-Typen bereitzustellen:

- E-Mails
- Landingpages
- Ausschnitte

## Überblick

Dynamische Inhalte werden auf Abschnittsebene ausgeführt. Jeder Abschnitt kann Varianten für Segmente in einer ausgewählten Segmentierung bereitstellen.

Wenn ein Lead das Asset anzeigt, zeigt Marketo die Variante für das Segment des Leads an. Wenn sich der Lead nicht für ein Segment qualifiziert, zeigt Marketo den Standardinhalt an.

## Beispiel

In diesem Beispiel wird eine Segmentierung der Region (US) verwendet, um eine Ereignisweiterleitung für Leads im Südwestsegment anzuzeigen. Das Segment umfasst Leads aus Kalifornien, Nevada, Utah, Colorado, Arizona und New Mexico.

Verwenden Sie den Endpunkt [Abschnitt „E-Mail-](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST) aktualisieren“, um den bearbeitbaren Abschnitt mit der ID `Q1-promotion-banner` in einen `DynamicContent` Abschnitt zu ändern. Der `value` gibt die Segmentierungs-ID an.

E-Mails und Landingpages folgen diesem Muster. Snippets verwenden das andere Muster, das in der Snippets-API-Dokumentation beschrieben wird.

Im folgenden Beispiel wird der Abschnitt als Abschnitt mit dynamischem Inhalt festgelegt, der nach Segmentierung 1001 segmentiert ist.

```http
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```text
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Rufen Sie den Endpunkt [Abschnitt zum Aktualisieren von E-Mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailDynamicContentUsingPOST)Dynamischem Inhalt auf, um Inhalte für ein Segment in einem bestimmten Abschnitt hinzuzufügen.

Die folgende Anfrage zeigt ein spezielles Banner anstelle des Standardinhalts für Leads im Südwestsegment an. Um weitere Varianten zu erstellen, rufen Sie den Endpunkt für jedes Segment und jeden Abschnitt auf.

```http
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```text
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentierung

Eine Segmentierung ist eine benutzerdefinierte Liste von Regelsätzen, die von Marketo von oben nach unten mit der Lead-Datenbank ausgewertet werden. Ein Lead kann in jeder Segmentierung nur zu einem Segment gehören. Der Lead tritt dem ersten Segment bei, für das er qualifiziert ist.

Wenn der Lead sich nicht für ein anderes Segment qualifiziert, tritt er in das Standardsegment ein und erhält den Standardinhalt der Segmentierung.

### Liste

Verwenden Sie den Listenendpunkt, um verfügbare Segmentierungen abzurufen.

```http
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

Verwenden Sie den Segmentendpunkt , um die Segmente in einer übergeordneten Segmentierung abzurufen.

```http
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
