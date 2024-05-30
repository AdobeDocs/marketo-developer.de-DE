---
title: "Dynamischer Inhalt"
feature: REST API, Dynamic Content
description: "Konfigurieren dynamischer Inhalte mit Marketo-APIs."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 2%

---


# Dynamischer Inhalt

Marketo erleichtert die Verwendung dynamischer Inhalte durch Lead-Segmentierung für mehrere Asset-Typen:

- E-Mails
- Landing Page
- Ausschnitte

## Überblick

Dynamische Inhalte werden auf Abschnittsebene implementiert, indem bestimmte Varianten eines Abschnitts für einen Lead basierend auf seiner Qualifizierung in einem Segment innerhalb einer ausgewählten Segmentierung benannt werden. Wenn ein Inhaltselement so konfiguriert ist, dass es dynamischen Inhalt basierend auf einer bestimmten Segmentierung bereitstellt, wird einem Lead, der diesen Inhalt anzeigt, die Inhaltsvariante bereitgestellt, die mit dem Segment übereinstimmt, in das er fällt, oder der Standardinhalt, wenn er nicht für ein Segment qualifiziert ist.

## Beispiel

Sehen wir uns dazu ein E-Mail-Beispiel an, in dem wir eine Region-Segmentierung (US) haben und eine Ereignispromotion nur für Leads anzeigen möchten, die in das Südwestsegment fallen, zu dem auch Kalifornien, Nevada, Utah, Colorado, Arizona und New Mexico gehören. Dazu erstellen wir einen bearbeitbaren Abschnitt in unserer E-Mail mit der ID &quot;Q1-Promotion-Banner&quot;in einen Abschnitt &quot;Dynamischer Inhalt&quot;. Dazu müssen wir die [Abschnitt &quot;E-Mail-Inhalt aktualisieren&quot;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) Endpunkt für unsere E-Mail. Die `value` wird verwendet, um die ID der Segmentierung anzugeben.

Hinweis: E-Mails und Landingpages folgen diesem Muster. Snippets weisen ein anderes Muster auf, das in der Dokumentation zur Snippets-API beschrieben wird.

Im folgenden Beispiel wird der Abschnitt als Abschnitt mit dynamischem Inhalt festgelegt, der durch die Segmentierung 1001 segmentiert wird.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
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

Um Inhalte für einzelne Segmente hinzuzufügen, müssen wir die [Abschnitt &quot;Dynamischen E-Mail-Inhalt aktualisieren&quot;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) -Endpunkt für den spezifischen Abschnitt.

Im folgenden Beispiel wird der -Abschnitt so eingestellt, dass unser spezielles Bannerbild für Leads im Südwesten-Segment anstelle des standardmäßigen angezeigt wird. Wenn wir mehr Varianten für mehr Segmente erstellen möchten, rufen wir diesen Endpunkt erneut für jedes Segment und jeden Abschnitt auf.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
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

Die Segmentierung ist der Kern der dynamischen Marketo-Inhalte. Eine Segmentierung ist eine benutzerdefinierte Liste einzelner Regelsätze, die von oben nach unten anhand der gesamten Lead-Datenbank bewertet werden. Ein Lead kann nur Mitglied eines Segments in jeder Segmentierung sein und gehört dem ersten Segment an, für das er in jeder Segmentierung qualifiziert ist. Wenn es sich nicht für ein Segment qualifiziert, wird es Mitglied des Standardsegments und erhält den Standardinhalt für jeden bestimmten dynamischen Inhalt, der diese Segmentierung verwendet.

### Liste

Segmente verfügen über einen Listenendpunkt, der eine Antwort mit einer Liste der verfügbaren Segmentierungen zurückgibt.

```
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

Segmente verfügen auch über einen Endpunkt, der eine Antwort mit einer Liste von Segmenten aus einer übergeordneten Segmentierung zurückgibt.

```
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
