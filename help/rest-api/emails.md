---
title: E-Mails
feature: REST API
description: Erfahren Sie, wie Sie mit der Marketo Asset REST-API E-Mail-Assets nach ID, Name oder Ordnersuche abfragen und verwalten können, einschließlich Hinweisen zu prädiktiven Inhalten und A/B-Testbeschränkungen.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
TQID: https://experienceleague.adobe.com/t2FyPbwS836MvOe5rL0rVS7ibtzzZMmXwmgHBDZEr8Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1813
ht-degree: 2%

---

# E-Mails

[E-Mail-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails)

Verwenden Sie die E-Mail-REST-Endpunkte, um E-Mail-Assets abzufragen und zu verwalten.

Wenn eine E-Mail [Marketo Predictive Content](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content) enthält, schlagen die folgenden Endpunkte mit Fehlercode 709 und einer entsprechenden Fehlermeldung fehl:

- [E-Mail-Inhalt abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET)
- [Abschnitt zum Aktualisieren des E-Mail-Inhalts](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST)
- [E-Mail-Entwurf genehmigen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/approveDraftUsingPOST)

## Abfrage

E-Mails unterstützen dieselben Abfragemuster wie Vorlagen: [nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET) und durch [Browsen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET). Die Endpunkte „By-Name“ und „Durchsuchen“ unterstützen auch die Ordnerfilterung.

Wenn eine E-Mail zu einem E-Mail-Programm gehört, das [A/B-Tests](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test) verwendet, geben die folgenden Endpunkte diese E-Mail nicht zurück:

- [E-Mail nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET)
- [E-Mail nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET)
- [E-Mails abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET)

Der Aufruf zeigt den Erfolg an, enthält jedoch die `No assets found for the given search criteria.`

### Nach ID

```http
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

Bei der Namensabfrage können Sie optional einen Ordner übergeben, um die Suche auf diesen Ordner zu beschränken.

```http
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

Das E-Mail-Browsen folgt dem Standard-Asset-API-Muster und unterstützt diese optionalen Filter:

- `status`: Entweder `Approved` oder `Draft`.
- `folder`: Ein JSON-Objekt, das `id` und `type` enthält.
- `earliestUpdatedAt` und `latestUpdatedAt`: Der Zeitraum für die Aktualisierung.
- `maxReturn`: Die Anzahl der zurückzugebenden Ergebnisse. Der Standardwert ist 20, der Maximalwert 200.
- `offset`: Funktioniert mit `maxReturn`, um durch große Ergebnismengen zu blättern. Der Standardwert ist 0.

```http
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

Um [&#x200B; bearbeitbaren Abschnitte einer E-Mail abzurufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET) fragen Sie deren Inhalt ab. Optional können Sie nach Status filtern, um Abschnitte aus der Version Genehmigt oder Entwurf zurückzugeben.

```http
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

Ein Abschnitt kann einen `dynamicContent` aufweisen. Weitere Informationen finden Sie unter [Dynamische Inhalte](dynamic-content.md).

## CC-Felder abfragen

Rufen Sie den Endpunkt [E-Mail-CC-Felder abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailCCFieldsUsingGET) auf, um die für E-Mail-CC aktivierten Felder in der Zielinstanz abzurufen.

```http
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

[Erstellen einer E](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailUsingPOST)Mail über eine Quellvorlage. Die bearbeitbaren Abschnitte der E-Mail stammen aus den HTML-Elementen der Vorlage, die die `mktEditable`-Klasse und eine eindeutige `id`-Eigenschaft aufweisen.

Für den Aufruf „E-Mail erstellen“ sind die folgenden Parameter erforderlich:

- `name`: Der E-Mail-Name.
- `template`: Die Quellvorlage.
- `folder`: Der übergeordnete Ordner.

Die optionalen Erstellungsparameter sind `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational` und `isOpenTrackingDisabled`. Die folgenden Standardwerte gelten, wenn diese Parameter weggelassen werden:

- `subject` ist leer.
- `fromName`, `fromEmail` und `replyEmail` verwenden die Instanzstandardwerte.
- `operational` und `isOpenTrackingDisabled` sind `false`.

Der `isOpenTrackingDisabled` bestimmt, ob die gesendete E-Mail das Öffnungs-Tracking-Pixel enthält.

```http
POST /rest/asset/v1/emails.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Um [E-Mail aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST) übergeben Sie ihre ID und aktualisieren Sie die Beschreibung oder den Namen der E-Mail.

