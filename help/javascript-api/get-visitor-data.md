---
title: Besucherdaten abrufen
description: Erhalten Sie eine Echtzeit-Besucheridentifizierung mithilfe der RTP User Context-API mit Parametern, Callback-Beispiel und Beispielantworten für Segmente, ABM und Standort.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 4%

---

# Besucherdaten abrufen

Diese Methode wird verwendet, um Echtzeit-Besucheridentifizierungsdaten abzurufen.

- Sie müssen Web Personalization-Kunde werden und das [RTP-Tag &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

Wenn ein Fehler auftritt, wird als Teil der Antwort-JSON eine Fehlermeldung angezeigt. Wenn ein 500-Code zurückgegeben wird, wenden Sie sich mit der von Ihnen gestellten Anfrage an den Support.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| `get` | Erforderlich | String | Aktion der Methode. |
| `visitor` | Erforderlich | String | Methodenname. |
| `callback` | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst werden soll. |

## Beispiele

Abrufen von Besucheridentifizierungsdaten:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Antwort mit Segment Match:

Nachfolgend finden Sie eine Beispielantwort, die zurückgegeben wird, falls der Besucher vor dem API-Aufruf zum Abrufen von Besucherdaten mit Echtzeit-Segmenten abgeglichen hat.

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

Antwort ohne Segment Match:

Nachfolgend finden Sie eine Beispielantwort, die zurückgegeben wird, falls der Besucher vor dem Aufruf der API zum Abrufen von Besucherdaten mit keinem Echtzeit-Segment übereinstimmte.

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
