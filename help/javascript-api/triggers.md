---
title: Trigger
description: Verwenden Sie RTP-Trigger in Web Personalization, um Funktionen im RTP-Status auszuführen, einschließlich userContextReady, mit Syntax, Parametern und einem Beispiel für einen Speicherort.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM
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
source-wordcount: 116
ht-degree: 8%

---

# Trigger

Fügt Trigger-Funktionen die Funktion für bestimmte Zustände des globalen RTP-Objekts hinzu.

Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

`rtp('triggerName', function_to_trigger);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
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
