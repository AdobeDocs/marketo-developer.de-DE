---
title: "Trigger"
description: "Trigger"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 11%

---


# Auslöser

Fügt die Funktion zu Trigger-Funktionen in bestimmten Status des globalen rtp-Objekts hinzu.

Sie müssen Web Personalization-Kunde sein und über die Variable [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der Benutzerkontext-API.

## Nutzung

`rtp('triggerName', function_to_trigger);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Erforderlich | Zeichenfolge | Name der Methode. |
| function_to_Trigger | Erforderlich | Funktion | Funktion zum Trigger. |


### Trigger &quot;Benutzerkontext bereit&quot;

Legt eine benutzerdefinierte Variable basierend auf dem Benutzerspeicherort fest. Diese Funktion wird aufgerufen, wenn das globale Objekt &quot;rtpUserContext&quot;bereit ist.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
