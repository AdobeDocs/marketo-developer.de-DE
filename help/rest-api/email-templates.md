---
title: E-Mail-Vorlagen
feature: REST API
description: Erfahren Sie, wie Sie E-Mail-Vorlagen für die Marketo-REST-API erstellen und verwalten, einschließlich HTML-Anforderungen, Abfragen nach ID oder Namen und Durchsuchen von Ordnern
exl-id: 0ecf4da6-eb7e-43c1-8d5c-0517c43b47c8
TQID: https://experienceleague.adobe.com/jKQpibaRP7nAyIsDdjMf8VkNPi5AMFbe7I4Iiy3MGc0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 570
ht-degree: 2%

---

# E-Mail-Vorlagen

[Endpunktreferenz für E-Mail-Vorlage](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates)

Jede neue E-Mail in Marketo basiert zunächst auf einer E-Mail-Vorlage. Obwohl Sie später die Verknüpfung einer E-Mail mit ihrer Vorlage aufheben können, indem Sie die HTML ersetzen, müssen Sie beim Erstellen der E-Mail eine Vorlage auswählen.

Vorlagen sind HTML-Dokumente mit Metadaten wie einem Namen und einer Beschreibung. Die Vorlagen-HTML muss gültig sein und mindestens einen bearbeitbaren Abschnitt enthalten, der die [bearbeitbaren Abschnittsanforderungen“ &#x200B;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-editable-sections-to-email-templates-v1-0).

## Abfrage

E-Mail-Vorlagen unterstützen die standardmäßigen Asset-Abfragemuster: [nach ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/getTemplateByIdUsingGET), [nach Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/getTemplateByNameUsingGET) und durch [Browsen](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/getEmailTemplatesUsingGET) eines Ordners.

### Nach ID

```http
GET /rest/asset/v1/emailTemplate/{id}.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "14f9e#14a12955df1",
    "result": [
        {
            "id": 19,
            "name": "aFgSxuZrBI",
            "description": "fUMhVfIyVkhHzRolYzjGyWouTMfjXCPIAZxHMAEmszAjguVKDtbznEeqbqiDuNBzQoHwBJFdXiMzYiMlGUwtuklUhjGfJlDbhaTL",
            "createdAt": "2014-11-14T02:41:26Z+0000",
            "updatedAt": "2014-11-14T02:41:26Z+0000",
            "folder": {
                "type": "Folder",
                "value": 15
            },
            "status": "Draft",
            "workspace": "Default"
        }
    ]
}
```

#### Nach Name

```http
GET /rest/asset/v1/emailTemplate/byName.json?name=Test Template
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "14f9e#14a12955df1",
    "result": [
        {
            "id": 19,
            "name": "aFgSxuZrBI",
            "description": "fUMhVfIyVkhHzRolYzjGyWouTMfjXCPIAZxHMAEmszAjguVKDtbznEeqbqiDuNBzQoHwBJFdXiMzYiMlGUwtuklUhjGfJlDbhaTL",
            "createdAt": "2014-11-14T02:41:26Z+0000",
            "updatedAt": "2014-11-14T02:41:26Z+0000",
            "folder": {
                "type": "Folder",
                "value": 15
            },
            "status": "Draft",
            "workspace": "Default"
        }
    ]
}
```

#### Durchsuchen

```http
GET /rest/asset/v1/emailTemplates.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":201,
         "name":"AIpwuwiaqb",
         "description":null,
         "createdAt":"2014-11-25T20:49:06Z+0000",
         "updatedAt":"2014-11-25T20:49:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":240,
         "name":"aqZGoAskEF",
         "description":"uOMEhLpXOEWkwdZxkpcdDjTjKfokxuHEYHPVIVsADFIUEUobzIEaDiqFFxezwfovGfwjuPTJRxUmuHmGpyIklJdDdVosPJdyOVom",
         "createdAt":"2014-11-26T21:11:56Z+0000",
         "updatedAt":"2014-11-26T21:11:56Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":199,
         "name":"BAxnkVfLGi",
         "description":"TzUMQKzKXdgukNCCcaiJHUWASceqlZswhCqDQFDFZULqzYkEiyKcwtQRzKERynReqtMHOhqjnhExCsZopyfzglmXAOjEJdxNURCX",
         "createdAt":"2014-11-25T20:49:06Z+0000",
         "updatedAt":"2014-11-25T20:49:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":278,
         "name":"bcBNCUIHrL",
         "description":"UJEPYBRGTSYosZRnMnahMyVtdyxjRpzJMSXyncATKwcLlDAqDnSCFezGVsDZFpZwPzQvBlvaOZzOzBIsIAtqIerZhJFfpqMogoiB",
         "createdAt":"2014-11-30T11:30:07Z+0000",
         "updatedAt":"2014-11-30T11:30:07Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

Vorlagenabfragen geben nur Datensatz-Metadaten zurück. Verwenden Sie den Content-Endpunkt zum Abrufen des Vorlageninhalts.

## Erstellen und aktualisieren

Um [&#x200B; Vorlage zu erstellen](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/createEmailTemplateUsingPOST) oder zu [aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/updateEmailTemplateContentUsingPOST) senden Sie das HTML-Dokument in einer `multipart/form-data` POST-Anfrage. Der `Content-Type` muss eine Begrenzung enthalten, wie in den RFCs für [multipart](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html) und [multipart/form-data](https://www.ietf.org/rfc/rfc2388.txt) beschrieben.

Das Erstellen einer Vorlage erfordert die folgenden Parameter:

- `name`: Der Name der Vorlage.
- `folder`: Der übergeordnete Ordner.
- `content`: Das HTML-Dokument. Die `Content-Disposition` Kopfzeile muss den konventionellen `filename` enthalten.

Sie können auch einen optionalen `description`-Parameter hinzufügen.

```http
POST /rest/asset/v1/emailTemplates.json
```

```text
Content-Type: multipart/form-data; boundary=mktoBoundary1480963323998
```

```html
--mktoBoundary1480963323998
Content-Disposition: form-data; name="name"

Sample Email Template
--mktoBoundary1480963323998
Content-Disposition: form-data; name="folder"

{"id":15,"type":"Folder"}
--mktoBoundary1480963323998
Content-Disposition: form-data; name="content"; filename="testHTML.html"
Content-Type: text/html

<html>
<body>
<h1>TEST HTML</h1>
</body>
</html>

--mktoBoundary1480963323998
Content-Disposition: form-data; name="description"

Create email template using API
--mktoBoundary1480963323998--
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "a99f#14e22b2b85e",
    "result": [
        {
            "id": 1022,
            "name": "Sample Email Template",
            "description": "Create email template using API",
            "createdAt": "2015-06-23T23:13:34Z+0000",
            "updatedAt": "2015-06-23T23:13:34Z+0000",
            "url": "https://app-abm.marketo.com/#ET1022B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 15,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default",
            "version": 1
        }
    ]
}
```

Um den Vorlageninhalt zu aktualisieren, rufen Sie den [Inhaltsendpunkt“ mit &#x200B;](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/updateEmailTemplateContentUsingPOST) E-Mail-Vorlagen-ID auf. Der Anfragetext akzeptiert nur den `content`.

Der gesendete Inhalt ersetzt vollständig den vorhandenen Vorlageninhalt. Beim Aktualisieren einer genehmigten Version wird ein neuer Entwurf erstellt. Wenn ein Asset nur als Entwurf aktualisiert wird, wird der aktuelle Entwurf ersetzt.

```http
POST /rest/asset/v1/emailTemplate/{id}/content.json
```

```text
Content-Type: multipart/form-data; boundary=mktoBoundaryEiJFFFPFKK2WovsT
```

```html
--mktoBoundaryEiJFFFPFKK2WovsT
Content-Disposition: form-data; name="content"; filename="testHTML2.html"
Content-Type: text/html

