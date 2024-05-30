---
title: "Tokens"
feature: REST API, Tokens
description: "Verwalten von Token in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---


# Token

[Token-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Token in Marketo sind spezielle Zeichenfolgen, die Kurzwahlnummern ähneln, die zur Laufzeit durch ein separates Datenelement ersetzt werden. In Marketo stehen verschiedene Token zur Verfügung, jedoch können nur My Tokens über die API bearbeitet werden. Meine Token sind untergeordnete Token, die für einen bestimmten Ordner oder ein bestimmtes Programm lokal verfügbar sind. Token können über die API gelesen, erstellt und gelöscht werden.

## Datentyp

Token können mit den folgenden Datentypen erstellt werden:

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert des Formulars &quot;yyyy-MM-dd&quot; |
| number | Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| sfdc campaign | Wird in der Salesforce-Kampagnenverwaltungsintegration verwendet |
| Text | Eine Textzeichenfolge |


Dies sind die einzigen Datentypen, die beim Erstellen eines Tokens über API verwendet werden können.

## Anfrage

[Abrufen von Token nach Ordner-ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) nimmt eine `id` als Pfadparameter entweder vom Typ Programm oder Ordner . Dieser Typ wird durch die Variable `folderType` -Parameter.

```curl
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

## Erstellen und Aktualisieren

Die [Token erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) -Endpunkt erstellt Token, oder wenn sie vorhanden sind, aktualisieren Sie sie mit gesendeten Werten. Token werden im Kontext eines Ordners oder Programms erstellt. Die erforderlichen `id` path parameter ist die ID des Ordners, mit dem das Token verknüpft wird. Die `name`, `type`, `value`, und `folderType` sind alle erforderlichen Parameter des Tokens. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON. Die `name` -Feld des Tokens darf nicht länger als 50 Zeichen sein.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Token nach Namen löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) akzeptiert eine ID als Pfadparameter, entweder vom Typ Programm oder Ordner . Dieser Typ wird durch die Variable `folderType` -Parameter. Token werden basierend auf ihrem übergeordneten Ordner, dem `name`und die `type` des Tokens, von denen jede erforderlich ist. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
