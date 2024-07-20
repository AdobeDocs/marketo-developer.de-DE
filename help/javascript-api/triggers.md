---
title: Auslöser
description: Auslöser
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 13%

---

# Auslöser

Fügt die Funktion zu Trigger-Funktionen in bestimmten Status des globalen rtp-Objekts hinzu.

Sie müssen Web-Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen, bevor Sie die User Context-API verwenden.

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
