---
title: E-Mail-Vorlagen
feature: REST API
description: Erstellen Sie E-Mail-Vorlagen mit Marketo-APIs.
exl-id: 0ecf4da6-eb7e-43c1-8d5c-0517c43b47c8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 2%

---

# E-Mail-Vorlagen

[E-Mail-Vorlagenendpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates)

E-Mail-Vorlagen bilden die Grundlage für jede neue E-Mail in Marketo.  E-Mails können zwar durch HTML-Austausch von Vorlagen abgekoppelt werden, doch müssen E-Mails zunächst mit einer Vorlage als Grundlage erstellt werden.  Vorlagen werden in Marketo als reine HTML-Dokumente mit Metadaten wie Namen und Beschreibungen erstellt.  Es gibt nur wenige Inhaltseinschränkungen, aber die HTML der Vorlage muss gültig sein und mindestens einen bearbeitbaren Abschnitt enthalten, der den hier beschriebenen [Anforderungen entspricht.](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-editable-sections-to-email-templates-v1-0)

## Anfrage

Beim Abfragen von E-Mail-Vorlagen wird das Standardmuster für Assets befolgt, sodass Abfragen [von id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/getTemplateByIdUsingGET), [von name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/getTemplateByNameUsingGET) und [browsing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/getEmailTemplatesUsingGET) von einem bestimmten Ordner ausgeführt werden.

### Nach ID

```
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

```
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

```
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

Beim Abfragen des Datensatzes selbst werden nur Metadaten zum Datensatz zurückgegeben. Informationen zum Abrufen von Inhalten finden Sie im Abschnitt #content .

## Erstellen und Aktualisieren

[Erstellen von](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/createEmailTemplateUsingPOST) oder [Aktualisieren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/updateEmailTemplateContentUsingPOST) einer Vorlage ist ziemlich einfach. Der Inhalt jeder Vorlage wird als HTML-Dokument gespeichert und muss mithilfe eines mehrteiligen Formulardaten vom Typ POST an Marketo übergeben werden. Sie müssen die entsprechende Content-Type-Kopfzeile übergeben, die eine Begrenzung enthält, wie in den RFCs für [multipart](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html) und [multipart/form-data](https://www.ietf.org/rfc/rfc2388.txt) beschrieben.

Zum Erstellen einer Vorlage müssen Sie drei Parameter angeben: Name, Ordner, Inhalt. Ein optionaler Beschreibungsparameter kann enthalten sein.  Das HTML-Dokument wird im Inhaltsparameter übergeben, der auch den herkömmlichen Dateinamenparameter als Teil der Content-Disposition-Kopfzeile enthalten muss.

```
POST /rest/asset/v1/emailTemplates.json
```

```
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

Die Aktualisierung des Inhalts erfolgt mithilfe eines [separaten Endpunkts](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/updateEmailTemplateContentUsingPOST), für den die ID der E-Mail-Vorlage erforderlich ist. Dieser Endpunkt ermöglicht nur die Übermittlung des Inhaltsparameters im Text. Wenn eine Aktualisierung vorgenommen wird, ersetzt der im Inhaltsparameter übergebene Inhalt den vorhandenen Inhalt der E-Mail in einem neuen Entwurf, wenn eine genehmigte Version aktualisiert wird, oder ersetzt den aktuellen Entwurf, wenn das Asset nur als Entwurf vorliegt.

```
POST /rest/asset/v1/emailTemplate/{id}/content.json
```

```
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

Um [die Metadaten, Namen und Beschreibungen einer Vorlage zu aktualisieren, können Sie denselben Endpunkt wie für die Aktualisierung von Inhalten verwenden, jedoch stattdessen eine application/x-www-url-formencoded -POST mit den Namen- und Beschreibungsparametern übergeben.](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/updateEmailTemplateUsingPOST)

```
POST /rest/asset/v1/emailTemplate/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

E-Mail-Vorlagen folgen dem Standardmuster für die Validierung von Asset-Datensätzen. Sie können einen Entwurf genehmigen, die Genehmigung einer genehmigten Version aufheben und einen vorhandenen Entwurf einer E-Mail-Vorlage über jeden ihrer Endpunkte verwerfen.

### Genehmigen

Beim Aufrufen des Validierungsendpunkts wird die E-Mail anhand der Regeln für Marketo-E-Mails validiert. Der Name des Formulars, die E-Mail-Adresse, die Antwort auf die E-Mail und der Betreff müssen ausgefüllt werden, bevor die E-Mail genehmigt werden kann.

```
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

Der Endpunkt &quot;Nicht genehmigen&quot;kann nur für genehmigte Vorlagen verwendet werden.

```
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

Die Entwurfsversion der Vorlage wird erstellt, nachdem eine genehmigte E-Mail aktualisiert wurde.

```
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

```
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

Marketo bietet eine einfache Methode zum [Klonen einer E-Mail-Vorlage](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/cloneTemplateUsingPOST). Anders als beim Erstellen erfolgt diese Art von Anforderung mit einer POST application/x-www-url-formencoded . Sie verwendet zwei erforderliche Parameter, den Namen und den Ordner, ein eingebettetes JSON-Objekt mit ID und Typ.  Beschreibung ist auch ein optionaler Parameter.

```
POST /rest/asset/v1/emailTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Verwenden Sie den Endpunkt [Von ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates/operation/getEmailTemplateUsedByUsingGET) verwendete E-Mail-Vorlage abrufen , um eine Liste der E-Mails abzurufen, die von einer bestimmten E-Mail-Vorlage abhängen.  Der Pfadparameter `id` gibt die übergeordnete E-Mail-Vorlage an.

Es gibt 2 optionale Parameter. `maxReturn`  ist eine Ganzzahl, die die Anzahl der Ergebnisse begrenzt (standardmäßig 20, maximal 200), und `offset` ist eine Ganzzahl, die mit `maxReturn` verwendet werden kann, um durch große Ergebnismengen zu lesen (der Standardwert lautet 0).

```
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
