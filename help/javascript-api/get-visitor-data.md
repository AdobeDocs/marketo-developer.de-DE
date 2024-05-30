---
title: "Abrufen von Besucherdaten"
description: "Abrufen von Besucherdaten"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 5%

---


# Abrufen von Besucherdaten

Diese Methode wird zum Abrufen von Echtzeit-Besucheridentifizierungsdaten verwendet.

- Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der User Context-API.
- RTP unterstützt keine Konto-basierten Marketinglisten mit Namen. ABM-Listen und -Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die innerhalb von RTP verwaltet werden.

Tritt ein Fehler auf, wird im Antwort-JSON eine Fehlermeldung angezeigt. Wenn ein 500-Code zurückgegeben wird, wenden Sie sich mit der von Ihnen gestellten Anfrage an den Support.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| `get` | Erforderlich | Zeichenfolge | Methodenaktion |
| `visitor` | Erforderlich | Zeichenfolge | Name der Methode. |
| `callback` | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst wird. |

## Beispiele

Abrufen von Besucheridentifizierungsdaten:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Antwort mit Segmentübereinstimmung:

Nachstehend finden Sie eine Beispielantwort, die zurückgegeben wird, falls der Besucher vor dem Aufruf der Get Visitor Data API Echtzeit-Segmente zugeordnet hat.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Antwort ohne Segmentübereinstimmung:

Nachstehend finden Sie eine Beispielantwort, die zurückgegeben wird, falls der Besucher vor dem Aufruf der Get Visitor Data API keine Echtzeitsegmente gefunden hat.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
