---
title: Nutzung
feature: REST API
description: Überwachen Sie die Nutzung und Fehler der Marketo REST-API mit Statistikendpunkten für täglich und letzte 7 Tage, einschließlich der Anzahl pro Benutzer und der Gesamtwerte des Fehler-Codes.
exl-id: 935a00a4-1e1e-4b48-ae9c-72c5e578312a
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
subfeature_v2:
  - id: cf1396d8-ab85-4e93-b35d-d9b573024abf
    internal-label: REST APIs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 9%
---
# Nutzung

[Usage Endpoint Reference](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

Die Nutzungs-APIs fassen die Nutzung der REST-API und die Fehleraktivität für Ihr Abonnement zusammen. Verwenden Sie diese Endpunkte, um Integrationen zu überwachen, das tägliche Anrufvolumen zu verfolgen und Fehlertrends zu identifizieren.

Die Nutzungsdaten enthalten eine Gesamtanzahl der API-Aufrufe und eine Aufschlüsselung pro Benutzer. Die Fehlerdaten enthalten eine Gesamtanzahl von Fehlern und eine Aufschlüsselung nach Fehlercode.

Die Nutzungs-APIs verwenden dieselbe Authentifizierungsmethode wie andere Marketo REST-APIs. Übergeben Sie das Zugriffstoken in der `Authorization: Bearer {accessToken}`.

## Endpunkte

| Methode | Pfad | Beschreibung |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Ruft die API-Nutzung für den aktuellen Tag ab. |
| GET | `/rest/v1/stats/usage/last7days.json` | Ruft die API-Nutzung der letzten 7 Tage ab. |
| GET | `/rest/v1/stats/errors.json` | Ruft API-Fehler für den aktuellen Tag ab. |
| GET | `/rest/v1/stats/errors/last7days.json` | Ruft API-Fehler der letzten 7 Tage ab. |

## Tägliche Nutzung

Ruft die API-Nutzung für den aktuellen Tag ab.

```http
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Jedes Objekt im `result`-Array enthält die Nutzungssummen für einen Tag und eine Aufschlüsselung pro Benutzer.

## Nutzung der letzten 7 Tage

Ruft die API-Nutzung der letzten 7 Tage ab. Jedes Element im `result` stellt einen Tag dar.

```http
GET /rest/v1/stats/usage/last7days.json
```

## Tägliche Fehler

Ruft API-Fehler für den aktuellen Tag ab.

```http
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Jedes Objekt im `result`-Array enthält einen Tag mit Fehlersummen und eine Aufschlüsselung nach Fehlercode.

## Fehler der letzten 7 Tage

Ruft API-Fehler der letzten 7 Tage ab. Jedes Element im `result` stellt einen Tag dar.

```http
GET /rest/v1/stats/errors/last7days.json
```

## Antwort-Member

### Usage Result Object

| Name | Datentyp | Beschreibung |
| --- | --- | --- |
| `date` | Zeichenfolge | Das Datum für die Nutzungszusammenfassung im `YYYY-MM-DD`. |
| `total` | Ganzzahl | Gesamtzahl der API-Aufrufe für diesen Tag. |
| `users` | Array | Liste der Nutzungszahlen pro Benutzer für diesen Tag. |

### Benutzerobjekt verwenden

| Name | Datentyp | Beschreibung |
| --- | --- | --- |
| `userId` | Zeichenfolge | API-Benutzerkennung. |
| `count` | Ganzzahl | Anzahl der API-Aufrufe, die dieser Benutzer für den Tag tätigt. |

### Fehlerergebnisobjekt

| Name | Datentyp | Beschreibung |
| --- | --- | --- |
| `date` | Zeichenfolge | Das Datum für die Fehlerzusammenfassung im `YYYY-MM-DD`. |
| `total` | Ganzzahl | Gesamtzahl der API-Fehler für diesen Tag. |
| `errors` | Array | Liste der Zählungen pro Fehlercode für diesen Tag. |

### Fehlerobjekt

| Name | Datentyp | Beschreibung |
| --- | --- | --- |
| `errorCode` | Zeichenfolge | Marketo-Fehlercode. |
| `count` | Ganzzahl | Gibt an, wie oft dieser Fehler an diesem Tag aufgetreten ist. |

## Hinweise

Die Nutzungsantwort meldet jeden API-Benutzer separat. Durch die Zuweisung von Integrationen zu separaten API-Benutzern kann leichter ermittelt werden, welcher Service Kontingente in Anspruch nimmt und Fehler verursacht.
