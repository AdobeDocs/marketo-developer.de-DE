---
title: Triggers
description: Triggers
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 13%

---

# Triggers

Fügt Trigger-Funktionen die Funktion für bestimmte Zustände des globalen RTP-Objekts hinzu.

Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

`rtp('triggerName', function_to_trigger);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Erforderlich | String | Methodenname. |
| function_to_Trigger | Erforderlich | Funktion | Funktion auf Trigger. |


### Benutzerkontextbereiter Trigger

Legt eine benutzerdefinierte Variable basierend auf dem Speicherort des Benutzers fest. Diese Funktion wird aufgerufen, wenn das globale Objekt „rtpUserContext“ bereit ist.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
