---
title: Rich-Media-Empfehlung
description: Rich-Media-Empfehlung mit Marketo Predictive Content RTP-Tag einrichten, template1 template2 template3 divs, GET zum Ausfüllen, SET zum Konfigurieren von Kategorien.
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
TQID: https://experienceleague.adobe.com/ygm5h1FJZZW4mC318-fRR3VAcO6j1sitcAeqIUjDTbI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 814
ht-degree: 5%

---

# Rich-Media-Empfehlung

Um eine Rich-Media-Empfehlungsvorlage anzuzeigen, fügen Sie der Seite die erforderlichen Tags und API-Aufrufe hinzu.

1. Im Seitenkopf:
   1. Installieren Sie das RTP-Tag.
   1. Fügen Sie den GET-Aufruf hinzu, mit dem die Empfehlungen ausgefüllt werden.
   1. Fügen Sie den SET-Aufruf hinzu, mit dem die Vorlage konfiguriert wird.
1. Im Hauptteil der Seite:
   1. Platzieren Sie das Vorlagen-Tag (div-Klasse) an die Stelle, an der die Vorlage angezeigt werden soll.

Weitere Informationen finden Sie unter [Aktivieren prädiktiver Inhalte für Web-Rich-Media](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Vorlagen-Tag

| Attribut | Optional/Erforderlich | Beschreibung |
| --- | --- | --- |
| klasse | Erforderlich | Identifiziert das div-HTML-Element als RTP-Recommendations-div. |
| data-rtp-template-id | Erforderlich | Bestimmt die Ausrichtung der Empfehlung. Verwenden Sie „template1“ für die horizontale Ausrichtung, „template2“ für die vertikale Ausrichtung oder „template3“ für die vertikale Ausrichtung mit nur einem Titel und einer Beschreibung. Das Skript fügt die entsprechende Vorlage in dieses `div` ein. Zulässige Werte: template1, template2, template3. |

### Beispiele

Verwenden Sie „template1“, um Empfehlungen horizontal anzuzeigen.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Verwenden Sie „template2“, um Empfehlungen vertikal anzuzeigen.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Verwenden Sie „template3“, um Empfehlungen vertikal nur mit einem Titel und einer Beschreibung anzuzeigen.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Siehe [Beispiele für die Vorlagenausrichtung](#example_of_rich_media_recommendation_template_1).

## Empfehlung befüllen

Diese Methode füllt alle Rich-Media-`<divs>` auf der Seite mit Empfehlungen.

### Nutzung

`rtp('get', 'rcmd', 'richmedia');`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;GET&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;rcmd&#39; | Erforderlich | String | Methodenname. |
| &#39;richmedia&#39; | Erforderlich | String | Name der Untermethode. |

## Vorlagenkonfiguration ändern

Diese Methode ändert die Standardvorlagenkonfiguration.

Rufen Sie diese Methode vor dem Aufruf von rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Nutzung

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;Satz&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;rcmd&#39; | Erforderlich | String | Methodenname. |
| &#39;richmedia&#39; | Erforderlich | String | Name der Untermethode. |
| template_id | Optional | String | Die Vorlagen-ID für Konfigurationsänderungen. Verwenden Sie , um Änderungen der Einstellungen nur für eine Vorlage anzugeben. |
| conf_obj | Erforderlich | Objekt | Die neue Konfiguration. Das -Objekt enthält alle Konfigurationen als Schlüssel-Wert-Paar. |

### Beispiele

In diesem Beispiel wird der Titeltext für eine Vorlage geändert.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

In diesem Beispiel werden Kategorien und mehrere Konfigurationseigenschaften für eine Vorlage festgelegt.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial",
            "category":
            [
                "webinar",
                "blog posts",
                "pricing_page_category",
                "product_a_category"
            ]
        }
    }
);
```

Verwenden Sie „Kategorie“, um den in prädiktiven Inhaltsempfehlungen angezeigten Inhalt zu filtern. Um prädiktive Inhalte für alle aktivierten Inhalte zu verwenden, lassen Sie „Kategorie“ leer.

Um nur bestimmte Inhalte in der Rich-Media-Vorlage zu empfehlen, fügen Sie eine Kategorie für den Inhalt auf der Seite Inhalt festlegen hinzu. Verknüpfen Sie dann diese Kategorie mit dem Code der Empfehlungsvorlage. Kategorisieren Sie beispielsweise relevante Inhalte anhand der Produkt- oder Lösungsabschnitte Ihrer Website.

In diesem Beispiel werden mehrere Konfigurationseigenschaften für eine Vorlage festgelegt.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial"
        }
    }
);
```