```http
POST /rest/asset/v1/email/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Aktualisieren Sie jeden Abschnitt mit E-Mail-Inhalt einzeln. Verwenden Sie den Endpunkt [E-Mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST)Inhalt aktualisieren, um `subject`, `fromName`, `fromEmail` und `replyEmail` zu aktualisieren. Mit diesem Endpunkt können Sie diese Werte auch so einstellen, dass dynamische Inhalte anstelle statischer Inhalte verwendet werden.

Jeder Parameter ist ein JSON-Objekt vom Typ/Wert. Der Typ ist `Text` oder `DynamicContent`. Der Wert ist der entsprechende Text oder die ID der Segmentierung, die für dynamische Inhalte verwendet wird. Senden Sie die Daten als POST-Anfrage mit `application/x-www-form-urlencoded` und nicht als JSON. Sie können `isOpenTrackingDisabled` auch mit E-Mail-Inhalt aktualisieren festlegen.

```http
POST /rest/asset/v1/email/{id}/content.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Um einen Abschnitt für die Verwendung dynamischer Inhalte zu konfigurieren, rufen Sie zunächst dessen Abschnitt-ID mit E-Mail-Inhalt abrufen ab.

### Bearbeitbaren Abschnitt aktualisieren

Aktualisieren eines bearbeitbaren Abschnitts durch seine `htmlId`. E-Mail-`id` und Abschnitt `htmlId` sind erforderliche Pfadparameter. Die Parameter `type`, `value` und `textValue` sind optional.

Die `type` bestimmt den Inhalt von `value`:

- `Text`: `value` ist eine Zeichenfolge, die den HTML-Inhalt des Abschnitts enthält.
- `DynamicContent`: `value` ist ein JSON-Objekt, das `type` auf `DynamicContent`, `segmentation` auf die Segmentierungs-ID und `default` auf den HTML-Standardinhalt setzt.
- `Snippet`: Ein unterstützter Abschnittstyp.

Der optionale `textValue` enthält die Textversion des Abschnitts. Senden Sie die Daten als POST-Anfrage mit `application/x-www-form-urlencoded` und nicht als JSON.

```http
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Wenn die automatische Textkopie für ein eingebettetes Snippet deaktiviert ist, wird beim Aktualisieren des Snippets HTML und der anschließenden Aktualisierung der Textversion eines anderen Abschnitts die E-Mail-Textversion dem aktualisierten Snippet HTML angezeigt. Der vorherige Snippet-Text wird nicht wie erwartet beibehalten, wenn die automatische Kopie deaktiviert ist.

## Module

Im E-Mail-Editor 1.0 ist ein Modul ein E-Mail-Abschnitt, der in der Vorlage definiert ist. Module können Elemente, Variablen und andere HTML-Inhalte enthalten, wie unter [E-Mail-Vorlagensyntax](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules) beschrieben.

Verwenden Sie die Modul-APIs, um Module innerhalb einer E-Mail zu verwalten. Formatieren Sie den Anfragetext für Modulendpunkte, die HTTP-POST verwenden, als `application/x-www-form-urlencoded` und nicht als JSON.

Die meisten Modulendpunkte erfordern `moduleId` als Pfadparameter. Der Endpunkt [E-Mail-](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET) abrufen“ gibt Modul-IDs im `htmlId`-Attribut zurück. Siehe [Abfrage](#modules_query).

### Abfrage

Um mit Modulen zu arbeiten, geben Sie die `moduleId` an, die das Modul eindeutig identifiziert. Möglicherweise benötigen Sie auch den ganzzahligen Modulindex, der die Reihenfolge des Moduls in der E-Mail beschreibt.

Um [Modul-IDs und ihre Indizes abzurufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET) geben Sie die E-Mail-ID als Pfadparameter an.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der `Skeleton` im Abschnitt Starter-Vorlagen der Benutzeroberfläche der Vorlagenauswahl abgefragt.

```http
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
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you have subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you have subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
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

