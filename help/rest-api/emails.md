---
title: "E-Mails"
feature: REST API
description: "APIs zum Bearbeiten von E-Mail-Assets."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---


# E-Mails

[Email Endpoint Reference](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Zur Bearbeitung von E-Mail-Assets wird ein vollständiger Satz von REST-Endpunkten bereitgestellt.

Hinweis: Wenn Sie [Marketo Predictive Content](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), schlagen die folgenden Endpunkte fehl, wenn sie auf eine E-Mail verweisen, die prädiktive Inhalte enthält: [E-Mail-Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Abschnitt &quot;E-Mail-Inhalt aktualisieren&quot;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [E-Mail-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). Der Aufruf gibt einen 709-Fehler-Code und die entsprechende Fehlermeldung zurück.

## Anfrage

Das Abfragemuster für E-Mails ist mit dem der Vorlagen identisch, sodass Abfragen möglich sind [by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [nach Namen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), und [Browsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET)und für die Filterung nach Ordner mit den Durchsuchen- und nach Namen-APIs.

Hinweis: Wenn eine E-Mail Teil eines E-Mail-Programms ist, das [A/B-Tests](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test)eingeben, ist diese E-Mail nicht für die Abfrage mit den folgenden Endpunkten verfügbar: [E-Mail nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [E-Mail nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). Der Aufruf gibt den Erfolg an, enthält jedoch die folgende Warnung: &quot;Für die angegebenen Suchkriterien wurden keine Assets gefunden.&quot;

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

Für nach Name können Sie optional einen Ordner übergeben, um nur in diesem Ordner zu suchen.

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

Das Durchsuchen von Ordnern funktioniert wie andere Asset-API-Durchsuchen-Endpunkte und ermöglicht das optionale Filtern nach `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn`, und `offset`. `status` ist entweder genehmigt oder Entwurf. `folder` ist ein JSON-Objekt, das `id` und `type`. `maxReturn` ist eine Ganzzahl, die die Anzahl der Ergebnisse begrenzt (standardmäßig 20, maximal 200), und `offset` ist eine Ganzzahl, die mit `maxReturn` zum Durchlesen großer Ergebnismengen (standardmäßig 0).

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

## Query Content

Sie können [Abrufen der verfügbaren bearbeitbaren Abschnitte](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) für eine E-Mail durch Abfrage des Inhalts und optional Filtern nach Status, um die Abschnitte für die Versionen &quot;Genehmigt&quot;oder &quot;Entwurf&quot;zu erhalten.

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

Abschnitte können als vom Typ dynamicContent zurückgegeben werden. Siehe [Dynamische Inhalte](dynamic-content.md) für weitere Informationen.

## Abfrage-CC-Felder

Sie können den für E-Mail CC aktivierten Feldsatz in der Zielinstanz abrufen, indem Sie die [E-Mail-CC-Felder abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET) -Endpunkt.

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

## Erstellen und Aktualisieren

[E-Mails werden erstellt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) basierend auf einer Quellvorlage erstellen und eine Liste bearbeitbarer HTML-Abschnitte haben, die von jedem separaten Element in dieser Vorlage mit der Klasse &quot;mktEditable&quot;und einer eindeutigen ID-Eigenschaft abgeleitet werden. Beim Erstellen einer E-Mail mit der API wird ein Datensatz basierend auf der Vorlage sowie auf weitergeleiteten Metadaten erstellt. Die folgenden Parameter sind für einen erfolgreichen Aufruf zum Erstellen einer E-Mail erforderlich: Name, Vorlage, Ordner.

Die folgenden Parameter sind optional für die Erstellung: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Wenn nicht festgelegt, `subject` leer ist, `fromName`, `fromEmail` und `replyEmail` auf die Instanzenstandardwerte festgelegt werden und `operational` und `isOpenTrackingDisabled` nicht korrekt sein. `isOpenTrackingDisabled` bestimmt, ob das Öffnungs-Tracking-Pixel beim Versand in eine E-Mail aufgenommen wird.

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

[E-Mails aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) -Datensatz kann anhand der ID erstellt werden. Dadurch kann die Beschreibung oder der Name der E-Mail aktualisiert werden.

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

### Inhaltsabschnitt, -typ und -aktualisierung

Der Inhalt für jeden Abschnitt einer E-Mail muss einzeln aktualisiert werden, mit Ausnahme des Betreffs, von &quot;Name&quot;, &quot;E-Mail&quot;und &quot;responseEmail&quot;, die mithilfe der Variablen [E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) -Endpunkt. Bei Verwendung dieses Endpunkts können diese Werte auch so eingestellt werden, dass dynamische Inhalte anstelle statischer Inhalte verwendet werden. Jeder Parameter ist ein Typ/Wert-JSON-Objekt, bei dem der Typ entweder &quot;Text&quot;oder &quot;DynamicContent&quot;ist und der Wert entweder der geeignete Textwert oder die ID der Segmentierung ist, die für den dynamischen Inhalt verwendet werden soll. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.  isOpenTrackingDisabled kann mit &quot;E-Mail-Inhalt aktualisieren&quot;eingestellt werden

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

Wenn Sie einen Abschnitt so einstellen, dass er dynamischen Inhalt verwendet, muss die Bereichs-ID über den Aufruf &quot;E-Mail-Inhalt abrufen&quot;abgerufen werden.

### Bearbeitbare Abschnitte aktualisieren

Bearbeitbare Abschnitte werden durch ihre einzelnen htmlIds aktualisiert. Als Pfadparameter sind nur die ID der E-Mail und die htmlId des Abschnitts erforderlich, während Typ, Wert und textValue optional sind. Der Typ kann &quot;Text&quot;, &quot;DynamicContent&quot;oder &quot;Snippet&quot;sein und wirkt sich auf den im Wert übergebenen Wert aus. Wenn der Typ Text ist, ist der Wert eine Zeichenfolge, die den HTML-Inhalt des Abschnitts enthält. Wenn es sich um DynamicContent handelt, handelt es sich um einen JSON-Block mit drei Mitgliedern, Typ: &quot;DynamicContent&quot;, Segmentierung, die die ID der für den Inhalt zu verwendenden Segmentierung ist, und Standard, bei der es sich um eine Zeichenfolge mit dem Standardinhalt für HTML des Abschnitts handelt. Der optionale Parameter textValue ist eine Zeichenfolge, die die Textversion des Abschnitts enthält. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.

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

Hinweis: Wenn die automatische Textkopie für ein in eine E-Mail eingebettetes Snippet deaktiviert ist, dann wird der HTML-Wert des Snippets aktualisiert und dann die Textversion eines anderen Abschnitts in der E-Mail aktualisiert. In der Textversion der E-Mail wird Text angezeigt, der den aktualisierten Wert des Snippet-HTML widerspiegelt, nicht die vorherige, wie bei deaktivierter automatischer Kopie erwartet wird.

## Module

In Email Editor 1.0 ist ein Modul ein Abschnitt Ihrer E-Mail, der in der Vorlage definiert ist. Module können eine beliebige Kombination aus Elementen, Variablen und anderem HTML-Inhalt enthalten, wie beschrieben [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo bietet eine Reihe von APIs zum Verwalten von Modulen in einer E-Mail. Bei modulbezogenen Endpunkten, für die die HTTP-POST-Methode erforderlich ist, wird der Hauptteil als &quot;application/x-www-form-urlencoded&quot;(nicht als JSON) formatiert.

Die meisten modulbezogenen Endpunkte erfordern eine &quot;moduleId&quot;als Pfadparameter. Dies ist eine Zeichenfolge, die das -Modul beschreibt. moduleIds werden von [E-Mail-Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) Endpunkt als Attribut &quot;htmlId&quot; (siehe [Abfrage](#modules_query) unten).

### Anfrage

Um mit Modulen zu arbeiten, müssen Sie einen moduleId -Parameter angeben, der das Modul eindeutig identifiziert. Sie müssen auch den Modulindex-Parameter angeben, d. h. eine Ganzzahl, die die Reihenfolge des Moduls in der E-Mail beschreibt.

[Abrufen von moduleIds und ihren Indizes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) durch Angabe der E-Mail-ID als Pfadparameter.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der Vorlage &quot;Skelett&quot;abgefragt, die im Abschnitt &quot;Startervorlagen&quot;der Vorlagenauswahl-Benutzeroberfläche zu finden ist.

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

Das Ergebnis-Array enthält Elemente, die sowohl Module als auch HTML-Elemente miteinander beschreiben. Modulelemente enthalten das Attribut &quot;contentType&quot;: &quot;Module&quot; und das Attribut &quot;index&quot;. Die moduleId wird im Attribut &quot;htmlId&quot;gespeichert.

Im weiteren Verlauf des obigen Beispiels &quot;Skelett&quot;enthält die folgende Tabelle eine Zusammenfassung der moduleIds und der zugehörigen in der E-Mail enthaltenen Indizes.

| moduleId (alias htmlId) | Index |
|---|---|
| Abstandhalter | 0 |
| free-image | 1 |
| Video | 2 |
| free-text | 3 |
| CTA | 4 |
| hr | 5 |
| Zweiartikel | 6 |
| Fußzeile | 7 |

#### Hinzufügen

[Modul hinzufügen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) zu einer E-Mail hinzufügen, indem Sie aus einem der vorhandenen Module in der verwendeten E-Mail-Vorlage auswählen. Geben Sie dazu die E-Mail-ID und die moduleId als Pfadparameter an. Der Index-Abfrageparameter ist erforderlich und bestimmt die Reihenfolge des Moduls in der E-Mail. Wenn der Indexwert den größten vorhandenen Indexwert überschreitet, wird das Modul an die E-Mail angehängt.

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

[Modul löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) durch Angabe der E-Mail-ID und der moduleId als Pfadparameter.

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

#### Doppelt

[Modul duplizieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) durch Angabe der E-Mail-ID und der moduleId als Pfadparameter. Dieser Aufruf dupliziert das Modul, platziert es unter dem ursprünglichen Modul und drückt die anderen Module nach unten.

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

#### Neu anordnen

[Module neu anordnen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)-Array, das alle Module und die gewünschte Position innerhalb der E-Mail für jedes enthält. Jedes Array-Element enthält ein JSON-Objekt des folgenden Formulars: { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, wobei &lt;_index_> ist die auf null basierende Modulbestellnummer und &lt;_moduleId_> ist die moduleId.

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

[Umbenennen eines Moduls](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) in einer E-Mail durch Übergabe des neuen Namens über den Parameter name . Geben Sie die E-Mail-ID und die moduleId (vorhandener Name) als Pfadparameter an.

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

In Email Editor 1.0 werden Variablen verwendet, um Werte für Elemente in Ihrer E-Mail zu speichern. Jede Variable wird definiert, indem Sie Ihrer HTML eine Marketo-spezifische Syntax hinzufügen, wie beschrieben [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo bietet eine Reihe von APIs zum Verwalten von Variablen in einer E-Mail.

### Anfrage

[Variablen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) für eine E-Mail durch Angabe der E-Mail-ID als Pfadparameter.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der Vorlage &quot;Skelett&quot;abgefragt, die im Abschnitt &quot;Startervorlagen&quot;der Vorlagenauswahl-Benutzeroberfläche zu finden ist.

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

Variablen können global auf die gesamte E-Mail oder lokal auf ein bestimmtes Modul übertragen werden. Variablen beider Perimeter enthalten die Attribute &quot;name&quot;, &quot;value&quot;und &quot;moduleScope&quot;. Das Attribut &quot;moduleScope&quot;ist boolesch, wobei false &quot;global&quot;und &quot;true&quot;&quot;local&quot;angibt. Lokale Variablen enthalten ein zusätzliches Attribut &quot;moduleId&quot;, das das Modul angibt, mit dem die Variable verknüpft ist.

#### Aktualisierung

[Variable aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) in einer E-Mail, indem der neue gewünschte Wert über den Parameter value übergeben wird. Geben Sie die E-Mail-ID und den Variablennamen als Pfadparameter an. Wenn Sie eine Modulvariable aktualisieren, müssen Sie auch den Parameter moduleId übergeben, um das Modul anzugeben, mit dem die Variable verknüpft ist.

Im folgenden Beispiel wird eine globale Variable mit dem Namen &quot;hrBorderSize&quot;auf den Wert 1 aktualisiert.

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

Im folgenden Beispiel aktualisieren wir eine lokale Variable namens &quot;ctaLinkText&quot;auf den Wert &quot;Click this button!&quot; in moduleId &quot;CTA&quot;.

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

E-Mails folgen dem Standardmuster für die Validierung von Asset-Datensätzen. Sie können einen Entwurf genehmigen, die Genehmigung einer genehmigten Version aufheben und einen vorhandenen Entwurf einer E-Mail über jeden ihrer Endpunkte verwerfen.

### Genehmigen

Beim Aufrufen des Validierungsendpunkts wird die E-Mail anhand der Regeln für Marketo-E-Mails validiert. Die `from name`, `from email`, `reply to email`, und `subject` müssen ausgefüllt werden, bevor die E-Mail genehmigt werden kann.

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

Die `unapprove` -Vorgang kann nur für genehmigte E-Mails durchgeführt werden.

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

Die E-Mail muss sich im Entwurfsstatus befinden, damit sie verworfen werden kann. Eine genehmigte E-Mail kann nicht verworfen werden.

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

Marketo bietet eine einfache Methode zum Klonen von E-Mails. Dieser Anfragetyp wird mit einer POST application/x-www-url-urlencoded durchgeführt und verwendet zwei erforderliche Parameter: Name und Ordner, ein eingebettetes JSON-Objekt mit ID und Typ. description ist auch ein optionaler Parameter. Wenn keine genehmigte Version vorhanden ist, wird der Entwurf geklont.

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

Sie können eine Beispiel-E-Mail über die API Trigger haben, die an den Abfrageparameter emailAddress gesendet wird. Sie können optional auch einen LeadId-Parameter hinzufügen, um sich als ein bestimmtes Lead aus Ihrer Datenbank auszugeben, und einen textOnly-Parameter, um nur die Textversion der E-Mail zu senden.

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

Marketo stellt die [E-Mail-Inhalt vollständig abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) Endpunkt zum Abrufen einer Live-Vorschau einer E-Mail, wie sie an einen Empfänger gesendet wird. Dieser Endpunkt kann nur für E-Mails der Version 1.0 verwendet werden. Es gibt einen erforderlichen Parameter: den Parameter id path , der die ID des E-Mail-Assets darstellt, das Sie in der Vorschau anzeigen möchten. Es gibt drei zusätzliche optionale Abfrageparameter:

- status: Akzeptiert die Werte &quot;draft&quot;oder &quot;authorised&quot;, die standardmäßig für die genehmigte Version verwendet werden, falls sie genehmigt wurde, Entwurf, falls nicht genehmigt
- Typ: Akzeptiert &quot;Text&quot; oder &quot;HTML&quot; und standardmäßig HTML
- leadId:. Akzeptiert die Ganzzahl-ID eines Leads. Wenn diese Einstellung festgelegt ist, wird die E-Mail in der Vorschau angezeigt, als ob sie vom dafür vorgesehenen Lead empfangen worden wäre

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

Marketo stellt die [E-Mail-Inhalt aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) -Endpunkt verwenden, um den gesamten Inhalt eines E-Mail-Assets zu ersetzen. Dieser Endpunkt kann nur für E-Mails der Version 1.0 verwendet werden, für die die Benutzeroberflächenfunktion &quot;Code bearbeiten&quot;verwendet wurde und die Beziehung zu ihrer übergeordneten Vorlage beschädigt wurde. Diese API ist in erster Linie für die Verwendung von Assets vorgesehen, die im Rahmen eines Programms geklont wurden, und kann nicht mit den Endpunkten für Standardinhalte geändert werden. E-Mails mit dynamischen Inhalten werden nicht unterstützt. Wenn Sie versuchen, HTML in einer E-Mail zu ersetzen, in der die Beziehung intakt ist, wird ein Fehler zurückgegeben.

Dieser Endpunkt erwartet einen Inhaltstyp : multipart/form-data mit dem id -Parameter im Pfad, der ID der E-Mail und einem -Parameter im Hauptteil, Inhalt als vollständiges HTML-E-Mail-Dokument mit dem Inhaltstyp &quot;text/html&quot;. Ein falsch formatiertes HTML-Dokument gibt eine Warnung aus, lässt jedoch die Genehmigung unter Verwendung von JavaScript und/oder `<script>`Tags im Dokument führen dazu, dass der Aufruf fehlschlägt und ein Fehler ausgegeben wird.

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
