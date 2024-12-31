---
title: Dynamischer Inhalt
feature: REST API, Dynamic Content
description: Konfigurieren von dynamischen Inhalten mit Marketo-APIs.
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 2%

---

# Dynamischer Inhalt

Marketo erleichtert die Verwendung dynamischer Inhalte durch Lead-Segmentierung für mehrere Asset-Typen:

- E-Mails
- Landing Page
- Snippets

## Überblick

Dynamische Inhalte werden auf Abschnittsebene implementiert, indem bestimmte Varianten eines Abschnitts, der einem Lead bereitgestellt werden soll, basierend auf ihrer Qualifizierung in einem Segment innerhalb einer ausgewählten Segmentierung bestimmt werden. Wenn ein Inhaltselement so konfiguriert ist, dass es dynamische Inhalte basierend auf einer bestimmten Segmentierung bereitstellt, wird einem Lead, der diesen Inhalt sieht, die Inhaltsvariante bereitgestellt, die dem Segment entspricht, dem sie angehören, oder der Standardinhalt, wenn sie nicht für ein Segment qualifiziert sind.

## Beispiel

Zur Veranschaulichung sehen wir uns ein E-Mail-Beispiel an, in dem wir eine Segmentierung nach Region (USA) haben und eine Ereignisaktion nur für Leads anzeigen möchten, die zum Segment „Südwest“ gehören, das Kalifornien, Nevada, Utah, Colorado, Arizona und New Mexico umfasst. Dazu wird ein bearbeitbarer Abschnitt in unserer E-Mail mit der ID „Q1-Promotion-Banner“ in einen DynamicContent-Abschnitt umgewandelt. Dazu müssen wir den Endpunkt [Abschnitt E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) für unsere E-Mail verwenden. Mit dem Parameter `value` wird die ID der Segmentierung angegeben.

Hinweis: Sowohl E-Mails als auch Landingpages folgen diesem Muster. Snippets weisen ein anderes Muster auf, das in der Dokumentation zur Snippets-API beschrieben wird.

Im folgenden Beispiel wird der Abschnitt als Abschnitt mit dynamischem Inhalt festgelegt, der nach Segmentierung 1001 segmentiert ist.

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

Um Inhalte für einzelne Segmente hinzuzufügen, müssen wir den Endpunkt [Abschnitt zum Aktualisieren dynamischer E-Mail-Inhalte](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) für den jeweiligen Abschnitt aufrufen.

Im folgenden Beispiel wird der Abschnitt so eingestellt, dass unser spezielles Bannerbild für Leads im Segment Südwest anstelle des Standardbilds angezeigt wird. Wenn wir mehr Varianten für weitere Segmente erstellen möchten, rufen wir diesen Endpunkt für jedes Segment und jeden Abschnitt erneut auf.

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

Die Segmentierung ist der Kern von Marketo Dynamic Content. Eine Segmentierung ist eine benutzerdefinierte Liste einzelner Regelsätze, die von oben nach unten mit der gesamten Lead-Datenbank ausgewertet werden. Ein Lead darf in jeder Segmentierung nur Mitglied eines Segments sein und ist Mitglied des ersten Segments, für das er in jeder Segmentierung qualifiziert ist. Wenn es sich nicht für ein Segment qualifiziert, ist es Mitglied des Standardsegments und erhält den Standardinhalt für jedes beliebige dynamische Inhaltselement, das diese Segmentierung verwendet.

### Liste

Segmentierungen haben einen Listenendpunkt, der eine Antwort mit einer Liste der verfügbaren Segmentierungen zurückgibt.

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

Segmentierungen haben auch einen Endpunkt, der eine Antwort mit einer Liste von Segmenten aus einer übergeordneten Segmentierung zurückgibt.

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
