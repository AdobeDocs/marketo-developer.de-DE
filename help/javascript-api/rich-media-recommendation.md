---
title: "Rich-Media-Empfehlung"
description: "Rich-Media-Empfehlung"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 4%

---


# Rich-Media-Empfehlung

Die folgenden Tags und API-Aufrufe müssen auf der Seite eingerichtet werden, auf der die Vorlage für Rich-Media-Empfehlungen angezeigt werden soll.

1. In der Kopfzeile der Seite
   1. RTP-Tag installieren
   1. Fügen Sie den GET-Aufruf zur Seite hinzu, um die Empfehlungen zu füllen.
   1. Fügen Sie den SET-Aufruf hinzu, um die Vorlage zu konfigurieren
1. Im Seitentext
   1. Platzieren Sie das Vorlagen-Tag (div-Klasse) an der Stelle, an der die Vorlage angezeigt werden soll

Weitere Informationen finden Sie unter [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Vorlagentag

| Attribut | Optional/Erforderlich | Beschreibung |
|---|---|---|
| klasse | Erforderlich | Geben Sie an, dass dieses div-HTML-Element RTP Recommendation div ist. |
| data-rtp-template-id | Erforderlich | Die Vorlagen-ID. Dies bestimmt die Ausrichtung Ihrer Empfehlung. Verwenden Sie &quot;template1&quot;für die horizontale Ausrichtung, &quot;template2&quot;für die vertikale Ausrichtung oder &quot;template3&quot; für die vertikale Ausrichtung, die nur Titel und Beschreibung enthält. Das Skript fügt die entsprechende Vorlage ein `div.Permissible` Werte: template1, template2, template3. |

### Beispiele

Um Ihre Empfehlungen in horizontaler Ausrichtung anzuzeigen, verwenden Sie &quot;template1&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Um Ihre Empfehlungen in vertikaler Ausrichtung anzuzeigen, verwenden Sie &quot;template2&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Um Ihre Empfehlungen nur in vertikaler Ausrichtung mit Titel und Beschreibung anzuzeigen, verwenden Sie &quot;template3&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Siehe Screenshots von Vorlagenausrichtungen [here](#example_of_rich_media_recommendation_template_1).

## Empfehlung ausfüllen

Diese Methode füllt alle Rich-Media-Daten `<divs>` auf der Seite mit Empfehlungen.

### Nutzung

`rtp('get', 'rcmd', 'richmedia');`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| &#39;get&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &quot;rcmd&quot; | Erforderlich | Zeichenfolge | Name der Methode. |
| &quot;richmedia&quot; | Erforderlich | Zeichenfolge | Name der Untermethode. |


## Vorlagenkonfiguration ändern

Diese Methode ändert die Standardkonfiguration für die Vorlage.

Hinweis: Bei Verwendung dieser Methode muss sie vor dem Aufruf von rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;) aufgerufen werden.

### Nutzung

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| &#39;set&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &quot;rcmd&quot; | Erforderlich | Zeichenfolge | Name der Methode. |
| &quot;richmedia&quot; | Erforderlich | Zeichenfolge | Name der Untermethode. |
| template_id | optional | Zeichenfolge | Die Vorlagen-ID für Konfigurationsänderungen. Verwenden Sie , um die Änderung der Einstellungen nur für eine Vorlage festzulegen. |
| conf_obj | Erforderlich | Objekt | Die neue Konfiguration. Das -Objekt enthält alle Konfigurationen als Schlüssel/Wert-Paar. |


### Beispiele

Dieses Codefragment ändert den Titeltext für eine Vorlage.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Dieses Codefragment zeigt das Festlegen von Kategorien mit mehreren Konfigurationen für eine Vorlage.

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

HINWEIS: Verwenden Sie &quot;Kategorie&quot;, um Inhalte zu filtern, die im Ergebnis der Empfehlungen für prädiktive Inhalte angezeigt werden. Lassen Sie die &quot;Kategorie&quot;leer, wenn Sie vorausschauenden Inhalt auf alle aktivierten Inhaltselemente anwenden möchten. Wenn Sie in der Rich-Media-Vorlage nur bestimmte Inhalte für die Ausgabe empfehlen möchten, fügen Sie auf der Seite Inhalt festlegen eine Kategorie für den Inhalt hinzu und verknüpfen Sie diese Kategorie im Empfehlungsvorlagencode. Kategorisierung relevanter Inhalte entsprechend den Abschnitten Ihrer Website (Produkte oder Lösungen).

Dieses Codefragment zeigt das Festlegen mehrerer Vorlagenkonfigurationen für eine Vorlage.

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
|---|---|---|
| rcmd.general.font.family | &quot;rcmd.general.font.family&quot; : &quot;arial&quot; | Ändert die Schriftfamilie für den gesamten Text in der Vorlage. Diese Eigenschaft unterstützt alle CSS-Werte nach Browsertyp. Es ist möglich, eine benutzerdefinierte Schriftfamilie zu verwenden, wenn sie auf der Seite vorhanden ist. |
| rcmd.content.background.color | &quot;rcmd.content.background.color&quot; : &quot;black&quot; | Ändert die Hintergrundfarbe der inneren Vorlagenfelder. Diese Eigenschaft unterstützt alle CSS-Werte nach Browsertyp. |
| rcmd.title.text | &quot;rcmd.title.text&quot;: &quot;EMPFOHLENER INHALT&quot; | Ändert den Vorlagentitel. |
| rcmd.title.background.color | &quot;rcmd.title.background.color&quot; : &quot;blue&quot; | Ändert die Hintergrundfarbe des Titelfelds. Diese Eigenschaft unterstützt alle CSS-Farbwerte (Farbname, RGB, ...) |
| rcmd.title.font.size | &quot;rcmd.title.font.size&quot;: &quot;26px&quot; | Ändert die Schriftgröße des Titels. Die -Eigenschaft unterstützt alle CSS-Schriftgrößen (px, em, ...) |
| rcmd.title.font.color | &quot;rcmd.title.font.color&quot; : &quot;weiß&quot; | Ändert die Schriftfarbe des Titels. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, ...) |
| rcmd.description.font.color | &quot;rcmd.description.font.color&quot; : &quot;weiß&quot; | Ändert die Schriftfarbe der Beschreibung. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, ...) |
| rcmd.cta.background.color | &quot;rcmd.cta.background.color&quot; : &quot;green&quot; | Ändert die Hintergrundfarbe der Schaltfläche. Diese Eigenschaft unterstützt alle CSS-Farbwerte (Farbname, RGB, ...) |
| rcmd.cta.font.color | &quot;rcmd.cta.font.color&quot; : &quot;rgb(90, 84, 164)&quot; | Ändert die Schriftfarbe der Schaltfläche. Diese Eigenschaft unterstützt alle Schriftfarbwerte (rgb, hex, ...) |
| rcmd.cta.text | &quot;rcmd.cta.text&quot; : &quot;Push&quot; | Ändert den Schaltflächentext. Der Text ist für alle Schaltflächen identisch. |
| Kategorie | &quot;category&quot; : [&quot;eine Kategorie&quot;] | Ändert die Empfehlungskategorie, die diese Vorlage unterstützt. Die Vorlage zeigt nur Empfehlungen mit einer der von dieser Konfiguration festgelegten Kategorien an. |


