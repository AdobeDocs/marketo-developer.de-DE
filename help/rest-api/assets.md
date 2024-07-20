---
title: Assets
feature: REST API
description: Eine API für die Arbeit mit Marketo-Assets.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---

# Assets

Marketo bietet APIs zur Interaktion mit den meisten Marketing- und Organisations-Assets in Marketo.

## Assets

Zu den Marketo-Assets gehören:

- Ordner
- Programme
- E-Mails
- E-Mail-Vorlagen
- Landing Page
- Landing Page-Vorlagen
- Ausschnitte
- Formulare
- Token
- Dateien

## API

Eine vollständige Liste der Asset-API-Endpunkte, einschließlich Parametern und Modellierungsinformationen, finden Sie in der [Asset-API-Endpunktreferenz](endpoint-reference.md).

## Anfrage

Assets weist in der Regel drei Muster auf, mit denen sie abgerufen werden können: nach ID, nach Name und durch Durchsuchen.  Nach ID und Name rufen sowohl ein einzelnes Asset für einen bestimmten Parameter ab, während das Durchsuchen zurückgibt und das Durchsuchen der gesamten Liste von Assets dieses Typs ermöglicht.  Einzelne Asset-Typen verfügen über verschiedene Parameter, anhand derer sie gefiltert werden können. Achten Sie daher darauf, sich die einzelnen Dokumente für bestimmte Details anzusehen.

In bestimmten Fällen gibt der Durchsuchen-Endpunkt für einige Asset-Typen keine untergeordneten Assets zurück, z. B. die zulässigen Werte für ein Tag, und sie müssen einzeln über den Endpunkt Nach Name oder Nach ID abgerufen werden, um den vollständigen Metadatensatz zurückzugeben.  Andere können separate Endpunkte zum vollständigen Abrufen von abhängigen Objekten wie Formularfeldern haben.

### Nach ID

```
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

### Nach Name

Aus technischen Gründen können die Asset-APIs nicht nach Asset-Namen suchen, die Kommas (,) enthalten.  Es wird empfohlen, dass Ihre Namenskonvention Kommas für alle Asset-Typen ausschließt.

```
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Durchsuchen

Beim Durchsuchen von Assets sind immer zwei Abfrageparameter zulässig:

- offset - Ein ganzzahliger Versatz, aus dem Ergebnisse zurückgegeben werden.
- maxReturn - Beschränkt die Anzahl der zurückgegebenen Datensätze.  Die Standardeinstellung ist 20, wenn nicht festgelegt, und hat eine maximale Größe von 200.

```
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
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
      }
   ]
}
```

## Erstellen und Aktualisieren

Bei einfachen Asset-Typen wie Ordnern, Token und Dateien gibt es in der Regel nur einen einzelnen Endpunkt zur Erstellung und dann einen zusätzlichen Endpunkt zum Aktualisieren von Datensätzen nach ID.  Assets wird mit einem Namen erstellt, der immer erforderlich ist. Anschließend werden alle Metadaten und IDs von der Erstellungs- oder Aktualisierungsantwort zurückgegeben.

So erstellen Sie beispielsweise ein Token:

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

Gehen Sie wie folgt vor, um einen Ordner zu aktualisieren:

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

Andere Assets verfügen über komplexere Strukturen und erfordern Aktualisierungen an zusätzlichen Unterabschnitten oder untergeordneten Objekten. Anschließend müssen sie einer Genehmigung unterzogen werden, bevor sie verwendet werden können.  Zu diesen Asset-Typen gehören Forms, E-Mails, E-Mail-Vorlagen, Landingpages und Landingpage-Vorlagen.  Diese enthalten jeweils einen einzelnen Endpunkt zum Erstellen eines Datensatzes und dann zusätzliche Endpunkte zum Aktualisieren von Metadaten-, Inhalts- und Inhaltsabschnitten.

