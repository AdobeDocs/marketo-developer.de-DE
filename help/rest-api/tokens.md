---
title: Token
feature: REST API, Tokens
description: Meine Marketo-Token mit der Asset-REST-API verwalten Siehe Unterstützte Datentypen, Nach Ordner oder Programm abrufen, Erstellen oder Aktualisieren über einen formularkodierten POST und Löschen nach Namen.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
TQID: https://experienceleague.adobe.com/uqOpu2vDuiQiZhILKuxZJQGadd0K14zwIaAdmNfK1-I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 290
ht-degree: 4%

---

# Token

[Token-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

Token sind Zeichenfolgen, die Marketo zur Laufzeit durch andere Daten ersetzt. Die API kann nur meine Token bearbeiten, die untergeordnete Token sind, die lokal in einem Ordner oder Programm sind.

Verwenden Sie die Token-API, um „Meine Token“ zu lesen, zu erstellen, zu aktualisieren und zu löschen.

## Datentyp

Token können mit den folgenden Datentypen erstellt werden:

| Typ | Beschreibung |
| --- | --- |
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

Die API unterstützt beim Erstellen eines Tokens nur diese Datentypen.

## Abfrage

[Token nach Ordner-ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/getTokensByFolderIdUsingGET) nimmt die ID eines Programms oder Ordners als Pfadparameter. Geben Sie den Typ mithilfe des `folderType`-Parameters an.

```http
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Erstellen und aktualisieren

Der Endpunkt [Token erstellen](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/addTokenTOFolderUsingPOST) erstellt ein Token oder aktualisiert ein vorhandenes Token mit den gesendeten Werten. Token gehören zu einem Ordner oder Programm.

Der `id` Pfadparameter identifiziert den übergeordneten Ordner. Die Parameter `name`, `type`, `value` und `folderType` sind erforderlich. Übergeben Sie die Daten als POST-`x-www-form-urlencoded`, nicht als JSON. Der Token-`name` darf nicht länger als 50 Zeichen sein.

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=April Fools&type=date&value=2015-04-01&folderType=Folder
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

## Löschen

[Token nach Namen löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/deleteTokenByNameUsingPOST) nimmt die ID eines Programms oder Ordners als Pfadparameter. Geben Sie den Typ mithilfe von `folderType` an.

Der übergeordnete Ordner, die Token-`name` und die Token-`type` sind erforderlich. Übergeben Sie die Daten als POST-`x-www-form-urlencoded`, nicht als JSON.

```http
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
