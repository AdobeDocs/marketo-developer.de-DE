---
title: Benutzerkontext
feature: REST API
description: Erfahren Sie, wie Sie die Marketo RTP User Context API aktivieren und verwenden, um benutzerdefinierte Variablen festzulegen, Benutzerdaten über Besuche hinweg zu lesen und angesehene und angeklickte Kampagnen zu verfolgen.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
TQID: https://experienceleague.adobe.com/Ph0Tw-C9jzWaR4bYyUIXyzzoa2yjHQk2gt6tNA8H2mA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2:
  - id: a1d50dda-6d94-4e16-8c30-5eb7181c4650
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 273
ht-degree: 5%

---

# Benutzerkontext

Die User Context JavaScript-API stellt Daten auf Benutzerebene und Besucherebene über mehrere Sitzungen hinweg bereit. Verwenden Sie historisches Verhalten und historische Daten, um eine erweiterte Personalisierung zu erstellen.

Die API stellt auch benutzerdefinierte Variablen zum Senden von Daten und Ereignissen an das RTP-Backend zur Segmentierung und Personalisierung bereit. Siehe die zugehörigen [&#128279;](../javascript-api/triggers.md) und [Musterübereinstimmung](../javascript-api/pattern-match.md)-Funktionen.

- Sie müssen Web Personalization-Kunde sein und das [RTP-Tag &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen.
- Sie müssen den Marketo-Support bitten, die Benutzerkontext-API zu aktivieren. Nach der Aktivierung wird ein userContext-Objekt unter dem globalen RTP-Objekt verfügbar gemacht.

## Benutzerkontexteigenschaften

| Name | Typ | Beschreibung |
| --- | --- | --- |
| `customVar[1-5]` | Zeichenfolge | Benutzerdefinierte Daten werden im Benutzerkontext gespeichert. |
| `viewedCampaigns` | Kampagnen-IDs als kommagetrennte Zeichenfolge | Kampagnen bei aktuellen oder vorherigen Besuchen angezeigt. |
| `clickedCampaigns` | Kampagnen-IDs als kommagetrennte Zeichenfolge | Durch Kampagnen bei aktuellen oder vorherigen Besuchen geklickt. |

## Festlegen benutzerdefinierter Variablen

Festlegen benutzerdefinierter Variablen zum Hinzufügen von Daten zum Benutzerkontext.

### Nutzung

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| `'set'` | Erforderlich | String | Aktion der Methode. |
| `customVar` | Erforderlich | String | Benutzerdefinierter Variablenname |
| `my_custom_value` | Erforderlich | String | Benutzerdefinierter Wert, der für die benutzerdefinierte Variable in Index 1-5 gespeichert wird. |

Benutzerdefinierte Variablen werden nur in einem Ansichtsaufruf an RTP gesendet. Festlegen benutzerdefinierter Variablen vor dem Ansichtsaufruf. Andernfalls werden die Variablen beim nächsten Ansichtsaufruf gesendet.

Benutzerdefinierte Variablen weisen die folgenden Einschränkungen auf:

- Eine benutzerdefinierte Variable darf 100 Zeichen nicht überschreiten.
- Die Kampagnendaten sind auf die letzten zehn Besuche beschränkt, mit zehn Kampagnen pro Besuch.

### Nutzung

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');

// Read location
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}

// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
