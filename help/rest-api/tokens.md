---
title: Token
feature: REST API, Tokens
description: Verwalten von Token in Marketo.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 4%

---

# Token

[Token-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

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

[Token nach Ordner-ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) akzeptiert eine `id` als Pfadparameter entweder vom Typ Programm oder Ordner . Dieser Typ wird durch den Parameter `folderType` angegeben.

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

Der Endpunkt [Token erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) erstellt Token oder, falls vorhanden, aktualisieren diese mit den gesendeten Werten. Token werden im Kontext eines Ordners oder Programms erstellt. Der erforderliche Pfadparameter `id` ist die ID des Ordners, mit dem das Token verknüpft wird. Die Parameter `name`, `type`, `value` und `folderType` sind alle erforderlichen Parameter des Tokens. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON. Das Feld `name` des Tokens darf 50 Zeichen nicht überschreiten.

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

[Löschen-Token nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) verwendet eine ID als Pfadparameter entweder vom Typ Programm oder Ordner . Dieser Typ wird durch den Parameter `folderType` angegeben. Token werden basierend auf ihrem übergeordneten Ordner, den `name` und den `type` des Tokens gelöscht, von denen jeder erforderlich ist. Daten werden als POST x-www-form-urlencoded übergeben, nicht als JSON.

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