Das Ergebnis-Array enthält sowohl Modul- als auch HTML-Elementbeschreibungen. Modulelemente haben eine `contentType` von `Module` und enthalten eine `index`. Das `htmlId`-Attribut enthält die `moduleId`.

Im `Skeleton` Beispiel ordnet die folgende Tabelle jede `moduleId` ihrem Index in der E-Mail zu.

| moduleId (alias htmlId) | Index |
| --- | --- |
| Distanzstück | 0 |
| Frei-Bild | 1 |
| video | 2 |
| Freitext | 3 |
| CTA | 4 |
| Std | 5 |
| Zwei-Artikel | 6 |
| Fußzeile | 7 |

#### Hinzufügen

Um [Modul hinzufügen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/addModuleUsingPOST) wählen Sie ein vorhandenes Modul aus der E-Mail-Vorlage aus. Geben Sie die E-Mail-ID und `moduleId` als Pfadparameter an. Der erforderliche `index` Abfrageparameter bestimmt die Position des Moduls. Wenn `index` den größten vorhandenen Index überschreitet, hängt die API das Modul an die E-Mail an.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
index=10
```

```json
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

Um [Modul zu löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/deleteModuleUsingPOST) geben Sie die E-Mail-ID und `moduleId` als Pfadparameter an.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```json
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

Um [Modul zu duplizieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/duplicateModuleUsingPOST) geben Sie die E-Mail-ID und `moduleId` als Pfadparameter an. Die API platziert das Duplikat unter dem ursprünglichen Modul und verschiebt die verbleibenden Module nach unten.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```json
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

Um [Module neu anzuordnen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/rearrangeModulesUsingPOST) senden Sie ein -Array, das jedes Modul und seine gewünschte Position enthält. Jedes Array-Element ist ein JSON-Objekt im `{ "index": <_index_>, "moduleId": "<_moduleId_>" }`, wobei `<_index_>` die Position des Moduls auf Basis null und `<_moduleId_>` die Modul-ID ist.

```http
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```json
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

Um [Modul umzubenennen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/renameUsingPOST) übergeben Sie den neuen Namen im `name`. Geben Sie die E-Mail-ID und vorhandene `moduleId` als Pfadparameter an.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=MarketoVideo
```

```json
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

Im E-Mail-Editor 1.0 speichern Variablen Werte für E-Mail-Elemente. Definieren Sie jede Variable, indem Sie der HTML eine Marketo-spezifische Syntax hinzufügen, wie unter [E-Mail-Vorlagensyntax](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables) beschrieben. Verwenden Sie die Variablen-APIs, um Variablen innerhalb einer E-Mail zu verwalten.

### Abfrage

Um [Variablen abzurufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailVariablesUsingGET) geben Sie die E-Mail-ID als Pfadparameter an.

Im folgenden Beispiel wird eine 1.0-E-Mail basierend auf der `Skeleton` im Abschnitt Starter-Vorlagen der Benutzeroberfläche der Vorlagenauswahl abgefragt.

```http
GET /rest/asset/v1/email/{id}/variables.json
```

```json
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

Jedes Element im Ergebnis-Array beschreibt eine Variable.

Variablen können für die gesamte E-Mail einen globalen Gültigkeitsbereich oder für ein Modul einen lokalen Gültigkeitsbereich haben. Jede Variable enthält `name`-, `value`- und `moduleScope`. Das boolesche `moduleScope` `false`-Attribut wird für globale Variablen und für lokale Variablen `true`. Eine lokale Variable enthält auch die `moduleId` des zugehörigen Moduls.

#### Aktualisierung

Um [Variable zu aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateVariableUsingPOST) übergeben Sie den neuen Wert im `value`. Geben Sie die E-Mail-ID und den Variablennamen als Pfadparameter an. Übergeben Sie beim Aktualisieren einer Modulvariablen auch `moduleId` , um das zugehörige Modul zu identifizieren.

Im folgenden Beispiel wird die globale Variable `hrBorderSize` aktualisiert.

```http
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```text
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```text
value=2
```

```json
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

