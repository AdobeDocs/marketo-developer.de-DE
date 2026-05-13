---
title: Benutzerspezifische Datenereignisse
description: Senden Sie benutzerspezifische Ereignisse mit der RTP JavaScript-API für Web Personalization mit Parametern, Zeichenfolgen- oder Array-Daten von bis zu vier Elementen und klickbasierten Triggern.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
TQID: https://experienceleague.adobe.com/oWDmtMF94xG5HYXeTwkx5zF9PWo98bpwoVB6kAKLYDo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 263
ht-degree: 3%

---

# Benutzerspezifische Datenereignisse

Diese Methode sendet benutzerdefinierte Ereignisse zum Tracking und zur Echtzeit-Personalisierung. Sie kann zum Senden von Drittanbieterdaten oder zum Trigger Ihres eigenen benutzerspezifischen Ereignisses auf Grundlage des Besucherverhaltens verwendet werden. Benutzerdefinierte Datenereignisse werden einmal in der Sitzung eines Besuchers gezählt.

Bevor Sie die User Context-API verwenden können, müssen Sie Web Personalization-Kunde [&#128279;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) und das RTPTag auf Ihrer Site bereitgestellt haben.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| `send` | Erforderlich | String | Aktion der Methode. |
| `event` | Erforderlich | String | Methodenname. |
| `customData` | Erforderlich | Zeichenfolge oder Array | Benutzerdefinierte Daten. |

## Beispiele

### Senden eines Ereignisses mit einer Zeichenfolge für benutzerdefinierte Daten

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Senden eines Ereignisses mit einem Array von Zeichenfolgen für benutzerdefinierte Daten

Das benutzerdefinierte Daten-Array kann maximal vier Elemente enthalten.  Wenn Sie mehr als vier Elemente senden müssen, rufen Sie die Ereignis-API wiederholt (mit maximal vier Elementen) auf, bis alle Elemente gesendet werden.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Senden eines Ereignisses basierend auf einem Klick auf eine Schaltfläche

Marketo personalisiert Inhalte auf seiner Website für Web-Besucher, die ein bestimmtes Whitepaper herunterladen. Dazu erfassen sie den Klick des Besuchers auf die Schaltfläche zum Herunterladen des Whitepapers, die ein benutzerdefiniertes Datenereignis sendet. RTP-Segmente in Echtzeit für alle Besucher, die auf die Schaltfläche Whitepaper herunterladen geklickt haben, um jedem Besucher eine personalisierte Kampagne mit zwei Klicks später zu zeigen. Dies wird durch die Anzeige eines weiteren Inhalts im Zusammenhang mit dem heruntergeladenen Whitepaper erreicht.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
