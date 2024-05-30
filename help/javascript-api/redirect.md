---
title: "Umleiten"
description: "Umleiten"
feature: Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 8%

---


# Weiterleiten

Mit der RTP-Umleitungs-API können Sie segmentierte Zielgruppen zu einer Ziel-URL umleiten.

- Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der User Context-API.
- RTP unterstützt keine Konto-basierten Marketinglisten mit Namen. ABM-Listen und -Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die innerhalb von RTP verwaltet werden.

## Nutzung

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;send&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &quot;redirect&quot; | Erforderlich | Zeichenfolge | Name der Methode. |
| field_name | Erforderlich | Zeichenfolge | Feldname, mit dem eine Übereinstimmung gefunden werden soll. Beispiel: &quot;abm.name&quot;(siehe unten). |
| values_array | Erforderlich | Tabelle | Liste der Werte, mit denen das Feld abgeglichen werden soll (nicht zwischen Groß- und Kleinschreibung unterschieden). |
| redirect_url | Erforderlich | Zeichenfolge | Target-URL, um Besucher umzuleiten, die mit der Bedingung übereinstimmen. |
| redirect_match_visitors | optional | Boolesch | Wenn &quot;true&quot;, werden Besucher, die mit Bedingungen übereinstimmen, umgeleitet. Wenn &quot;false&quot;, werden nicht übereinstimmende Besucher umgeleitet. Standard: true. |

Organisation, Branche, ABM-Listen, Standort, ISP, übereinstimmende Segmente

| Bedingung | Datenhierarchie | Beispiel |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Matched Segments (funktioniert nur nach dem ersten Klick) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.name&#39; , [&quot;Fortune 1.000&quot; , &quot;Enterprise&quot;] , &quot;http://www.marketo.com&#39;); |
| Matched Segments (funktioniert nur nach dem ersten Klick) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.id&#39; , [106 , 107 , 190] , &quot;http://www.marketo.com&#39;); |
| ABM-Listen | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_Customers&#39;] , &quot;http://www.marketo.com&#39;); |
| ABM-Listen | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &quot;http://www.marketo.com&#39;); |
| Organisationen | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &quot;http://www.marketo.com&#39;); |
| Standort | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&quot;Vereinigte Staaten&quot;], &quot;http://www.marketo.com&#39;); |
| Standort | location.state | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.state&#39;, [&#39;ca&#39;], &quot;http://www.marketo.com&#39;); |
| Standort | location.city | rtp( &#39;send&#39;, &#39;redirect&#39;, &#39;location.city&#39;, [&quot;San Mateo&quot;], &quot;http://www.marketo.com&#39;); |
| Branchen | Industrien | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;industry&#39; , [&quot;Bildung&quot;], &quot;http://www.marketo.com&#39;); |
| ISP | isp | rtp( &#39;send&#39;, &#39;redirect&#39;, isp , [&#39;False&#39;], &quot;http://www.marketo.com&#39;); |


## Hinweise

- Wenn die Umleitungsregel/-bedingung auf Firmographie (Firma, Branche, Standort) basiert, können Sie den Umleitungs-Code vor rtp(&#39;send&#39;, &#39;view&#39;) und rtp(&#39;get&#39;,&#39;campaign&#39;) einfügen, um die Latenz zu verringern.
- Die Umleitung über JavaScript ist eine browserseitige Umleitung und hängt vom Laden und Optimieren der Website ab, um die maximale Geschwindigkeit zu erreichen.
- Es empfiehlt sich, den Umleitungs-Code direkt nach dem rtp-Tag festzulegen und ihn in der Kopfzeile zu platzieren.
- Vergewissern Sie sich, dass Sie keine Selbstumleitung ausführen (es gibt ein Sicherheitsnetz in rtp, um zyklische Umleitungsanrufe zu blockieren).

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");
 
// START REDIRECT EXAMPLE 
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');
 
// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm'] 
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Weiterleiten von verfolgten Besuchern

1. Hängen Sie einen Parameter an das Ende der Ziel-URL an: z. B. www.marketo.com?rtp=redirect
1. Erstellen Sie ein Segment mit dem Namen &quot;Umgeleitet von RTP&quot;.
1. Verwenden Sie den Parameter &quot;Spezifische Seiten&quot;, um Besucher anzusprechen, die eine beliebige Seite mit dem folgenden Parameter anzeigen.

![tracking-redirect-vistors](assets/tracking-redirected-vistors.png)

## Definieren von mehr als einer Bedingung mit verschiedenen Ziel-URLs

Der Umleitungsaufruf unterstützt mehrere Aufrufe. Dies ermöglicht eine Umleitung mit mehreren Feldern und die Erstellung komplexer Bedingungen mit verschiedenen URLs und Werten.

### Nutzung

`rtp('send', 'redirect', field_name, url_values_map);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| &#39;send&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &quot;redirect&quot; | Erforderlich | Zeichenfolge | Name der Methode. |
| field_name | Erforderlich | Zeichenfolge | Feldname, mit dem eine Übereinstimmung gefunden werden soll. Beispiel: &quot;abm.name&quot;(siehe oben). |
| url_values_map | Erforderlich | Objekt | Zuordnung zwischen Umleitungs-URL und Werteliste. Beispiel:{&#39;http://marketo.com&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |


#### Beispiel

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
