---
title: Besucherdaten abrufen
description: Erhalten Sie eine Echtzeit-Besucheridentifizierung mithilfe der RTP User Context-API mit Parametern, Callback-Beispiel und Beispielantworten für Segmente, ABM und Standort.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
TQID: https://experienceleague.adobe.com/B-JMACtMs3aRVsb1eJKaRoQGgVKB6MTbd0KBoZ7Ay6k
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 223
ht-degree: 4%

---

# Besucherdaten abrufen

Diese Methode wird verwendet, um Echtzeit-Besucheridentifizierungsdaten abzurufen.

- Sie müssen Web Personalization-Kunde werden und das [RTP-Tag ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

Wenn ein Fehler auftritt, wird als Teil der Antwort-JSON eine Fehlermeldung angezeigt. Wenn ein 500-Code zurückgegeben wird, wenden Sie sich mit der von Ihnen gestellten Anfrage an den Support.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
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
