---
title: Landingpage-Vorlagen
feature: REST API, Landing Pages
description: Verwalten Sie Landingpage-Vorlagen von Marketo über REST-API-Endpunkte für Freiform und geführte Typen, fragen Sie nach ID oder Namen ab, erstellen, aktualisieren Sie HTML, klonen Sie Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---

# Landingpage-Vorlagen

[Endpunkt-Referenz zur Landingpage-Vorlage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Landingpage-Vorlagen sind eine übergeordnete Ressource und eine Abhängigkeit für einzelne Marketo-Landingpages. Landingpages leiten das Skelett ihrer Inhalte von der übergeordneten Vorlage ab.

## Vorlagentypen

Marketo verfügt über zwei Arten von Landingpage-Vorlagen: Freiform und Geführt. Freiform-Landingpage-Vorlagen bieten ein locker strukturiertes Bearbeitungserlebnis für die von ihnen abgeleiteten Seiten. Geführte Vorlagen bieten ein stark strukturiertes Erlebnis, in dem Elementtypen und Speicherorte auf Vorlagenebene eingeschränkt werden können. Weitere Informationen zu den Unterschieden finden Sie [diesem Dokument](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Abfrage

Landingpage-Vorlagen unterstützen die standardmäßigen Abfragetypen für Assets [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) und [Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Diese Endpunkte geben Metadaten für die Vorlagen zurück. Das Abrufen des HTML-Inhalts von Vorlagen muss für jede Vorlage über ihre ID erfolgen.

## Erstellen und aktualisieren

Vorlagen werden als leere Assets mit zugehörigen Metadaten erstellt. Beim Erstellen einer Vorlage müssen ein Name und ein Ordner zusammen mit einer optionalen Beschreibung, einem templateType-Parameter und einem enableMunchkin-Parameter enthalten sein. templateType kann entweder Freiform oder Geführt sein und standardmäßig auf freeForm eingestellt sein. Unterschiede zwischen den Typen finden Sie im Abschnitt Geführte vs. Freiform . enableMunchkin ist standardmäßig auf „false“ gesetzt. Wenn diese Option aktiviert ist, wird das Munchkin-Tracking für alle untergeordneten Landingpages der Vorlage verhindert.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Der Inhalt der Vorlage muss separat über den Endpunkt [Inhalt der Landingpage-Vorlage aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) ausgefüllt werden.

### Aktualisieren von Metadaten

Metadaten für Landingpage-Vorlagen können über den Endpunkt [Aktualisieren von Landingpage-Vorlagen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) aktualisiert werden. Name, Beschreibung und die Einstellung enableMunchkin können auf diese Weise aktualisiert werden.

### Inhalt aktualisieren

Inhalte in Landingpage-Vorlagen werden als destruktive Aktualisierung des gesamten HTML-Inhalts erstellt. Der Inhalt muss als multipart/form-data übergeben werden, wobei der einzige Parameter „content“ heißt.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
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

```
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

## Klon

Marketo bietet eine einfache Methode zum Klonen von Landingpage-Vorlagen. Dies ist eine application/x-www-url-formecodierte POST-Anfrage.

Der Parameter `id` gibt die ID der zu klonenden Quell-Landingpage-Vorlage an.

Mit dem Parameter `name` wird der Name der neuen Landingpage-Vorlage angegeben.

Mit dem Parameter `folder` wird der übergeordnete Ordner angegeben, in dem sich die neue Landingpage-Vorlage befindet. Dies ist in Form eines eingebetteten JSON-Objekts mit  `id` und `type`.

Der optionale Parameter `description` wird verwendet, um die neue Landingpage-Vorlage zu beschreiben.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Landingpage-Vorlagen folgen dem standardmäßigen Entwurfsbestätigungsmodell, für das es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf eine Vorlage angewendet werden, werden sie immer zuerst auf die Entwurfsversion angewendet und erst dann live angezeigt, wenn die Vorlage genehmigt wurde.

Damit eine Vorlage genehmigt werden kann, muss sie den Regeln für ihren Typ entsprechen, entweder geführt von Freiform. Weitere Informationen zu den Anforderungen für das Erstellen und Genehmigen von Vorlagen der jeweiligen Typen finden Sie in den jeweiligen Erstellungsdokumenten:

- [Freiform-Landingpage-Vorlagen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Geführte Landingpage-Vorlagen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Beispiele für geführte Vorlagen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Löschen

Um eine Vorlage zu löschen, muss sie unbrauchbar und nicht genehmigt sein. Das bedeutet, dass keine untergeordnete Landingpage darauf verweisen darf.  Landingpage-Vorlagen mit eingebetteten Social-Media-Schaltflächen können mit dieser API nicht gelöscht werden.
