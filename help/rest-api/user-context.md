---
title: Benutzerkontext
feature: REST API
description: Erfahren Sie, wie Sie die Marketo RTP User Context API aktivieren und verwenden, um benutzerdefinierte Variablen festzulegen, Benutzerdaten über Besuche hinweg zu lesen und angesehene und angeklickte Kampagnen zu verfolgen.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 5%

---

# Benutzerkontext

User Context JavaScript-API stellt Daten auf Benutzer- und Besucherebene über mehrere Sitzungen hinweg bereit, um erweiterte Personalisierungsfunktionen unter Verwendung historischer Benutzerverhaltensdaten und -daten zu ermöglichen. Die API geht über das Lesen von Daten hinaus und stellt benutzerdefinierte Variablen bereit, mit denen Sie aussagekräftige Daten und Ereignisse für erweiterte Segmentierungs- und Personalisierungszwecke an das RTP-Backend übertragen können. Zusätzliche Funktionen: [Trigger ](../javascript-api/triggers.md), [Musterübereinstimmung](../javascript-api/pattern-match.md).

- Bevor Sie die User Context-API verwenden können, müssen Sie Web Personalization-Kunde [ und das RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript)Tag auf Ihrer Site bereitgestellt haben.
- Die User Context-API ist eine Funktion, die vom Marketo-Support auf Anfrage aktiviert werden muss. Wenn die API aktiviert ist, wird ein userContext-Objekt unter dem globalen RTP-Objekt verfügbar gemacht.

## Benutzerkontexteigenschaften

| Name | Typ | Beschreibung |
|------------------|-------------|------|
| customVar[1-5] | String | Benutzerdefinierte Daten werden im Benutzerkontext gespeichert. |
| Angezeigte Kampagnen | Kampagnen-IDs als kommagetrennte Zeichenfolge | Kampagnen bei aktuellen oder vorherigen Besuchen angezeigt. |
| ClickedCampaign | Kampagnen-IDs als kommagetrennte Zeichenfolge | Durch Kampagnen bei aktuellen oder vorherigen Besuchen geklickt. |

## Festlegen benutzerdefinierter Variablen

Hinzufügen benutzerdefinierter Daten zum Benutzerkontext.

### Nutzung

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|-----------------|-------------------|--------|-----------------|
| &#39;Satz&#39; | Erforderlich | String | Aktion der Methode. |
| customVar | Erforderlich | String | Benutzerdefinierter Variablenname |
| my_custom_value | Erforderlich | String | Benutzerdefinierter Wert, der für die benutzerdefinierte Variable in Index 1-5 gespeichert wird. |

Hinweis: Benutzerdefinierte Variablen werden nur im Ansichtsaufruf an RTP gesendet. Daher wird empfohlen, benutzerdefinierte Variablen festzulegen, bevor die Ansicht aufgerufen wird. Andernfalls wird sie nur beim nächsten Ansichtsaufruf gesendet.

Benutzerdefinierte VAR-Einschränkungen

- Benutzerdefinierte Variable darf nicht länger als 100 Zeichen sein.
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
