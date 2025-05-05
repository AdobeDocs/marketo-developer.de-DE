---
title: Benutzerspezifische Datenereignisse
description: Verwenden Sie die JavaScript-API für benutzerdefinierte Datenereignisse zum Tracking Ihrer eindeutigen Ereignisse.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
source-git-commit: e609f9d5d58f656298412acef5e2106a19765396
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 3%

---

# Benutzerspezifische Datenereignisse

Diese Methode sendet benutzerdefinierte Ereignisse zum Tracking und zur Echtzeit-Personalisierung. Sie kann zum Senden von Drittanbieterdaten oder zum Trigger Ihres eigenen benutzerspezifischen Ereignisses auf Grundlage des Besucherverhaltens verwendet werden. Benutzerdefinierte Datenereignisse werden einmal in der Sitzung eines Besuchers gezählt.

Bevor Sie die User Context-API verwenden können, müssen Sie Web Personalization-Kunde [&#128279;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) und das RTPTag auf Ihrer Site bereitgestellt haben.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
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
