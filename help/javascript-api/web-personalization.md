---
title: Web-Personalisierung
description: Handbuch für die Web Personalization JavaScript-API und das RTP-Tag, in dem Seitenansichtsereignisse, Kontoeinrichtung, Bot-Ausschlüsse sowie Kern- und On-Demand-Skripte behandelt werden
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
TQID: https://experienceleague.adobe.com/yplunKmgjOJ7gJTA2TDc9cfJXyXbrVWuM-NdVbDMN4A
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: cdd4e0f6-e87e-453f-88ee-2ee54a7de272
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 435
ht-degree: 6%

---

# Web-Personalisierung

Die Web Personalization JavaScript-API verfolgt Ereignisse und passt Web-Seiten dynamisch an. Dies erweitert die automatisierten Personalisierungsfunktionen der Plattform.

Zu den zugehörigen Funktionen gehören [Benutzerdefinierte Datenereignisse](custom-data-events.md), [Dynamischer Inhalt](web-personalization.md), [Besucherdaten abrufen](get-visitor-data.md) und [Tag ausschließen für bestimmte Bots](#exclude_tag_for_specific_bots).

- Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

## Tag-Setup

Fügen Sie das RTP-Tag in die Kopfzeile jeder personalisierten Seite ein.

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

Das Tag ruft diese Methode automatisch auf, um die entsprechende Konto-ID festzulegen. Legen Sie die Konto-ID explizit fest, wenn Sie verschiedene Konten für verschiedene Domains verwenden möchten.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;setAccount&#39; | Erforderlich | String | Methodenname. |
| accountId | Erforderlich | String | Konto-ID |

```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funktionen zum Senden von Ereignissen

Diese Methode sendet ein Ansichtsereignis für das Seiten-Tracking. Beim ersten Aufruf im folgenden Beispiel wird die aktuelle Seiten-URL als Besucherseitenansicht verfolgt.

Übergeben Sie den optionalen Parameter „page“, um die aktuelle Seite zu überschreiben, wie im zweiten Aufruf gezeigt.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
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

Um zu verhindern, dass identifizierte Bots Daten an die Web-Personalization-Plattform senden, fügen Sie dem Tag-Skript die folgende `if`-Anweisung hinzu.

In diesem Beispiel werden die Benutzeragenten von „googlebot|msnbot“ aus den Aktivitäten von Web Personalization ausgeschlossen.

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

In den folgenden Tabellen wird die JavaScript beschrieben, die zu einer Website hinzugefügt wurde, die Web-Personalization und prädiktiven Inhalt verwendet.

### Core/Dependent JavaScript

| Name | Beschreibung | Kontrollvariante |
| --- | --- | --- |
| rtp.js | – | Kontrolliert von Marketo |
| jquery.min.js | v1.8.3 | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |
| jquery-custom-ui-min.js | v1.9.2 | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |
| query-ui-1.8.17-dialog.js | v1.9.2* | Kann deaktiviert werden, indem man sich an den Marketo-Support wendet |

*Wird nur verwendet, wenn das jQuery UI-Dialogfeld fehlt.

### JavaScript On Demand

| Name | Beschreibung | Kontrollvariante |
| --- | --- | --- |
| ga-integration-2.0.1.js | Wird verwendet, wenn die Integration von Google Analytics/Facebook/SiteCatalyst aktiviert ist. | Kontrolliert von Marketo |
| insightera-bar-2.1.js | Wird verwendet, wenn die Leiste für prädiktive Inhaltsempfehlungen aktiviert ist | Kontrolliert von Marketo |
| froogaloop2.min.js | Wird verwendet, wenn das Content-Tracking aktiviert ist und der Vimeo-Player auf der Seite vorhanden ist. | – |
| iframe-api-v1.js | Wird verwendet, wenn das Inhalts-Tracking aktiviert ist und der YouTube-Player auf der Seite vorhanden ist | – |
