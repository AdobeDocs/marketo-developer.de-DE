---
title: Trigger
description: Verwenden Sie RTP-Trigger in Web Personalization, um Funktionen im RTP-Status auszuführen, einschließlich userContextReady, mit Syntax, Parametern und einem Beispiel für einen Speicherort.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 10%

---

# Trigger

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
