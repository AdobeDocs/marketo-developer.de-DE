---
title: Web-Personalisierung
description: Web-Personalisierung
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 7%

---

# Web-Personalisierung

Die Web Personalization JavaScript-API erweitert die automatisierte Personalisierungsfunktion der Plattform. Sie ermöglicht die Ereignisverfolgung und dynamische Anpassung einer Web-Seite. Zusätzliche Funktionen: [Benutzerdefinierte Datenereignisse](custom-data-events.md), [Dynamischer Inhalt](web-personalization.md), [Besucherdaten abrufen](get-visitor-data.md), [Tag für bestimmte Bots ausschließen](#exclude_tag_for_specific_bots).

- Bevor Sie die User Context-API verwenden können, müssen Sie Web Personalization-Kunde [ und das RTP](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript)Tag auf Ihrer Site bereitgestellt haben.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

## Tag-Setup

Das RTP-Tag sollte in die Kopfzeile der personalisierten Seite eingefügt werden.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Konto-Setup

Diese Methode wird automatisch auf Tag-Ebene aufgerufen, um die entsprechende Konto-ID festzulegen. Sie können die Konto-ID festlegen, wenn Sie eine Aufteilung auf verschiedene Domains wünschen.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Erforderlich | String | Methodenname. |
| accountId | Erforderlich | String | Konto-ID |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funktionen zum Senden von Ereignissen

Diese Methode sendet ein Ansichtsereignis, das für die Seitenverfolgung verwendet wird. Im folgenden Beispiel wird die aktuelle Seiten-URL als Besucherseitenansicht verfolgt.

Durch Übergeben des optionalen Parameters „page“ in dieser Methode kann die aktuelle Seite überschrieben werden.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|-----------|-------------------|--------|---------------------------------|
| &#39;Senden&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;Ansicht&#39; | Erforderlich | String | Methodenname. |
| Seite | Optional | String | Relativer Pfad oder vollständige Seiten-URL. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Tag für bestimmte Bots ausschließen (Benutzeragenten)

Um bestimmte Browser vom Senden von Daten an die Web-Personalization-Plattform auszuschließen (im Falle identifizierter Bots), fügen Sie die folgende IF-Anweisung zum Tag-Skript hinzu.

Im folgenden Code-Beispiel wird „googlebot|msnbot“ als Bot-Beispiel verwendet, um aus Web-Personalization-Aktivitäten auszuschließen.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## JavaScript-Aufrufe - Erklärung

Beschreibung von JavaScript, die einer Website bei Verwendung von Web-Personalization und prädiktiven Inhalten hinzugefügt wird.

### Core/Dependent JavaScript

| Name | Beschreibung | Kontrollvariante |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | – | Kontrolliert von Marketo |
| jquery.min.js | v1.8.3 | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |
| jquery-custom-ui-min.js | v1.9.2 | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |
| query-ui-1.8.17-dialog.js | v1.9.2* | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |


*Wird nur verwendet, wenn das Dialogfeld in der jQuery-Benutzeroberfläche fehlt.

### JavaScript On Demand

| Name | Beschreibung | Kontrollvariante |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Wird verwendet, wenn die Integration von Google Analytics/Facebook/SiteCatalyst aktiviert ist. | Kontrolliert von Marketo |
| insightera-bar-2.1.js | Wird verwendet, wenn die Leiste für prädiktive Inhaltsempfehlungen aktiviert ist | Kontrolliert von Marketo |
| froogaloop2.min.js | Wird verwendet, wenn das Content-Tracking aktiviert ist und der Vimeo-Player auf der Seite vorhanden ist. | – |
| iframe-api-v1.js | Wird verwendet, wenn das Inhalts-Tracking aktiviert ist und der YouTube-Player auf der Seite vorhanden ist | – |
