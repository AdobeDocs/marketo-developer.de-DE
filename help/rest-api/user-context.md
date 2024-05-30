---
title: "Benutzerkontext"
feature: REST API
description: "Übersicht über den Benutzerkontext und API-Beschreibungen"
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 5%

---


# Benutzerkontext

Die JavaScript-API für Benutzerkontext legt Daten auf Benutzer- und Besucherebene über mehrere Sitzungen hinweg offen, um eine erweiterte Personalisierungsfunktion zu ermöglichen, die historisches Benutzerverhalten und historische Daten verwendet. Die API geht über das Lesen von Daten hinaus und stellt benutzerdefinierte Variablen bereit, mit denen Sie aussagekräftige Daten und Ereignisse zu erweiterten Segmentierungs- und Personalisierungszwecken an das RTP-Backend senden können. Zusätzliche Funktionen: [Trigger](../javascript-api/triggers.md), [Musterübereinstimmung](../javascript-api/pattern-match.md).

- Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der Benutzerkontext-API.
- Die Benutzerkontext-API ist eine Funktion, die vom Marketo-Support auf Anfrage aktiviert werden muss. Wenn die API aktiviert ist, wird ein userContext-Objekt unter dem globalen RTP-Objekt bereitgestellt.

## Benutzerkontextattribute

| Name | Typ | Beschreibung |
|------------------|-------------|------|
| customVar[1-5] | Zeichenfolge | Benutzerdefinierte Daten, die im Benutzerkontext gespeichert werden. |
| displayedCampaigns | Kampagnen-IDs als kommagetrennte Zeichenfolge | Kampagnen bei aktuellen oder vorherigen Besuchen angezeigt. |
| clickedCampaigns | Kampagnen-IDs als kommagetrennte Zeichenfolge | Durch Kampagnen bei aktuellen oder vorherigen Besuchen geklickt. |

## Festlegen benutzerdefinierter Variablen

Hinzufügen benutzerdefinierter Daten zum Benutzerkontext.

### Nutzung

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| customVar | Erforderlich | Zeichenfolge | Benutzerdefinierter Variablenname. |
| my_custom_value | Erforderlich | Zeichenfolge | Benutzerdefinierter Wert, der für eine benutzerdefinierte Variable in Index 1-5 gespeichert werden soll. |

Hinweis: Benutzerdefinierte Variablen werden nur im Ansichtsaufruf an RTP gesendet. Daher wird empfohlen, benutzerdefinierte Variablen festzulegen, bevor die Ansicht aufgerufen wird. Andernfalls wird sie nur beim nächsten Ansichtsaufruf gesendet.

Benutzerdefinierte Var-Einschränkungen

- Die Länge benutzerspezifischer Variablen darf nicht länger als 100 Zeichen sein.
- Die Kampagnendaten sind auf die letzten zehn Besuche mit zehn Kampagnen pro Besuch beschränkt.

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
