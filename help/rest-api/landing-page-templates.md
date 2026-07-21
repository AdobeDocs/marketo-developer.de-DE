---
title: Landingpage-Vorlagen
feature: REST API, Landing Pages
description: Verwalten Sie Landingpage-Vorlagen von Marketo über REST-API-Endpunkte für Freiform und geführte Typen, fragen Sie nach ID oder Namen ab, erstellen, aktualisieren Sie HTML, klonen Sie Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
TQID: https://experienceleague.adobe.com/U9K1MG-q2gIgJMgfM3lt1S4olETt8ln9seOIKZUncBY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 519
ht-degree: 2%

---

# Landingpage-Vorlagen

[Endpunkt-Referenz für Landingpage-Vorlage](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates)

Landingpage-Vorlagen sind übergeordnete Ressourcen für Marketo-Landingpages. Jede Landingpage leitet ihre anfängliche Inhaltsstruktur von ihrer übergeordneten Vorlage ab.

## Vorlagentypen

Marketo bietet Freiform- und geführte Landingpage-Vorlagen. Freiformvorlagen bieten ein locker strukturiertes Bearbeitungserlebnis. Geführte Vorlagen können Elementtypen und Speicherorte auf Vorlagenebene einschränken.

Einen detaillierten Vergleich finden Sie [Grundlegendes zu Freiform und geführten Landingpages](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Abfrage

Abfragen von Landingpage[Vorlagen (nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) oder nach [Browsen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Diese Endpunkte geben Vorlagenmetadaten zurück. Rufen Sie für jede Vorlage nach ID separaten HTML-Inhalt ab.

## Erstellen und aktualisieren

Vorlagen werden als leere Assets mit Metadaten erstellt. Die Parameter `name` und `folder` sind erforderlich. Die Parameter `description`, `templateType` und `enableMunchkin` sind optional.

Der `templateType` kann `freeform` oder `guided` sein und ist standardmäßig auf `freeForm` eingestellt. Der `enableMunchkin` ist standardmäßig auf `false` gesetzt. Wenn diese Option aktiviert ist, wird das Munchkin-Tracking auf den untergeordneten Landingpages der Vorlage verhindert.

```http
POST /rest/asset/v1/landingPageTemplates.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Fügen Sie Vorlageninhalte separat mit dem Endpunkt [Inhalt der Landingpage aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) hinzu.

### Aktualisieren von Metadaten

Verwenden Sie den Endpunkt [Aktualisieren von Landingpage-Vorlagen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST), um den Namen, die Beschreibung oder die `enableMunchkin` zu ändern.

### Inhalt aktualisieren

Durch die Aktualisierung des Vorlageninhalts werden alle vorhandenen HTML-Inhalte ersetzt. Übergeben Sie die Ersetzung als `multipart/form-data` im `content`.

```http
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```html
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Klonen

Klonen Sie eine Landingpage-Vorlage mit einer `application/x-www-url-formencoded` POST-Anfrage.

Der Parameter `id` gibt die Vorlage für die Quell-Landingpage an.

Der Parameter `name` gibt den Namen der neuen Landingpage-Vorlage an.

Der Parameter `folder` gibt den übergeordneten Ordner für die neue Vorlage an. Übergeben Sie sie als eingebettetes JSON-Objekt, das `id` und `type` enthält.

Der optionale `description`-Parameter beschreibt die neue Vorlage.

```http
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Genehmigung

Landingpage-Vorlagen verwenden das Standardmodell Entwurf und Genehmigt . Aktualisierungen werden zuerst auf den Entwurf angewendet und erst nach der Genehmigung der Vorlage live geschaltet.

Vor der Genehmigung muss eine Vorlage die Anforderungen für ihren geführten oder Freiformtyp erfüllen. Diese Ressourcen anzeigen:

- [Freiform-Landingpage-Vorlagen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Geführte Landingpage-Vorlagen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Beispiele für geführte Vorlagen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Löschen

Um eine Vorlage zu löschen, stellen Sie sicher, dass sie nicht genehmigt ist und keine untergeordnete Landingpage darauf verweist. Sie können diese API nicht verwenden, um Landingpage-Vorlagen mit eingebetteten Social-Media-Schaltflächen zu löschen.
