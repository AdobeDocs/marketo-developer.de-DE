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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 241
ht-degree: 3%

---

# Benutzerspezifische Datenereignisse

Verwenden Sie diese Methode, um benutzerdefinierte Ereignisse zum Tracking und zur Echtzeit-Personalisierung zu senden. Sie können Drittanbieterdaten oder Trigger ein benutzerspezifisches Ereignis senden, das auf dem Besucherverhalten basiert.

Jedes benutzerspezifische Datenereignis wird während der Sitzung eines Besuchers einmal gezählt.

Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

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

Das benutzerdefinierte Daten-Array kann bis zu vier Elemente enthalten. Um mehr als vier Elemente zu senden, rufen Sie die API Ereignis senden wiederholt auf, wobei in jedem Aufruf nicht mehr als vier Elemente enthalten sind.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Senden eines Ereignisses basierend auf einem Klick auf eine Schaltfläche

In diesem Beispiel wird ein benutzerdefiniertes Datenereignis gesendet, wenn ein Besucher auf die Schaltfläche klickt, um ein bestimmtes Whitepaper herunterzuladen. RTP kann das Ereignis verwenden, um diese Besucher in Echtzeit zu segmentieren.

Auf der Website kann dann nach zwei weiteren Klicks eine personalisierte Kampagne angezeigt werden. Beispielsweise kann die Kampagne einen weiteren Inhalt im Zusammenhang mit dem heruntergeladenen Whitepaper darstellen.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
