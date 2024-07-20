---
title: Landing Page
feature: REST API, Landing Pages
description: Abfragen von Landingpages in Marketo.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 2%

---

# Landing Page

[Endpunktverweis für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Landingpages sind Webseiten, die von Marketo gehostet werden.

## Anfrage

Wie die meisten anderen Assets können Einstiegsseiten [nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) und durch [Durchsuchen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET) abgefragt werden. Diese Abfragen geben nur Metadaten zurück. Die Liste der Inhaltsabschnitte für eine Landingpage muss separat durch die ID der Landingpage abgefragt werden.

Durch Abfrage des Inhalts der Landingpage erhalten Sie eine Liste der auf der Landingpage verfügbaren Inhaltsabschnitte. In der Inhaltsliste einer Seite muss ein Abschnitt vorhanden sein, um den Inhalt aktualisieren zu können:

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

Die Ergebnisse unterscheiden sich zwischen Vorlagen für geführtes und freies Formular, da geführte Landingpages eine Reihe von Abschnitten enthalten, die durch die Vorlage definiert sind, von der sie abgeleitet werden. Freiformseiten enthalten dagegen keine vordefinierten Abschnitte und ihr Inhalt muss vor der Bearbeitung hinzugefügt werden.  Beachten Sie, dass das Format des Attributs &quot;content&quot;je nach Attribut &quot;type&quot;variieren kann und ob das Feld statisch oder dynamisch ist.

## Erstellen und Aktualisieren

[Landingpages werden erstellt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST), indem auf eine Vorlage verwiesen wird. Die einzigen erforderlichen Felder für die Erstellung sind Name, Vorlage (die ID der Vorlage) und Ordner, in dem die Seite platziert werden soll. Weitere Metadaten, die aufgefüllt werden können, finden Sie in der Endpunktreferenz .

Gültige Inhaltstypen für die Endpunkte [Inhalt der Landingpage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) sind: richText, HTML, Formular, Bild, Rechteck, Snippet.

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

Landingpage-Metadaten können mit dem Endpunkt [Landingpage-Metadaten-Endpunkt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST) aktualisiert werden.

## Genehmigung

Landingpages folgen dem standardmäßigen Entwurf-genehmigten Modell, bei dem es eine Entwurfsversion und/oder eine genehmigte Version geben kann. Wenn Aktualisierungen auf eine Seite angewendet werden, werden sie immer zuerst auf den Entwurf der Version angewendet und erst live angezeigt, wenn die Seite genehmigt wurde.

## Löschen

Um eine Landingpage zu löschen, muss sie zunächst nicht verwendet werden und nicht von anderen Marketo-Assets referenziert und auch nicht genehmigt werden. Seiten werden einzeln mit dem Endpunkt [Einstiegsseite löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST) gelöscht. Landingpages mit eingebetteten Social-Schaltflächen können über diese API nicht gelöscht werden. 

## Klonen

Marketo bietet eine einfache Methode zum Klonen einer Landingpage. Dies ist eine Anfrage zur POST der Anwendung/x-www-url-formencoded .

Der Pfadparameter `id` gibt die ID der zu klonenden Quelleinstiegsseite an.

Der Parameter `name` wird verwendet, um den Namen der neuen Einstiegsseite anzugeben.

Der Parameter `folder` wird verwendet, um den übergeordneten Ordner anzugeben, in dem die neue Einstiegsseite erstellt wird. Dies geschieht in Form eines eingebetteten JSON-Objekts, das `id` und `type` enthält.

Der Parameter `template` wird verwendet, um die Quell-Landingpage-Vorlagen-ID anzugeben.

Der optionale Parameter `description` wird zur Beschreibung der neuen Landingpage verwendet.

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

## Inhaltsbereich verwalten

Inhaltsabschnitte werden anhand ihrer Indexeigenschaft geordnet und letztendlich gemäß den CSS-Regeln angeordnet, die bei der Anzeige durch den Client angewendet werden. Inhaltsbereiche werden mit den entsprechenden Endpunkten [Hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) und [Löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) des Einstiegsseiteninhalts eingeschlossen und verwaltet und können mit [Einstiegsseiteninhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) abgefragt werden. Jeder Abschnitt hat einen Typ und einen Werteparameter. Der Typ bestimmt, was in den Wert eingefügt werden soll.  Für diese Endpunkte werden Daten als POST x-www-form-urlencoded und nicht als JSON übergeben.

**Abschnittstypen**

| Typ | Wert |
|--- |--- |
| DynamicContent | Die ID der Segmentierung. |
| Formular | Die ID des Formulars. |
| HTML | HTML-Textinhalt. |
| Bild | Die ID des Bild-Assets. |
| Rechteck | Leer. |
| RichText | HTML-Textinhalt.  Darf nur Rich-Text-Elemente enthalten. |
| Ausschnitt | Die ID des Ausschnitts. |
| SocialButton | Die ID von  Wählen Sie die Social -Schaltfläche aus. |
| Video | Die ID des Videos. |

Für Seiten mit kostenlosen Formularen müssen alle gewünschten Inhaltsabschnitte hinzugefügt und mit der ID `mktoContent` in das div -Element eingebettet werden. Für geführte Seiten kann eine Liste vordefinierter Elemente in der Liste vom Endpunkt [Inhalt der Einstiegsseite abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) vorhanden sein. Über ihre jeweiligen Endpunkte können weitere hinzugefügt oder ihr [Inhalt aktualisiert](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) werden.

### Dynamischer Inhalt

Um einen Abschnitt mit dynamischen Inhalten zu erstellen, muss dieser bereits in der Inhaltsliste der Landingpage vorhanden sein. Der Endpunkt [Inhalt der Landingpage aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) muss dann verwendet werden, um den Typ auf &quot;DynamicContent&quot;festzulegen. Wenn ein Abschnitt auf dynamischen Inhalt festgelegt ist, erstellt er zugrunde liegende dynamische Abschnitte im Inhaltsabschnitt, die alle den Basistyp des konvertierten Elements übernehmen. Jeder dynamische Abschnitt übernimmt auch den Inhalt aus dem konvertierten Abschnitt.

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

[Die Aktualisierung des Inhalts](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) für jedes einzelne Segment erfolgt auf Basis der Segment-ID.

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

Eine der Funktionen, die in geführten Landingpages eingeführt wurden, sind bearbeitbare Variablen.  Variablen enthalten Werte für Elemente auf einer Landingpage.  Variablen können einfach mit dem Landingpage-Editor wie unten gezeigt geändert werden:

![Landingpage-Variablen](assets/landing-page-variables.png)

Variablen werden als Meta-Tags innerhalb des Elements `<head>` einer Landingpage-Vorlage für den geführten Modus definiert. Es stehen drei Variablentypen zur Verfügung: Zeichenfolge, Farbe und Boolesch.  Im Folgenden finden Sie ein Beispiel für drei Variablendefinitionen:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Weitere Informationen finden Sie im Abschnitt &quot;Bearbeitbare Variable&quot;in der Dokumentation zum Erstellen einer geführten Landingpage-Vorlage ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).[

### Anfrage

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

In  In diesem Beispiel enthält die geführte Landingpage 3 Variablen: stringVar, colorVar, boolVar.

### Aktualisierung

Aktualisieren Sie eine Variable für eine geführte Landingpage, indem Sie die Landingpage-ID, die Variablenkennung und den Variablenwert an den Endpunkt Landingpage-Variablen aktualisieren übergeben.

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

Marketo stellt den Endpunkt [Vollständigen Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) bereit, um eine Live-Vorschau einer Landingpage so abzurufen, wie sie in einem Browser gerendert würde. Es gibt einen erforderlichen Parameter, den Pfadparameter `id` , der die ID der Landingpage darstellt, die Sie in der Vorschau anzeigen möchten. Es gibt zwei zusätzliche optionale Abfrageparameter:

- segmentation: Akzeptiert ein Array von JSON-Objekten, die segmentationId- und segmentId-Attribute enthalten. Wenn diese Einstellung festgelegt ist, wird eine Vorschau der Landingpage so angezeigt, als ob Sie ein Lead wären, der mit diesen Segmenten übereinstimmt.
- leadId:  Akzeptiert die Ganzzahl-ID eines Leads. Wenn diese Einstellung festgelegt ist, wird eine Vorschau der Landingpage so angezeigt, als ob sie vom festgelegten Lead angezeigt wurde.

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