Im folgenden Beispiel wird die `ctaLinkText` der lokalen Variablen in Modul `CTA` auf `Click this button!` aktualisiert.

```http
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

E-Mails folgen dem Standard-Lebenszyklus für die Asset-Genehmigung. Separate Endpunkte ermöglichen es Ihnen, einen Entwurf zu genehmigen, die Genehmigung einer genehmigten Version aufzuheben oder einen vorhandenen Entwurf zu verwerfen.

### Genehmigen

Der Validierungs-Endpunkt validiert die E-Mail anhand der E-Mail-Regeln von Marketo. `from name`, `from email`, `reply to email` und `subject` müssen vor der Genehmigung ausgefüllt werden.

```http
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

Verwenden Sie den `unapprove` nur für genehmigte E-Mails.

```http
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

Sie können nur eine E-Mail mit dem Status Entwurf verwerfen. Genehmigte E-Mails können nicht verworfen werden.

```http
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

```http
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

Um eine E-Mail zu klonen, senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage mit den folgenden Parametern:

- `name`: Erforderlich. Der geklonte E-Mail-Name.
- `folder`: Erforderlich. Ein eingebettetes JSON-Objekt mit `id` und `type`.
- `description`: Optional. Die geklonte E-Mail-Beschreibung.

Wenn die Quell-E-Mail keine genehmigte Version aufweist, klont der Endpunkt die Entwurfsversion.

```http
POST /rest/asset/v1/email/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Um eine Beispiel-E-Mail zu senden, geben Sie im Abfrageparameter `emailAddress` den Empfänger an. Sie können auch die folgenden optionalen Parameter einbeziehen:

- `leadId`: Gibt die Identität eines bestimmten Leads aus der Datenbank an.
- `textOnly`: Sendet nur die Textversion der E-Mail.

```http
POST /rest/asset/v1/email/{id}/sendSample.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Verwenden Sie den Endpunkt [Vollständigen Inhalt einer E](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailFullContentUsingGET)Mail abrufen, um eine Live-Vorschau einer E-Mail abzurufen, wie sie ein Empfänger erhalten würde. Dieser Endpunkt unterstützt nur E-Mails der Version 1.0.

Der erforderliche `id` identifiziert die E-Mail, die in der Vorschau angezeigt werden soll. Der Endpunkt akzeptiert außerdem drei optionale Abfrageparameter:

- `status`: Akzeptiert `draft` oder `approved`. Die Standardversion ist die genehmigte Version für eine genehmigte E-Mail oder die Entwurfsversion für eine nicht genehmigte E-Mail.
- `type`: Akzeptiert `Text` oder `HTML`. Die Standardeinstellung lautet `HTML`.
- `leadId`: Akzeptiert die Ganzzahl-ID eines Leads und zeigt eine Vorschau der E-Mail an, als ob dieser Lead sie erhalten hätte.

```http
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

Verwenden Sie den Endpunkt [Vollständigen Inhalt der E-Mail aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailFullContentUsingPOST), um den gesamten Inhalt eines E-Mail-Assets zu ersetzen. Dieser Endpunkt unterstützt nur E-Mails der Version 1.0, die die Funktion Code bearbeiten in der Benutzeroberfläche verwendet haben und nicht mehr mit ihrer übergeordneten Vorlage verknüpft sind.

Der Endpunkt ist in erster Linie für Assets vorgesehen, die als Teil eines Programms geklont wurden und nicht mit den Standardinhaltsendpunkten geändert werden können. E-Mails mit dynamischen Inhalten werden nicht unterstützt. Wenn die E-Mail weiterhin mit ihrer Vorlage verknüpft ist, gibt der Endpunkt einen Fehler zurück.

Senden Sie eine `multipart/form-data` mit der E-Mail-`id` im Pfad. Der Anfragetext muss einen `content` mit einem vollständigen HTML-E-Mail-Dokument und dem Inhaltstyp `text/html` enthalten.

Ein falsch formatiertes HTML-Dokument erzeugt eine Warnung und kann die Genehmigung verhindern. Das Einschließen von JavaScript- oder `<script>`-Tags führt dazu, dass der Aufruf mit einem Fehler fehlschlägt.

```http
POST /rest/asset/v1/email/{id}/fullContent.json
```

```text
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