<html>
<body>
<h1>TEST HTML WITH UPDATE</h1>
<div class="mktEditable"></div>
</body>
</html>
--mktoBoundaryEiJFFFPFKK2WovsT--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "f8e2#158d0ae24f8",
   "result":[
      {
         "id":1022,
         "status":"Draft",
         "content":"<html>\n<body>\n<h1>TEST HTML WITH UPDATE</h1>\n<div class="mktEditable"></div>\n</body>\n</html>"
      }
   ]
}
```

## Aktualisieren von Metadaten

Um [die Metadaten einer Vorlage zu aktualisieren](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/updateEmailTemplateUsingPOST) senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage mit den Parametern `name` und `description`.

```http
POST /rest/asset/v1/emailTemplate/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=Updated description&name=New Name
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "17ca5#14a12ab900a",
    "result": [
        {
            "id": 19,
            "name": "New Name",
            "description": "Updated description",
            "createdAt": "2014-11-14T02:41:26Z+0000",
            "updatedAt": "2014-11-14T02:41:26Z+0000",
            "folder": {
                "type": "Folder",
                "value": 15
            },
            "status": "Draft",
            "workspace": "Default"
        }
    ]
}
```

## Genehmigung

E-Mail-Vorlagen folgen dem Standard-Lebenszyklus für die Asset-Genehmigung. Separate Endpunkte ermöglichen es Ihnen, einen Entwurf zu genehmigen, die Genehmigung einer genehmigten Version aufzuheben oder einen vorhandenen Entwurf zu verwerfen.

### Genehmigen

Der Validierungsendpunkt validiert die Vorlage anhand der Regeln für Marketo-E-Mails. Vor der Genehmigung müssen Absendername, Absender-E-Mail, Antwort-E-Mail und Betreff ausgefüllt werden.

```http
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

