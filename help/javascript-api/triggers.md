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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 117
ht-degree: 8%

---

# Trigger

Trigger führen Funktionen aus, wenn das globale `rtp` einen angegebenen Status erreicht.

Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

`rtp('triggerName', function_to_trigger);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;triggerName&#39; | Erforderlich | String | Methodenname. |
| function_to_Trigger | Erforderlich | Funktion | Funktion auf Trigger. |

### Benutzerkontextbereiter Trigger

Der `userContextReady` Trigger ruft eine Funktion auf, wenn das globale `rtpUserContext` fertig ist. Im folgenden Beispiel wird eine benutzerdefinierte Variable basierend auf dem Speicherort des Benutzers festgelegt.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
