---
title: Besucherdaten abrufen
description: Erhalten Sie eine Echtzeit-Besucheridentifizierung mithilfe der RTP User Context-API mit Parametern, Callback-Beispiel und Beispielantworten für Segmente, ABM und Standort.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
TQID: 'https://experienceleague.adobe.com/B-JMACtMs3aRVsb1eJKaRoQGgVKB6MTbd0KBoZ7Ay6k'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: dda1332a-c3e0-583f-9d9b-15f1934e0ad3
    internal-label: Javascript
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 4%
---
# Besucherdaten abrufen

Verwenden Sie diese Methode, um Besucheridentifikationsdaten in Echtzeit zu erhalten.

- Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

Wenn ein Fehler auftritt, enthält die JSON-Antwort eine Fehlermeldung. Wenn die API einen 500-Code zurückgibt, wenden Sie sich an den Support und geben Sie die Anfrage an, die den Fehler verursacht hat.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| `get` | Erforderlich | String | Aktion der Methode. |
| `visitor` | Erforderlich | String | Methodenname. |
| `callback` | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst werden soll. |

## Beispiele

Im folgenden Beispiel werden Daten zur Besucheridentifizierung abgerufen.

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Antwort mit Segment Match:

Die folgende Antwort enthält `matchedSegments`, da der Besucher vor dem Aufruf der API zum Abrufen von Besucherdaten mit Echtzeit-Segmenten abgeglichen hat.

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

Die folgende Antwort enthält keine `matchedSegments`, da der Besucher vor dem Aufruf der API zum Abrufen der Besucherdaten mit keinen Echtzeit-Segmenten übereinstimmte.

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
