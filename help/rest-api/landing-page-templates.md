---
title: "Landingpage-Vorlagen"
feature: REST API, Landing Pages
description: "Erstellen und bearbeiten Sie Landingpage-Vorlagen."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 1%

---


# Landing Page-Vorlagen

[Endpoint-Referenz zu Landingpage-Vorlagen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Landingpage-Vorlagen sind eine übergeordnete Ressource und Abhängigkeit für einzelne Marketo-Landingpages. Landingpages leiten das Skelett ihres Inhalts von der übergeordneten Vorlage ab.

## Vorlagentypen

Marketo verfügt über zwei Arten von Einstiegsseitenvorlagen: Freiform und Führung. Landingpage-Vorlagen für Formulare bieten ein locker strukturiertes Bearbeitungserlebnis für daraus abgeleitete Seiten. Geführte Vorlagen bieten ein stark strukturiertes Erlebnis, bei dem Elementtypen und -speicherorte auf Vorlagenebene eingeschränkt werden können. Weitere Informationen zu den Unterschieden finden Sie unter [dieses Dokuments](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Anfrage

Landingpage-Vorlagen unterstützen die standardmäßigen Abfragetypen für Assets von [by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET), und [Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Diese Endpunkte geben Metadaten für die Vorlagen zurück. Der HTML-Inhalt von Vorlagen muss pro Vorlage über die zugehörige ID abgerufen werden.

## Erstellen und Aktualisieren

Vorlagen werden als leere Assets mit zugehörigen Metadaten erstellt. Beim Erstellen einer Vorlage müssen ein Name und ein Ordner zusammen mit einer optionalen Beschreibung, dem Parameter templateType und enableMunchkin angegeben werden. templateType kann entweder Freiform oder Guided sein und standardmäßig freeForm verwenden. Informationen zu Unterschieden zwischen den Typen finden Sie im Abschnitt Geführtes vs. Freies Formular . enableMunchkin ist standardmäßig auf &quot;false&quot;gesetzt. Wenn diese Option aktiviert ist, wird verhindert, dass auf untergeordneten Landingpages der Vorlage das Munchkin-Tracking ausgeführt wird.

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

Der Inhalt der Vorlage muss separat über die [Inhalt der Landingpage-Vorlage aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) -Endpunkt.

### Aktualisieren von Metadaten

Metadaten für Landingpage-Vorlagen können über die [Aktualisieren der Metadaten der Landingpage-Vorlage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) -Endpunkt. Name, Beschreibung und die Einstellung enableMunchkin können auf diese Weise aktualisiert werden.

### Inhalt aktualisieren

Der Inhalt in den Landingpage-Vorlagen wird zerstörerisch auf den gesamten HTML-Inhalt aktualisiert. Der Inhalt muss als mehrteilige Formulardaten übergeben werden, wobei der einzige Parameter &quot;content&quot;ist.

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

## Klonen

Marketo bietet eine einfache Methode zum Klonen von Einstiegsseitenvorlagen. Dies ist eine Anfrage zur POST der Anwendung/x-www-url-formencoded .

Die `id` path parameter gibt die ID der zu klonenden Quelleinstiegsseitenvorlage an.

Die `name` wird verwendet, um den Namen der neuen Landingpage-Vorlage anzugeben.

Die `folder` wird verwendet, um den übergeordneten Ordner anzugeben, in dem sich die neue Landingpage-Vorlage befindet. Dies geschieht in Form eines eingebetteten JSON-Objekts, das  `id` und `type`.

Das optionale `description` wird verwendet, um die neue Landingpage-Vorlage zu beschreiben.

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

Landingpage-Vorlagen folgen dem standardmäßigen Entwurf-genehmigten Modell, bei dem es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf eine Vorlage angewendet werden, werden sie immer zuerst auf den Entwurf der Version angewendet und erst dann live angezeigt, wenn die Vorlage genehmigt wurde.

Damit eine Vorlage genehmigt werden kann, muss sie den Regeln für ihren Typ entsprechen, die entweder frei formuliert sind. Weitere Informationen zu den Anforderungen für die Erstellung und Validierung von Vorlagen der jeweiligen Typen finden Sie in den entsprechenden Erstellungsdokumenten:

- [Landingpage-Vorlagen für kostenlose Formulare](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Geführte Landingpage-Vorlagen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Beispiele für geführte Vorlagen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Löschen

Um eine Vorlage zu löschen, muss sie nicht verwendet und nicht genehmigt sein, d. h., keine untergeordnete Landingpage darf auf sie verweisen.  Landingpage-Vorlagen mit eingebetteten Social-Schaltflächen können mit dieser API möglicherweise nicht gelöscht werden.
