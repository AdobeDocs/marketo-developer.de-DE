---
title: "Benutzerspezifische Datenereignisse"
description: "Custom Data Events API"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 4%

---


# Benutzerspezifische Datenereignisse

Diese Methode sendet benutzerspezifische Ereignisse zum Tracking und zur Echtzeit-Personalisierung. Sie kann zum Senden von Drittanbieterdaten oder zum Trigger Ihres eigenen benutzerspezifischen Ereignisses basierend auf dem Besucherverhalten verwendet werden. Benutzerspezifische Datenereignisse werden einmal in der Sitzung eines Besuchers gezählt.

Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der Benutzerkontext-API.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| `send` | Erforderlich | Zeichenfolge | Methodenaktion |
| `event` | Erforderlich | Zeichenfolge | Name der Methode. |
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