Hinweis: Die Konfigurationsunterstützung kann sich je nach Vorlage ändern.

#### Grundlegendes Beispiel

Dieses Beispiel enthält eine Vorlage mit drei Empfehlungen. Kopieren Sie dieses Beispiel in eine HTML-Seite und ersetzen Sie dann das RTP-Tag durch Ihr -Tag.

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

Dieses Beispiel enthält eine Vorlage mit drei Empfehlungen. Der Vorlagentitel ist &quot;EMPFOHLENER INHALT&quot;und der Schaltflächentext lautet &quot;Mehr lesen&quot;. Kopieren Sie dieses Beispiel in eine HTML-Seite und ersetzen Sie dann das RTP-Tag durch Ihr -Tag.

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

#### Beispiel einer Rich-Media-Empfehlungsvorlage Nr. 1

**Name**: template1 **Beschreibung**: Horizontaler Inhalt einschließlich Bild, Titel und Beschreibung und Aktionsschaltfläche.

![Rich-Media-Vorlage](assets/rich-media-template1.png)

#### Beispiel einer Rich-Media-Empfehlungsvorlage Nr. 2

**Name**: template2 **Beschreibung**: Vertikaler Inhalt einschließlich Bild, Titel und Beschreibung und Aktionsschaltfläche .

![Rich-Media-Vorlage](assets/rich-media-template2.png)

#### Beispiel einer Rich-Media-Empfehlungsvorlage Nr. 3

**Name**: template3 **Beschreibung**: Vertikaler Inhalt mit nur Titel und Beschreibung. Wenn Sie den Mauszeiger darüber bewegen, ändert sich die Farbe der Kopfzeile und ist mit der Inhalts-URL verlinkt. Beschreibung enthält auch Links zu Inhalten ohne Farbänderung. ![Rich-Media-Vorlage](assets/rich-media-template3.png)