#### Konfigurationseigenschaften

| Konfiguration | Beispiel | Beschreibung |
| --- | --- | --- |
| rcmd.general.font.family | „rcmd.general.font.family“ : „Arial“ | Ändert die Schriftfamilie für den gesamten Text in der Vorlage. Diese Eigenschaft unterstützt alle CSS-Werte nach Browser-Typ. Es ist möglich, eine benutzerdefinierte Schriftfamilie zu verwenden, wenn sie auf der Seite vorhanden ist. |
| rcmd.content.background.color | „rcmd.content.background.color“ : „Schwarz“ | Ändert die Hintergrundfarbe der inneren Vorlagenfelder. Diese Eigenschaft unterstützt alle CSS-Werte nach Browser-Typ. |
| rcmd.title.text | „rcmd.title.text“ : „EMPFOHLENER INHALT“ | Ändert den Vorlagentitel. |
| rcmd.title.background.color | „rcmd.title.background.color“ : „blue“ | Ändert die Hintergrundfarbe des Titelfelds. Diese Eigenschaft unterstützt alle CSS-Farbwerte (Farbname, RGB usw.) |
| rcmd.title.font.size | „rcmd.title.font.size“ : „26px“ | Ändert die Schriftgröße des Titels. Die -Eigenschaft unterstützt alle möglichen CSS-Schriftgrößen (px, em, …) |
| rcmd.title.font.color | „rcmd.title.font.color“ : „Weiß“ | Ändert die Schriftfarbe des Titels. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, …) |
| rcmd.description.font.color | „rcmd.description.font.color“ : „Weiß“ | Ändert die Schriftfarbe der Beschreibung. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, …) |
| rcmd.cta.background.color | „rcmd.cta.background.color“ : „grün“ | Ändert die Hintergrundfarbe der Schaltfläche. Diese Eigenschaft unterstützt den gesamten CSS-Farbwert (Farbname, RGB usw.) |
| rcmd.cta.font.color | „rcmd.cta.font.color“ : „RGB(90, 84, 164)“ | Ändert die Schriftfarbe der Schaltfläche. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, …) |
| rcmd.cta.text | „rcmd.cta.text“ : „Push“ | Ändert den Schaltflächentext. Der Text ist für alle Schaltflächen identisch. |
| Kategorie | „category“ : [ „one category“] | Ändert die Empfehlungskategorie, die diese Vorlage unterstützt. Die Vorlage zeigt nur Empfehlungen mit einer der von dieser Konfiguration festgelegten Kategorien an. |

Die Konfigurationsunterstützung kann je nach Vorlage variieren.

#### Einfaches Beispiel

In diesem Beispiel werden drei Empfehlungen in einer Vorlage angezeigt. Kopieren Sie das Beispiel in eine HTML-Seite und ersetzen Sie dann das RTP-Tag durch Ihr -Tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Erweitertes Beispiel

In diesem Beispiel werden drei Empfehlungen in einer Vorlage angezeigt. Der Vorlagentitel lautet „RECOMMENDED CONTENT“ und der Schaltflächentext lautet „Read More“. Kopieren Sie das Beispiel in eine HTML-Seite und ersetzen Sie dann das RTP-Tag durch Ihr -Tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate the recommendation zone
rtp('get', 'campaign',true);
// Change template configuration
rtp('set', 'rcmd', 'richmedia',
    {
        template1 :
        {
            "rcmd.title.text" : "RECOMMENDED CONTENT",
            "rcmd.cta.text" : "Read More"
        }
    }
);
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Beispiel für #1 der Rich-Media-Empfehlungsvorlage

**Name**: template1

**Beschreibung**: Horizontaler Inhalt, der ein Bild, einen Titel, eine Beschreibung und eine call-to-action-Schaltfläche enthält.

![Rich-Media-Vorlage](assets/rich-media-template1.png)

#### Beispiel für #2 der Rich-Media-Empfehlungsvorlage

**Name**: template2

**Beschreibung**: Vertikaler Inhalt, der ein Bild, einen Titel, eine Beschreibung und eine call-to-action-Schaltfläche enthält.

![Rich-Media-Vorlage](assets/rich-media-template2.png)

#### Beispiel für #3 der Rich-Media-Empfehlungsvorlage

**Name**: template3

**Beschreibung**: Vertikaler Inhalt, der nur einen Titel und eine Beschreibung enthält. Beim Bewegen des Mauszeigers ändert sich die Farbe der Kopfzeile und die Links zur Inhalts-URL. Die Beschreibung verweist auch auf den Inhalt, ohne die Farbe zu ändern.

![Rich-Media-Vorlage](assets/rich-media-template3.png)
