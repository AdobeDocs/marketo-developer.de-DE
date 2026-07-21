---
title: Umleiten
description: Implementieren Sie die RTP-Redirect-API, um segmentierte Besucher mithilfe von Feldern wie ABM, Organisation, Standort und Segmenten mit Beispielen und Tipps an zielgerichtete URLs zu senden.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
TQID: https://experienceleague.adobe.com/frvGjN7DBJ1RJ3QFvWxo1qGiTNFmvyxi3H6FeynJHLU
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 8%

---

# Umleiten

Verwenden Sie die RTP Redirect-API, um segmentierte Zielgruppen an eine Ziel-URL zu senden.

- Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.
- RTP unterstützt keine Listen mit Account-basierten Marketing-Konten. ABM-Listen und Code beziehen sich nur auf die hochgeladenen Kontolisten (CSV-Dateien), die in RTP verwaltet werden.

## Nutzung

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;Senden&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;Umleiten&#39; | Erforderlich | String | Methodenname. |
| field_name | Erforderlich | String | Feldname, mit dem abgeglichen werden soll. Beispiel: „abm.name“ (siehe unten). |
| values_array | Erforderlich | Array | Liste der Werte, mit denen das Feld abgeglichen werden soll (Groß-/Kleinschreibung wird nicht beachtet). |
| redirect_url | Erforderlich | String | Ziel-URL für die Umleitung von Besuchern, die der Bedingung entsprachen. |
| redirect_matches_visitors | Optional | Boolesch | Wenn „true“, werden Besucher, die mit der Bedingung übereinstimmen, umgeleitet. Wenn die Bedingung „false“ ist, werden nicht übereinstimmende Besucher umgeleitet. Standard: true. |

Umleitungsbedingungen können Organisation, Branche, ABM-Listen, Standort, ISP oder übereinstimmende Segmente verwenden.

| Bedingung | Datenhierarchie | Beispiel |
| --- | --- | --- |
| Übereinstimmende Segmente (funktioniert nur nach dem ersten Klick) | matchSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchesSegments.name&#39; , [&#39;Fortune 1,000&#39; , &#39;Enterprise&#39;] , &#39;<https://www.example.com>&#39;); |
| Übereinstimmende Segmente (funktioniert nur nach dem ersten Klick) | matchSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchesSegments.id&#39; , [106 , 107 , 190] , &#39;<https://www.example.com>&#39;); |
| ABM-Listen | abm.name | rtp(&#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_customers&#39;] , &#39;<https://www.example.com>&#39;); |
| ABM-Listen | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &#39;<https://www.example.com>&#39;); |
| Organisationen | org | rtp(&#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;<https://www.example.com>&#39;); |
| Standort | location.country | rtp(&#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; [&#39;United States&#39;], &#39;<https://www.example.com>&#39;); |
| Standort | location.state | rtp(&#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;<https://www.example.com>&#39;); |
| Standort | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<https://www.example.com>&#39;); |
| Branchen | Branchen | rtp(&#39;send&#39;, &#39;redirect&#39; , &#39;industries&#39; , [&#39;education&#39;], &#39;<https://www.example.com>&#39;); |
| ISP | ISP | rtp(&#39;send&#39;, &#39;redirect&#39; , isp , [&#39;false&#39;], &#39;<https://www.example.com>&#39;); |

## Hinweise

- Um die Latenz für eine Weiterleitung auf der Grundlage von Firmografien wie Unternehmen, Branche oder Standort zu reduzieren, fügen Sie den Weiterleitungs-Code vor rtp(&#39;Senden&#39;, &#39;Anzeigen&#39;) und rtp(&#39;GET&#39;, &#39;Kampagne&#39;) ein.
- Platzieren Sie den Umleitungs-Code unmittelbar nach dem rtp-Tag in der Kopfzeile der Seite.
- Optimieren Sie das Laden von Websites, um die Browser-seitige JavaScript-Umleitung zu beschleunigen.
- Vermeiden Sie Selbstumleitungen. RTP umfasst einen Sicherheitsmechanismus, der zyklische Umleitungsaufrufe blockiert.

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

## Weiterleiten getrackter Besucher

1. Hängen Sie den Parameter an die Ziel-URL an, z. B. &lt;www.marketo.com?rtp=redirect>.
1. Erstellen Sie ein Segment mit dem Namen „Redirect by RTP“.
1. Verwenden Sie den Parameter „Spezifische Seiten“, um Besuchende anzusprechen, die eine Seite mit dem Parameter aufrufen.

![tracking-redirected-Visitors](assets/tracking-redirected-vistors.png)

## Definieren von mehr als einer Bedingung mit verschiedenen Ziel-URLs

Der Umleitungsaufruf unterstützt mehrere Aufrufe. Verwenden Sie mehrere Aufrufe, um Felder zu kombinieren und Bedingungen mit unterschiedlichen URLs und Werten zu erstellen.

### Nutzung

`rtp('send', 'redirect', field_name, url_values_map);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;Senden&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;Umleiten&#39; | Erforderlich | String | Methodenname. |
| field_name | Erforderlich | String | Feldname, mit dem abgeglichen werden soll. Beispiel: „abm.name“ (siehe oben). |
| url_values_map | Erforderlich | Objekt | Zuordnung zwischen Umleitungs-URL und Werteliste. Beispiel:{&#39;<https://www.example.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

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
