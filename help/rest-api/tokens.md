---
title: Token
feature: REST API, Tokens
description: Meine Marketo-Token mit der Asset-REST-API verwalten Siehe Unterstützte Datentypen, Nach Ordner oder Programm abrufen, Erstellen oder Aktualisieren über einen formularkodierten POST und Löschen nach Namen.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 4%

---

# Token

[Token-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

Token in Marketo sind spezielle Zeichenfolgen, die Shortcodes ähneln und zur Laufzeit durch separate Daten ersetzt werden. In Marketo gibt es verschiedene Arten von Token, aber nur „Meine Token“ können über die API bearbeitet werden. Meine Token sind untergeordnete Token, die sich in einem bestimmten Ordner oder Programm befinden. Token können über die API gelesen, erstellt und gelöscht werden.

## Datentyp

Token können mit den folgenden Datentypen erstellt werden:

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

Dies sind die einzigen Datentypen, die beim Erstellen eines Tokens über die API verwendet werden können.

## Abfrage

[Token nach Ordner-ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) akzeptiert eine `id` als Pfadparameter entweder vom Typ „Programm“ oder „Ordner“. Dieser Typ wird durch den `folderType` angegeben.

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

## Erstellen und aktualisieren

Der Endpunkt [Token erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) erstellt Token oder aktualisiert sie, falls vorhanden, mit gesendeten Werten. Token werden im Kontext eines Ordners oder Programms erstellt. Der erforderliche `id` ist die ID des Ordners, mit dem das Token verknüpft werden soll. `name`, `type`, `value` und `folderType` sind alle erforderlichen Parameter des Tokens. Die Daten werden als POST x-www-form-urlencoded und nicht als JSON übergeben. Das `name` Feld des Tokens darf 50 Zeichen nicht überschreiten.

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

[Token nach Namen löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) akzeptiert eine ID als Pfadparameter entweder vom Typ „Programm“ oder „Ordner“. Dieser Typ wird durch den `folderType` angegeben. Token werden je nach übergeordnetem Ordner, `name` und `type` des Tokens gelöscht. Diese sind jeweils erforderlich. Die Daten werden als POST x-www-form-urlencoded und nicht als JSON übergeben.

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
