---
title: Assets
feature: REST API
description: Übersicht über Marketo Asset REST-APIs zum Abfragen nach ID oder Namen, zum Durchsuchen mit Paging und zum Erstellen oder Aktualisieren von Ordnern, E-Mails, Formularen, Vorlagen, Dateien oder Token.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
TQID: https://experienceleague.adobe.com/gRhXvFtG1FHtGJ4tFQxOyGMkEiOX0K1S0VpjcB6s6xM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 3%

---

# Assets

Verwenden Sie die Marketo Asset REST-APIs, um Marketing- und Organisations-Assets abzufragen und zu verwalten.

## Assets

Zu den Marketo-Assets gehören:

- Ordner
- Programme
- E-Mails
- E-Mail-Vorlagen
- Fragmente
- Landingpages
- Landingpage-Vorlagen
- Ausschnitte
- Formulare
- Token
- Dateien

## API

Eine vollständige Liste der Asset-API-Endpunkte, einschließlich Parameter und Modellierungsinformationen, finden Sie in der [Asset-API-Endpunktreferenz](endpoint-reference.md).

## Abfrage

Asset-APIs unterstützen in der Regel drei Abrufmuster: nach ID, nach Name und nach Browser. Abfragen nach ID oder Name rufen ein Asset für den angegebenen Parameter ab. Durchsuchen-Endpunkte geben eine paginierte Liste von Assets dieses Typs zurück.

Filterparameter variieren je nach Asset-Typ. In der Dokumentation zu den einzelnen Asset-Typen finden Sie die unterstützten Filter.

Einige Durchsuchen-Endpunkte geben keine untergeordneten Assets zurück, z. B. die zulässigen Werte für ein -Tag. Rufen Sie diese Assets einzeln nach Name oder ID ab, um ihre vollständigen Metadaten zu erhalten. Andere Asset-Typen bieten separate Endpunkte für abhängige Objekte, z. B. Formularfelder.

### Nach ID

```http
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

Asset-APIs können nicht nach Asset-Namen suchen, die Kommas enthalten. Schließen Sie Kommas aus Asset-Namen aus.

```http
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

Asset-Browser-Endpunkte unterstützen diese Abfrageparameter:

- `offset` - Ein ganzzahliger Versatz, bei dem mit der Rückgabe von Ergebnissen begonnen werden soll.
- `maxReturn` : Die maximale Anzahl an zurückzugebenden Datensätzen. Der Standardwert ist 20, der Maximalwert 200.

```http
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

## Erstellen und aktualisieren

Einfache Asset-Typen wie Ordner, Token und Dateien stellen normalerweise einen Endpunkt für die Erstellung und einen anderen für Aktualisierungen nach ID bereit. Beim Erstellen eines Assets ist ein Name erforderlich. Die Antwort zum Erstellen oder Aktualisieren gibt die Asset-Metadaten und die ID zurück.

Die folgende Anfrage erstellt ein Token:

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Die folgende Anfrage aktualisiert einen Ordner:

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
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

Forms, E-Mails, E-Mail-Vorlagen, Landingpages und Landingpage-Vorlagen weisen komplexere Strukturen auf. Jeder Typ bietet einen Endpunkt zum Erstellen des Assets und zusätzliche Endpunkte zum Aktualisieren der Metadaten-, Inhalts- und Inhaltsabschnitte.

Diese Assets müssen vor der Verwendung genehmigt werden. Erstellen Sie beispielsweise eine Landingpage mit einer Vorlagen-ID, rufen Sie deren Inhaltsabschnitte ab, aktualisieren Sie jeden erforderlichen Abschnitt und genehmigen Sie dann die Seite für die Bereitstellung.

### Komplexes Erstellen

Erstellen Sie eine Landingpage aus einer übergeordneten Vorlage. Die neue Landingpage enthält den Standardinhalt der Vorlage für jeden Abschnitt.

```http
POST rest/asset/v1/landingPages.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

#### Abschnitte abrufen

Rufen Sie die Inhaltsabschnitte der Landingpage ab. Aktualisieren Sie jeden Abschnitt, der sich von der Vorlage unterscheiden muss.

```http
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

```http
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

E-Mails, Landingpages, Snippets, Formulare und ihre Vorlagen verwenden ein Entwurfs- und Genehmigungssystem. Inhaltsaktualisierungen ändern den Entwurf, ohne die genehmigte Live-Version zu beeinflussen.

Der Genehmigungsendpunkt validiert den Entwurf. Wenn die Validierung erfolgreich ist, ersetzt der Entwurf die Live-Version und der Entwurfsstatus wird gelöscht. Wenn die Validierung fehlschlägt, gibt der Endpunkt den Grund zurück.

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

Die erfolgreiche Genehmigung ersetzt die vorherige Live-Version durch die aktualisierte Version.

Jeder unterstützte Asset-Typ bietet einen Endpunkt zum Verwerfen von Entwürfen. Bei bestätigten Assets mit einem Entwurf verwirft dieser Endpunkt den Entwurf und die ausstehenden Änderungen.

Der Endpunkt gibt einen Fehler zurück, wenn das Asset keine genehmigte Version hat. Sie können ein Asset, das nur Entwurf enthält, löschen, seinen Entwurf jedoch nicht verwerfen.

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

Die Genehmigung für ein Asset mit dem Status „Nur genehmigt“ kann aufgehoben werden. Bei der Aufhebung der Genehmigung wird die Live-Version entfernt, das Asset in einen Nur-Entwurf-Status zurückgesetzt und alle zugehörigen Entwürfe werden verworfen.

Bei den meisten Asset-Typen darf das Asset nicht verwendet werden. Sie können beispielsweise die Genehmigung für eine E-Mail, auf die im Schritt E-Mail senden verwiesen wird, oder für einen in eine E-Mail eingebetteten Ausschnitt nicht aufheben.

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

## Löschen

Mit Ausnahme von Formularen müssen Assets mit dem Status Genehmigung und Entwurf vor dem Löschen nicht genehmigt werden. Ein Asset muss im Allgemeinen ebenfalls nicht verwendet werden. Ein Ordner muss leer sein.

Programme bilden eine Ausnahme. Sie können ein Programm und seinen untergeordneten Inhalt löschen, wenn weder das Programm noch sein Inhalt außerhalb des Programms verwendet wird.

```http
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

## Zeitüberschreitungen

Bei Asset-APIs beträgt die Zeitüberschreitung 300 Sekunden.
