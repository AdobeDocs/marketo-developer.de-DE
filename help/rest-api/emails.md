---
title: E-Mails
feature: REST API
description: APIs zum Bearbeiten von E-Mail-Assets.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# E-Mails

[E-Mail-](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails): Ein vollständiger Satz von REST-Endpunkten wird zur Bearbeitung von E-Mail-Assets bereitgestellt.

Hinweis: Wenn Sie [prädiktiven Marketo-Inhalt verwenden](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content) schlagen die folgenden Endpunkte fehl, wenn sie auf eine E-Mail verweisen, die prädiktiven Inhalt enthält: [E-Mail-Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Abschnitt E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) [E-Mail-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). Der Aufruf gibt einen 709-Fehler-Code und die entsprechende Fehlermeldung zurück.

## Abfrage

Das Abfragemuster für E-Mails ist identisch mit dem von Vorlagen und ermöglicht Abfragen [nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [nach ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) und [Browsing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET) sowie zum Filtern nach Ordner mit den APIs zum Durchsuchen und nach Namen.

Hinweis: Wenn eine E-Mail Teil eines E-Mail-Programms ist, das [A/B-Tests](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test) verwendet, ist diese E-Mail nicht für eine Abfrage mit den folgenden Endpunkten verfügbar: [E-Mail nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [E-Mail nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). Der Aufruf zeigt an, dass der Vorgang erfolgreich war, enthält jedoch die folgende Warnung: „Für die angegebenen Suchkriterien wurden keine Assets gefunden.“

### Nach ID

```
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Nach Name

Für nach Namen können Sie optional einen Ordner übergeben, um nur in diesem Ordner zu suchen.

```
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Durchsuchen

Das Durchsuchen von Ordnern funktioniert wie andere Asset-API-Durchsuchen-Endpunkte und ermöglicht das optionale Filtern nach `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` und `offset`. `status` ist entweder genehmigt oder Entwurf. `folder` ist ein JSON-Objekt, das `id` und `type` enthält. `maxReturn` ist eine Ganzzahl, die die Anzahl der Ergebnisse begrenzt (standardmäßig ist 20, der Höchstwert 200), und `offset` ist eine Ganzzahl, die mit `maxReturn` verwendet werden kann, um große Ergebnismengen zu lesen (der Standardwert ist 0).

```
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Anfrageinhalt

Sie können [die verfügbaren bearbeitbaren Abschnitte abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) für eine E-Mail, indem Sie deren Inhalt abfragen, und optional nach Status filtern, um die Abschnitte für die genehmigte oder die Entwurfsversion zu erhalten.

```
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Abschnitte können als mit dem Typ „dynamicContent“ zurückgegeben werden. Weitere Informationen finden Sie [ Abschnitt ](dynamic-content.md)Dynamische Inhalte“.

## CC-Felder abfragen

Sie können den Satz von Feldern abrufen, die für E-Mail-CC in der Zielinstanz aktiviert sind, indem Sie den Endpunkt [E-Mail-CC-Felder abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET) aufrufen.

```
GET /rest/asset/v1/email/ccFields.json
```

```json
{
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[
      {
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Erstellen und aktualisieren

[E-Mails werden basierend ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) einer Quellvorlage erstellt und enthalten eine Liste bearbeitbarer Abschnitte, die von jedem separaten HTML-Element in dieser Vorlage mit der Klasse „mktEditable“ und einer eindeutigen ID-Eigenschaft abgeleitet werden. Beim Erstellen einer E-Mail mit der API wird ein Datensatz erstellt, der auf der Vorlage basiert, zusammen mit allen übergebenen zusätzlichen Metadaten. Für einen erfolgreichen E-Mail-Aufruf von „Erstellen“ sind die folgenden Parameter erforderlich: Name, Vorlage, Ordner.

Die folgenden Parameter sind für die Erstellung optional: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Wenn nicht festgelegt, ist `subject` leer, `fromName`, `fromEmail` und `replyEmail` auf die Instanzstandardwerte festgelegt und `operational` und `isOpenTrackingDisabled` auf „false“ gesetzt. `isOpenTrackingDisabled` bestimmt, ob das Öffnungs-Tracking-Pixel beim Versand in einer E-Mail enthalten ist.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

[E-Mail-](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) aktualisieren) kann nach ID erfolgen. Dies ermöglicht die Aktualisierung der Beschreibung oder des Namens der E-Mail.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

### Inhaltsabschnitt, Typ und Aktualisierung

Der Inhalt für jeden Abschnitt einer E-Mail muss einzeln aktualisiert werden, mit Ausnahme des Betreffs, vonName, vonEmail und Antwort-E-Mail, die mit dem Endpunkt [E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) aktualisiert werden. Bei Verwendung dieses Endpunkts können diese Werte auch so festgelegt werden, dass dynamische Inhalte anstelle statischer Inhalte verwendet werden. Jeder Parameter ist ein Typ-Wert-JSON-Objekt, wobei der Typ entweder „Text“ oder „DynamicContent“ ist und der Wert entweder der entsprechende Textwert oder die ID der Segmentierung ist, die für den dynamischen Inhalt verwendet werden soll. Die Daten werden als POST x-www-form-urlencoded und nicht als JSON übergeben.  isOpenTrackingDisabled kann mit E-Mail-Inhalt aktualisieren festgelegt werden

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Wenn Sie für einen Abschnitt die Verwendung dynamischer Inhalte festlegen, muss die Abschnitt-ID über den Aufruf zum Abrufen von E-Mail-Inhalten abgerufen werden.

### Bearbeitbaren Abschnitt aktualisieren

Bearbeitbare Abschnitte werden durch ihre jeweiligen HTML-IDs aktualisiert. Nur die ID der E-Mail-Adresse und die htmlId des Abschnitts sind als Pfadparameter erforderlich, während Typ, Wert und textValue optional sind. Der Typ kann „Text“, „DynamicContent“ oder „Snippet“ lauten und beeinflusst, was im Wert übergeben wird. Wenn der Typ Text ist, ist der Wert eine Zeichenfolge, die den HTML-Inhalt des Abschnitts enthält. Handelt es sich um einen DynamicContent-Block, handelt es sich um einen JSON-Block mit drei Elementen vom Typ , der als „DynamicContent“ bezeichnet wird (Segmentierung ist die ID der für den Inhalt zu verwendenden Segmentierung), und als Standard , eine Zeichenfolge, die den standardmäßigen HTML-Inhalt des Abschnitts enthält. Der optionale textValue-Parameter ist eine Zeichenfolge, die die Textversion des Abschnitts enthält. Die Daten werden als POST x-www-form-urlencoded und nicht als JSON übergeben.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Hinweis: Wenn die automatische Textkopie für einen in eine E-Mail eingebetteten Ausschnitt deaktiviert ist, wird der HTML-Wert des Ausschnitts aktualisiert und dann wird die Textversion eines anderen Abschnitts in der E-Mail aktualisiert, dann enthält die Textversion der E-Mail Text, der den aktualisierten Wert des Ausschnitts HTML widerspiegelt, und nicht die vorherige Version, wie bei deaktivierter automatischer Textkopie erwartet würde.

## Module

Im E-Mail-Editor 1.0 ist ein Modul ein Abschnitt Ihrer E-Mail, der in der Vorlage definiert ist. Module können eine beliebige Kombination aus Elementen, Variablen und anderen HTML-Inhalten enthalten, wie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules) beschrieben. Marketo bietet eine Reihe von APIs zum Verwalten von Modulen innerhalb einer E-Mail. Bei modulbezogenen Endpunkten, für die die HTTP-POST-Methode erforderlich ist, wird der Hauptteil als „application/x-www-form-urlencoded“ formatiert (nicht als JSON).

Die meisten modulbezogenen Endpunkte erfordern eine „moduleId“ als Pfadparameter. Dies ist eine Zeichenfolge, die das Modul beschreibt. moduleIds werden vom Endpunkt [E-Mail-Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) als „htmlId“-Attribut zurückgegeben (siehe Abschnitt [Abfrage](#modules_query) unten).

### Abfrage

Um mit Modulen zu arbeiten, müssen Sie einen moduleId-Parameter angeben, der das Modul eindeutig identifiziert. Sie können auch den Modul-Indexparameter angeben. Dies ist eine Ganzzahl, die die Reihenfolge des Moduls in der E-Mail beschreibt.

[Rufen Sie moduleIds und ihre Indizes ab](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) indem Sie die E-Mail-ID als Pfadparameter angeben.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der Vorlage „Skelett“ im Abschnitt „Starter-Vorlagen“ der Benutzeroberfläche der Vorlagenauswahl abgefragt.

```
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {

      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

Das Ergebnis-Array enthält -Elemente, die sowohl die -Module als auch die gemischten HTML-Elemente beschreiben. Modulelemente enthalten das Attribut „contentType“: „module“ und das Attribut „index“. Die moduleId wird im Attribut „htmlId“ gespeichert.

Um mit dem obigen Beispiel „Skelett“ fortzufahren, enthält die folgende Tabelle eine Zusammenfassung der Modul-IDs und der entsprechenden Indizes, die in der E-Mail enthalten sind.

| moduleId (alias htmlId) | Index |
|---|---|
| Distanzstück | 0 |
| Frei-Bild | 1 |
| video | 2 |
| Freitext | 3 |
| CTA | 4 |
| Std | 5 |
| Zwei-Artikel | 6 |
| Fußzeile | 7 |

#### Hinzufügen

[Ein Modul hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) können Sie einer E-Mail hinzufügen, indem Sie eines der vorhandenen Module in der verwendeten E-Mail-Vorlage auswählen. Geben Sie dazu die E-Mail-ID und die moduleId als Pfadparameter an. Der Index-Abfrageparameter ist erforderlich und bestimmt die Reihenfolge des Moduls in der E-Mail. Wenn der Indexwert den größten vorhandenen Indexwert überschreitet, wird das Modul an die E-Mail angehängt.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Löschen

[Löschen eines Moduls](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) durch Angabe der E-Mail-ID und der moduleId als Pfadparameter.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Duplizieren

[Duplizieren Sie ein ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST), indem Sie die E-Mail-ID und die moduleId als Pfadparameter angeben. Dieser Aufruf dupliziert das Modul, platziert es unter dem ursprünglichen Modul und pusht die anderen Module nach unten.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### neu ordnen

[Module neu anordnen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)Array, das alle Module und für jedes die gewünschte Position innerhalb der E-Mail enthält. Jedes Array-Element enthält ein JSON-Objekt der folgenden Form:  { „index“: &lt;_index_>, „moduleId“: &quot;&lt;_moduleId_>&quot; }, wobei &lt;_index_> die Bestellnummer des nullbasierten Moduls und &lt;_moduleId_> die moduleId ist.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Umbenennen

[Benennen Sie ein Modul ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) einer E-Mail um, indem Sie den neuen Namen über den Parameter „name“ übergeben. Geben Sie die E-Mail-ID und die moduleId (vorhandener Name) als Pfadparameter an.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variablen

Im E-Mail-Editor 1.0 werden Variablen verwendet, um Werte für Elemente in Ihrer E-Mail zu speichern. Jede Variable wird definiert, indem Sie Ihrer HTML wie [ beschrieben eine Marketo-spezifische Syntax ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo bietet eine Reihe von APIs zum Verwalten von Variablen in einer E-Mail.

### Abfrage

[Abrufen von Variablen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) für eine E-Mail durch Angabe der E-Mail-ID als Pfadparameter.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der Vorlage „Skelett“ im Abschnitt „Starter-Vorlagen“ der Benutzeroberfläche der Vorlagenauswahl abgefragt.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

Das Ergebnis-Array enthält Elemente, die Variablen beschreiben, eine Variable pro Element.

Variablen können global auf die gesamte E-Mail oder lokal auf ein bestimmtes Modul angewendet werden. Variablen beider Bereiche enthalten die Attribute „name“, „value“ und „moduleScope“. Das Attribut „moduleScope“ ist ein boolescher Wert, wobei „false“ für „global“ und „true“ für „local“ steht. Lokale Variablen enthalten ein zusätzliches „moduleId“-Attribut, das das Modul angibt, mit dem die Variable verknüpft ist.

#### Update

[Aktualisieren Sie eine Variable](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) in einer E-Mail, indem Sie den neuen gewünschten Wert über den value-Parameter übergeben. Geben Sie die E-Mail-ID und den Variablennamen als Pfadparameter an. Wenn Sie eine Modulvariable aktualisieren, müssen Sie auch den moduleId-Parameter übergeben, um das Modul anzugeben, mit dem die Variable verknüpft ist.

Im folgenden Beispiel aktualisieren wir eine globale Variable mit dem Namen „hrBorderSize“ auf den Wert 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

Im folgenden Beispiel aktualisieren wir eine lokale Variable mit dem Namen „ctaLinkText“ auf den Wert „Click this button!“. in moduleId &quot;CTA&quot;.

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Genehmigung

E-Mails folgen dem Standardmuster für Genehmigungen von Asset-Datensätzen. Sie können einen Entwurf genehmigen, die Genehmigung einer genehmigten Version aufheben und einen vorhandenen Entwurf einer E-Mail über jeden ihrer eigenen Endpunkte verwerfen.

### Genehmigen

Beim Aufruf des Validierungsendpunkts wird die E-Mail anhand der Regeln für Marketo-E-Mails validiert. `from name`, `from email`, `reply to email` und `subject` müssen ausgefüllt werden, bevor die E-Mail genehmigt werden kann.

```
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Genehmigung aufheben

Der `unapprove` Vorgang kann nur für genehmigte E-Mails ausgeführt werden.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

#### Verwerfen

Die E-Mail muss den Status Entwurf aufweisen, damit sie verworfen werden kann. Eine genehmigte E-Mail kann nicht verworfen werden.

```
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Löschen

```
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Klonen

Marketo bietet eine einfache Methode zum Klonen einer E-Mail. Dieser Anfragetyp wird mit einem application/x-www-url-encoded POST durchgeführt und benötigt zwei erforderliche Parameter: Name und Ordner, ein eingebettetes JSON-Objekt mit der ID und dem Typ . Beschreibung ist auch ein optionaler Parameter. Wenn keine genehmigte Version vorhanden ist, wird die Entwurfsversion geklont.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Beispiel senden

Sie können über die API Trigger für eine Beispiel-E-Mail erstellen, die an den Abfrageparameter emailAddress gesendet wird. Sie können auch optional einen leadId-Parameter hinzufügen, um in Ihrer Datenbank die Identität eines bestimmten Leads einzunehmen, und einen textOnly-Parameter, um nur die Textversion der E-Mail zu senden.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## E-Mail in der Vorschau anzeigen

Marketo stellt den Endpunkt [Vollständiger Inhalt der E-Mail abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) bereit, um eine Live-Vorschau einer E-Mail so abzurufen, wie sie an einen Empfänger gesendet würde. Dieser Endpunkt kann nur bei E-Mails der Version 1.0 verwendet werden. Es gibt einen erforderlichen Parameter, den ID-Pfadparameter. Dies ist die ID des E-Mail-Assets, das Sie in der Vorschau anzeigen möchten. Es gibt drei zusätzliche optionale Abfrageparameter:

- Status: Akzeptiert die Werte „Entwurf“ oder „Genehmigt“, die standardmäßig auf die genehmigte Version gesetzt werden, falls genehmigt, Entwurf nicht genehmigt
- type: Akzeptiert „Text“ oder &quot;HTML&quot; und standardmäßig HTML
- leadId:. Akzeptiert die Ganzzahl-ID eines Leads. Nach der Einstellung wird die E-Mail so in der Vorschau angezeigt, als ob sie vom benannten Lead empfangen worden wäre

```
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## HTML ersetzen

Marketo stellt den Endpunkt [Vollständiger Inhalt der E-Mail aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) bereit, um den gesamten Inhalt eines E-Mail-Assets zu ersetzen. Dieser Endpunkt kann nur bei E-Mails der Version 1.0 verwendet werden, bei denen die UI-Funktion „Code bearbeiten“ verwendet wurde und bei denen die Beziehung zur übergeordneten Vorlage unterbrochen war. Diese API ist in erster Linie für die Verwendung bei Assets vorgesehen, die im Rahmen eines Programms geklont wurden, und kann nicht mit den Standard-Inhaltsendpunkten geändert werden. E-Mails mit dynamischen Inhalten werden nicht unterstützt. Wenn Sie außerdem versuchen, HTML in einer E-Mail zu ersetzen, in der die Beziehung intakt ist, wird ein Fehler zurückgegeben.

Dieser Endpunkt erwartet einen Inhaltstyp : multipart/form-data mit dem ID-Parameter im Pfad, der ID der E-Mail und einem Parameter im Textkörper, content als vollständiges HTML-E-Mail-Dokument mit dem Inhaltstyp „text/html“. Ein falsch formatiertes HTML-Dokument gibt eine Warnung aus, lässt jedoch möglicherweise keine Genehmigung zu, während die Aufnahme von JavaScript- und/oder `<script>`Tags in das Dokument dazu führt, dass der Aufruf fehlschlägt und einen Fehler ausgibt.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