Um beispielsweise eine Landingpage zu erstellen, müssen Sie ihren Erstellungsendpunkt mit einer Vorlagen-ID aufrufen und dann die zugehörigen Inhaltsabschnitte abrufen und jeden einzelnen Abschnitt aktualisieren, um Inhalte hinzuzufügen, bevor Sie sie genehmigen, damit sie live bereitgestellt werden kann.

### Komplexe Erstellung

Für Landingpages muss zunächst ein Landingpage-Asset mit einer übergeordneten Vorlage erstellt werden.  Dadurch wird eine neue Landingpage erstellt, die den Standardinhalt der Vorlage für jeden Inhaltsabschnitt enthält.

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

#### Abrufen von Abschnitten

Um den Inhalt einer Landingpage auszufüllen, müssen Sie die Liste der Inhaltsabschnitte abrufen und dann einzelne Aktualisierungen für jeden Abschnitt durchführen, der von der Vorlage abweicht.

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

#### Abschnitt aktualisieren

```
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Genehmigung

Viele Asset-Typen verfügen über ein verknüpftes Entwurfs- und Genehmigungssystem, einschließlich E-Mails, Landingpages, Snippets, Forms und den zugehörigen Vorlagen.  Wenn Sie versuchen, ein Asset zu genehmigen, wird es anhand eines bestimmten Satzes von Validierungsregeln ausgewertet und entweder auf den Status &quot;Genehmigt&quot;gesetzt oder ein Fehlergrund zurückgegeben.  Bei diesen Asset-Typen werden bei jeder Aktualisierung des Inhalts eines bestimmten Assets die Änderungen an einem Entwurf des Assets vorgenommen, was sich nicht auf die genehmigte Version auswirkt.  Dadurch können Änderungen an Inhalten sicher vorgenommen werden, ohne dass sich dies auf Live-Versionen des Assets auswirkt.  Die Änderungen können dann mithilfe des Validierungsendpunkts auf die Live-Version angewendet werden.  Dadurch wird auch der Entwurfsstatus des Assets gelöscht, bis zusätzliche Aktualisierungen angewendet werden.

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

Die erfolgreiche Genehmigung ersetzt die vorherige Live-Version durch die aktualisierte Version.

Entwürfe werden auch über einen Endpunkt für jeden gültigen Asset-Typ verworfen.  Wenn Sie dies für ein Asset verwenden, das im Entwurfsstatus genehmigt wurde, werden der aktuelle Entwurf und alle ausstehenden Änderungen verworfen.  Wenn Sie dies für ein Asset verwenden, für das derzeit keine genehmigte Version vorliegt, wird nichts bewirkt und ein Fehler zurückgegeben.  Nur-Entwurf-Assets können gelöscht werden, sie können jedoch nicht verworfen werden.

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

Assets kann auch nicht genehmigt werden, wenn sie sich in einem Nur-Genehmigen-Status befinden.  Dadurch werden alle Live-Versionen des Assets heruntergefahren und das Asset in einen Nur-Entwurf-Status zurückgegeben, während auch alle zugehörigen Entwürfe verworfen werden.  Diese Aktion kann nur für die meisten Assets ausgeführt werden, wenn sie nirgends in Marketo verwendet wird, z. B. wenn eine E-Mail in einem Schritt zum Senden einer E-Mail referenziert wird oder ein in eine E-Mail eingebetteter Snippet verwendet wird.

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

## Löschen

Assets mit Genehmigungs- und Entwurfsstatus, mit Ausnahme von Formularen, darf bei der Genehmigung nicht gelöscht werden und muss vor dem Löschen nicht genehmigt werden.  Löschungen können im Allgemeinen nur durchgeführt werden, wenn ein Asset nicht genehmigt wurde und nicht verwendet wird, und im Fall von Ordnern, die von Assets leer sind.  Eine wichtige Ausnahme sind Programme, die zusammen mit allen untergeordneten Inhalten gelöscht werden können, sofern das Programm und sein Inhalt an keiner Stelle außerhalb der Grenzen des Programms verwendet werden.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Timeouts

Asset-APIs haben eine Zeitüberschreitung von 300 Sekunden
