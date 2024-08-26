---
title: Benutzerspezifische Datenereignisse
description: Verwenden Sie die JavaScript-API Benutzerspezifische Datenereignisse , um Ihre eindeutigen Ereignisse zu verfolgen.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
source-git-commit: e609f9d5d58f656298412acef5e2106a19765396
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 3%

---

# Benutzerspezifische Datenereignisse

Diese Methode sendet benutzerspezifische Ereignisse zum Tracking und zur Echtzeit-Personalisierung. Sie kann zum Senden von Drittanbieterdaten oder zum Trigger Ihres eigenen benutzerspezifischen Ereignisses basierend auf dem Besucherverhalten verwendet werden. Benutzerspezifische Datenereignisse werden einmal in der Sitzung eines Besuchers gezählt.

Sie müssen Web-Personalization-Kunde werden und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen, bevor Sie die User Context-API verwenden.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| `send` | Erforderlich | String | Methodenaktion |
| `event` | Erforderlich | String | Name der Methode. |
| `customData` | Erforderlich | Zeichenfolge oder Array | Benutzerdefinierte Daten. |

## Beispiele

### Ereignis mit Zeichenfolge für benutzerdefinierte Daten senden

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Ereignis mit Array von Zeichenfolgen für benutzerdefinierte Daten senden

Das benutzerdefinierte Daten-Array kann maximal vier Elemente enthalten.  Wenn Sie mehr als vier Elemente senden müssen, rufen Sie die Send Event API wiederholt auf (mit maximal vier Elementen), bis alle Elemente gesendet werden.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Ereignis basierend auf Schaltflächen-Klick senden

Marketo personalisiert Inhalte auf seiner Website für Webbesucher, die ein bestimmtes Whitepaper herunterladen. Dies geschieht durch Erfassen des Klicks des Besuchers auf die Schaltfläche zum Herunterladen von Whitepaper, über die ein benutzerspezifisches Datenereignis gesendet wird. RTP segmentiert alle Besucher in Echtzeit, die auf die Whitepaper-Schaltfläche zum Herunterladen geklickt haben und jedem Besucher eine personalisierte Kampagne mit zwei Klicks später zeigen. Dies wird erreicht, indem ein anderes Inhaltselement zum heruntergeladenen Whitepaper angezeigt wird.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
