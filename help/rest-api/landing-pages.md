---
title: Landingpages
feature: REST API, Landing Pages
description: Abfragen von Landingpages in Marketo.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 2%

---

# Landingpages

[Endpunkt-Referenz für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Landingpages sind von Marketo gehostete Web-Seiten.

## Abfrage

Wie die meisten anderen Assets können Landingpages [nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET) &quot;[nach ID“ ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) &quot;[&quot; ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET) werden. Diese Abfragen geben nur Metadaten zurück. Die Liste der Inhaltsabschnitte für eine Landingpage muss separat nach der ID der Landingpage abgefragt werden.

Wenn Sie den Inhalt der Landingpage abfragen, wird eine Liste der Inhaltsabschnitte zurückgegeben, die in der Landingpage verfügbar sind. Ein Abschnitt muss in der Inhaltsliste einer Seite vorhanden sein, um den Inhalt zu aktualisieren:

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

Die Ergebnisse unterscheiden sich zwischen geführten und Freiformvorlagen, da die geführten Landingpages eine Reihe von Abschnitten enthalten, die durch die Vorlage definiert werden, von der sie abgeleitet werden, während die Freiformseiten keine vordefinierten Abschnitte enthalten und ihr Inhalt vor der Bearbeitung hinzugefügt werden muss.  Beachten Sie, dass das Format des Attributs „content“ je nach dem Attribut „type“ und abhängig davon, ob das Feld statisch oder dynamisch ist, variieren kann.

## Erstellen und aktualisieren

[Landingpages werden erstellt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) indem auf eine Vorlage verwiesen wird. Die einzigen erforderlichen Felder für die Erstellung sind Name, Vorlage (die ID der Vorlage) und der Ordner, in dem die Seite platziert werden soll. Weitere Metadaten, die ausgefüllt werden können, finden Sie in der Endpunkt-Referenz.

Gültige Inhaltstypen für Endpunkte [Inhalt der Landingpage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) sind: richText, HTML, Form, Image, Rectangle, Snippet.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

Die Metadaten der Landingpage können mit dem Endpunkt [Metadaten der Landingpage aktualisieren“ aktualisiert ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Genehmigung

Landingpages folgen dem standardmäßigen Entwurfsvalidierungsmodell, für das es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf eine Seite angewendet werden, werden sie immer zuerst auf die Entwurfsversion angewendet und erst live angezeigt, wenn die Seite genehmigt wurde.

## Löschen

Um eine Landingpage zu löschen, muss sie zunächst nicht mehr verwendet und nicht von anderen Marketo-Assets referenziert werden und muss außerdem nicht genehmigt sein. Seiten werden einzeln mit dem Endpunkt [Landingpage löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST) gelöscht. Landingpages mit eingebetteten Social-Media-Schaltflächen können über diese API nicht gelöscht werden.

## Klonen

Marketo bietet eine einfache Methode zum Klonen einer Landingpage. Dies ist eine application/x-www-url-formecodierte POST-Anfrage.

Der `id`-Pfadparameter gibt die ID der zu klonenden Quell-Landingpage an.

Mit dem Parameter `name` wird der Name der neuen Landingpage angegeben.

Mit dem Parameter `folder` wird der übergeordnete Ordner angegeben, in dem eine neue Landingpage erstellt wird. Dies erfolgt in Form eines eingebetteten JSON-Objekts, das `id` und `type` enthält.

Mit dem Parameter `template` wird die Vorlagen-ID der Quell-Landingpage angegeben.

Der optionale Parameter `description` wird verwendet, um die neue Landingpage zu beschreiben.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Abschnitt „Inhalt verwalten“

Inhaltsabschnitte werden nach ihrer Indexeigenschaft sortiert und letztendlich gemäß den CSS-Regeln angeordnet, die beim Anzeigen durch den Client angewendet werden. Die Inhaltsabschnitte werden mit den entsprechenden Endpunkten [Hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) und [Löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) des Inhalts der Landingpage eingeschlossen und verwaltet und können mithilfe von [Landingpage-Inhalt abfragen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Jeder Abschnitt verfügt über einen Typ und einen Wertparameter. Der Typ bestimmt, was in den Wert eingefügt werden soll.  Für diese Endpunkte werden Daten als POST x-www-form-urlencoded und nicht als JSON übergeben.

**Abschnittstypen**

| Typ | Wert |
|--- |--- |
| DynamicContent | Die ID der Segmentierung. |
| Formular | Die ID des Formulars. |
| HTML | Text HTML-Inhalt. |
| Bild | Die ID des Bild-Assets. |
| Rechteck | Leer. |
| RichText | Text HTML-Inhalt.  Darf nur Rich-Text-Elemente enthalten. |
| Ausschnitt | Die ID des Snippets. |
| Schaltfläche „Social“ | Die ID von  Die Schaltfläche „Social“. |
| Video | Die ID des Videos. |

Bei Freiformseiten müssen alle gewünschten Inhaltsabschnitte hinzugefügt werden und werden in das div-Element mit der ID `mktoContent` eingebettet. Für geführte Seiten kann eine Liste vordefinierter Elemente in der Liste vom Endpunkt [Landingpage-Inhalte abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) vorhanden sein. Weitere können über ihre jeweiligen Endpunkte hinzugefügt [aktualisiert](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) werden.

### Dynamischer Inhalt

Um einen Abschnitt mit dynamischen Inhalten zu erstellen, muss er bereits in der Inhaltsliste der Landingpage vorhanden sein. Der Endpunkt [Abschnitt Inhalt der Landingpage aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) muss dann verwendet werden, um den Typ auf „DynamicContent“ festzulegen. Wenn ein Abschnitt auf dynamische Inhalte festgelegt ist, werden zugrunde liegende dynamische Abschnitte im Inhaltsabschnitt erstellt, die alle den Basistyp des konvertierten Elements erben. Jeder dynamische Abschnitt übernimmt auch den Inhalt des konvertierten Abschnitts.

```
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[Inhalt wird aktualisiert](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) erfolgt für jedes einzelne Segment basierend auf der Segment-ID.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variablen

Zu den in geführten Landingpages eingeführten Funktionen gehören bearbeitbare Variablen.  Variablen enthalten Werte für Elemente auf einer Landingpage.  Variablen können einfach mit dem Landingpage-Editor wie unten gezeigt geändert werden:

![Landingpage-Variablen](assets/landing-page-variables.png)

Variablen werden als Meta-Tags innerhalb `<head>` Elements einer Landingpage-Vorlage mit geführtem Modus definiert. Es stehen drei Variablentypen zur Verfügung: Zeichenfolge, Farbe und Boolesch.  Im Folgenden finden Sie ein Beispiel für drei Variablendefinitionen:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Weitere Informationen finden Sie im Abschnitt „Bearbeitbare Variable“ in der Dokumentation [Erstellen einer geführten Landingpage](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Abfrage

Rufen Sie Variablen für eine geführte Landingpage ab, indem Sie die Landingpage-ID an den Endpunkt Landingpage-Variablen abrufen übergeben.

```
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

in  In diesem Beispiel enthält die geführte Landingpage drei Variablen: stringVar, colorVar, boolVar.

### Update

Aktualisieren Sie eine Variable für eine geführte Landingpage, indem Sie die Landingpage-ID, die Variable-ID und den Variablenwert übergeben, um den Endpunkt Landingpage-Variablen zu aktualisieren.

```
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Vorschau der Landingpage

Marketo stellt den Endpunkt [Vollständiger Inhalt der Landingpage abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) bereit, um eine Live-Vorschau einer Landingpage so abzurufen, wie sie in einem Browser gerendert würde. Es gibt einen erforderlichen Parameter, den `id`-Pfadparameter. Dies ist die ID der Landingpage, die Sie in der Vorschau anzeigen möchten. Es gibt zwei zusätzliche optionale Abfrageparameter:

- Segmentierung: Akzeptiert ein Array von JSON-Objekten, die die Attribute segmentationId und segmentId enthalten. Nach dem Festlegen wird die Landingpage in der Vorschau angezeigt, als ob Sie ein Lead wären, der diesen Segmenten entspricht.
- LeadId:  Akzeptiert die Ganzzahl-ID eines Leads. Nach dem Festlegen wird die Landingpage in der Vorschau angezeigt, als ob sie vom ausgewählten Lead angesehen worden wäre.

```
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