### Genehmigung aufheben

Verwenden Sie den Endpunkt Genehmigung aufheben nur für eine genehmigte Vorlage.

```http
POST /rest/asset/v1/emailTemplate/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
"description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

### Verwerfen

Beim Aktualisieren einer genehmigten Vorlage wird eine Entwurfsversion erstellt. Verwenden Sie den Endpunkt „Verwerfen“, um diesen Entwurf zu verwerfen.

```http
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

### Löschen

```http
POST /rest/asset/v1/emailTemplate/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15cef#149d3de83db",
   "result":[
      {
         "id":12
      }
   ]
}
```

## Klonen

Um [E-Mail-Vorlage klonen](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/cloneTemplateUsingPOST) senden Sie eine `application/x-www-form-urlencoded` POST-Anfrage mit den folgenden Parametern:

- `name`: Erforderlich. Der geklonte Vorlagenname.
- `folder`: Erforderlich. Ein eingebettetes JSON-Objekt mit `id` und `type`.
- `description`: Optional. Die geklonte Vorlagenbeschreibung.

```http
POST /rest/asset/v1/emailTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Sample Template 01 - deverly&folder={"id":12,"type":"Folder"}&description=This is a sample template
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "905e#14e22c693f8",
    "result": [
        {
            "id": 1024,
            "name": "Sample Template 01 - deverly",
            "description": "This is a sample template",
            "createdAt": "2015-06-23T23:35:16Z+0000",
            "updatedAt": "2015-06-23T23:35:16Z+0000",
            "url": "https://app-abm.marketo.com/#ET1024B2ZN12",
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

## E-Mail-Abhängigkeiten abfragen

Verwenden Sie den Endpunkt [E-Mail-Vorlage abrufen von](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates/operation/getEmailTemplateUsedByUsingGET) um E-Mails abzurufen, die von einer Vorlage abhängen. Der `id` Pfadparameter identifiziert die übergeordnete E-Mail-Vorlage.

Der Endpunkt unterstützt zwei optionale Paginierungsparameter:

- `maxReturn`: Begrenzt die Anzahl der Ergebnisse. Der Standardwert ist 20, der Maximalwert 200.
- `offset`: Funktioniert mit `maxReturn`, um durch große Ergebnismengen zu blättern. Der Standardwert ist 0.

```http
GET /rest/asset/v1/emailTemplates/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "18b0#16fa3344169",
    "warnings": [],
    "result": [
        {
            "id": 1022,
            "name": "EmailPr.Email2",
            "type": "Email",
            "status": "approved",
            "updatedAt": "2019-12-02T00:42:21Z+0000"
        },
        {
            "id": 1023,
            "name": "Default.Email1.email1",
            "type": "Email",
            "status": "approved",
            "updatedAt": "2019-12-02T00:42:21Z+0000"
        },
        {
            "id": 1025,
            "name": "Defa.E1",
            "type": "Email",
            "status": "draft",
            "updatedAt": "2019-12-02T00:42:21Z+0000"
        },
        {
            "id": 1052,
            "name": "Email v1 Program.Email v1 Email",
            "type": "Email",
            "status": "draft",
            "updatedAt": "2019-06-07T20:07:16Z+0000"
        }
    ]
}
```
