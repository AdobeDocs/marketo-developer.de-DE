---
title: "Web-Personalisierung"
description: "Web-Personalisierung"
feature: Web Personalization, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 5%

---


# Web-Personalisierung

Die Web Personalization JavaScript-API erweitert die automatisierte Personalisierungsfunktion der Plattform. Sie ermöglicht die Verfolgung von Ereignissen und die dynamische Anpassung einer Webseite. Zusätzliche Funktionen: [Benutzerspezifische Datenereignisse](custom-data-events.md), [Dynamische Inhalte](web-personalization.md), [Abrufen von Besucherdaten](get-visitor-data.md), [Ausschluss von Tags für bestimmte Bots](#exclude_tag_for_specific_bots).

- Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der Benutzerkontext-API.
- RTP unterstützt keine Konto-basierten Marketinglisten mit Namen. ABM-Listen und -Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die innerhalb von RTP verwaltet werden.

## Tag-Einrichtung

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

## Kontoeinrichtung

Diese Methode wird automatisch auf Tag-Ebene aufgerufen, um die relevante Konto-ID festzulegen. Sie können die Konto-ID festlegen, wenn Sie zwischen verschiedenen Domänen aufteilen möchten.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Erforderlich | Zeichenfolge | Name der Methode. |
| accountId | Erforderlich | Zeichenfolge | Konto-ID. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Ereignissendefunktionen

Diese Methode sendet ein Ansichtsereignis, das für die Seitenverfolgung verwendet wird. Im folgenden Beispiel wird die aktuelle Seiten-URL als Besucherseitenansicht verfolgt.

Durch Übergabe des optionalen Parameters &quot;page&quot;in dieser Methode kann die aktuelle Seite überschrieben werden.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|-----------|-------------------|--------|---------------------------------|
| &#39;send&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &quot;view&quot; | Erforderlich | Zeichenfolge | Name der Methode. |
| Seite | optional | Zeichenfolge | Relativer Pfad oder vollständige Seiten-URL. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Ausschluss von Tags für bestimmte Bots (Benutzeragenten)

Um bestimmte Browser vom Senden von Daten an die Web Personalization-Plattform auszuschließen (im Fall von identifizierten Bots), fügen Sie die folgende IF-Anweisung zum Tag-Skript hinzu.

Im folgenden Codebeispiel wird &quot;googlebot|msnbot&quot;als Bot-Beispiele verwendet, um ihn aus Web-Personalisierungsaktivitäten auszuschließen.

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

## Erläuterung von JavaScript-Aufrufen

Beschreibung von JavaScript, das einer Website bei Verwendung von Web-Personalisierung und prädiktiven Inhalten hinzugefügt wird.

### Kern/Abhängiges JavaScript

| Name | Beschreibung | Kontrolle |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Kontrolliert von Marketo |
| jquery.min.js | v1.8.3 | Kann deaktiviert werden, indem Sie sich an den Marketo-Support wenden |
| jquery-custom-ui-min.js | v1.9.2 | Kann deaktiviert werden, indem Sie sich an den Marketo-Support wenden |
| query-ui-1.8.17-dialog.js | v1.9.2* | Kann deaktiviert werden, indem Sie sich an den Marketo-Support wenden |


*Wird nur verwendet, wenn das Dialogfeld &quot;jQuery UI&quot;fehlt

### On-Demand-JavaScript

| Name | Beschreibung | Kontrolle |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Wird verwendet, wenn die Google Analytics/Facebook/SiteCatalyst-Integration aktiviert ist. | Kontrolliert von Marketo |
| insightera-bar-2.1.js | Wird verwendet, wenn die Empfehlungsleiste für prädiktive Inhalte aktiviert ist | Kontrolliert von Marketo |
| froogaloop2.min.js | Wird verwendet, wenn das Content-Tracking aktiviert ist und der Vimeo-Player auf der Seite vorhanden ist | - |
| iframe-api-v1.js | Wird verwendet, wenn das Content-Tracking aktiviert ist und der YouTube-Player auf der Seite vorhanden ist | - |

