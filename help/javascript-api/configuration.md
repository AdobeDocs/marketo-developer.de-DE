---
title: Konfiguration
description: Konfigurieren von Marketo Munchkin mit der JavaScript-API. Erfahren Sie mehr über Munchkin.init-Einstellungen wie altIds, anonymizeIP, asyncOnly, Cookie Life, domainLevel, Beacon-API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
TQID: https://experienceleague.adobe.com/ip2cCGgoa83v8m9GYLYXe132veYxS1C6UWX1iLB6X5Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 5%

---

# Konfiguration

Munchkin akzeptiert Konfigurationseinstellungen, die sein Verhalten anpassen. Übergeben Sie die Einstellungen als Eigenschaften eines JavaScript-Objekts im zweiten Parameter von [Munchkin.init()](api-reference.md#munchkin_init).

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

Das Konfigurationseinstellungsobjekt kann eine beliebige Anzahl von Eigenschaften in der folgenden Tabelle enthalten.

## Eigenschaften

| Name | Datentyp | Beschreibung |
| --- | --- | --- |
| altIds | Array | Akzeptiert ein Array von Munchkin ID-Zeichenfolgen. Wenn diese Option aktiviert ist, werden alle Web-Aktivitäten mit den Abonnements dupliziert, die durch ihre Munchkin-IDs identifiziert werden. |
| anonymizeIP | Boolesch | Anonymisiert die in Marketo aufgezeichnete IP-Adresse für neue Besucher. |
| apiOnly | Boolesch | Wenn auf „true“ gesetzt, ruft `Munchkin.Init()` Funktion `visitsWebPage` nicht auf. Dies ist für Single-Page-Web-Anwendungen nützlich, die die volle Kontrolle über jedes `visitsWebPage`-Ereignis benötigen. |
| asyncOnly | Boolesch | Wenn auf „true“ gesetzt, sendet XMLHttpRequests asynchron. Der Standardwert ist „false“. |
| clickTime | Ganzzahl | Legt die Zeit in Millisekunden fest, die nach einem Klick blockiert werden soll, damit die Klick-Tracking-Anfrage abgeschlossen werden kann. Wenn Sie diesen Wert verringern, wird die Genauigkeit des Klick-Trackings reduziert. Der Standardwert ist 350 ms. |
| cookieAnon | Boolesch | Wenn dieser Wert auf „false“ gesetzt ist, verhindert das Tracking und die Cookie-Erstellung für neue anonyme Leads. Leads erhalten Cookies und werden nach dem Senden eines Marketo-Formulars oder dem Klicken über eine Marketo-E-Mail verfolgt. Der Standardwert ist „true“. |
| cookieLifeDays | Ganzzahl | Legt das Ablaufdatum aller neu erstellten Munchkin-Tracking-Cookies auf diese Anzahl von Tagen in der Zukunft fest. Der Standardwert ist 730 Tage (2 Jahre). |
| customName | String | Name der benutzerdefinierten Seite. Nur Systembenutzung. |
| <a name="domainlevel"></a>domainLevel | Ganzzahl | Legt fest, wie viele Teile der Seiten-Domain für das Domain-Attribut des Cookies verwendet werden.<br><br>Für &quot;www.example.com&quot; setzt `domainLevel: 2` die Cookie-Domain auf &quot;.example.com“, und `domainLevel: 3` setzt sie auf &quot;.www.example.com&quot;.<br><br>Standardmäßig verwendet Munchkin zwei Teile, wenn die Domain der obersten Ebene drei Buchstaben hat. Beispiel: &quot;www.example.com&quot; verwendet &quot;.example.com“.<br><br>Für aus zwei Buchstaben bestehende Ländercodes wie &quot;.jp“, &quot;.us“, &quot;.cn“ und &quot;.uk“ verwendet Munchkin drei Teile. Beispielsweise verwendet &quot;www.example.co.jp&quot; &quot;.example.co.jp&quot;.<br><br>Verwenden Sie den Parameter &quot;`domainLevel`&quot;, wenn das Domain-Muster ein anderes Verhalten erfordert. |
| domainSelectorV2 | Boolesch | Bei Festlegung auf „true“ wird eine verbesserte Methode verwendet, um zu bestimmen, wie das Cookie-Domain-Attribut festgelegt wird. |
| httpsOnly | Boolesch | Die Standardeinstellung ist false. Wenn auf „true“ gesetzt, setzt Cookie auf die Verwendung der Einstellung „Sicher“, wenn die verfolgte Seite über HTTPS bereitgestellt wurde. |
| useBeaconAPI | Boolesch | Die Standardeinstellung ist false. Bei Festlegung auf „true“ verwendet [Beacon-API](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) um nicht blockierende Anfragen anstelle von &quot;[XMLHttpRequest](https://developer.mozilla.org/de-DE/docs/Web/API/XMLHttpRequest) zu senden. Wenn der Browser die Beacon-API nicht unterstützt, verwendet Munchkin XMLHttpRequest. |
| wsInfo | String | Targeting eines Arbeitsbereichs. Rufen Sie die Workspace-ID ab, indem Sie den Workspace im Menü Admin > Integration > Munchkin auswählen.<br><br>Diese Einstellung gilt nur, wenn zum ersten Mal ein anonymer Lead-Datensatz erstellt wird. Nachdem der Munchkin-Cookie-Wert für diesen Lead-Datensatz festgelegt wurde, kann der wsInfo-Parameter seine Partition nicht ändern.<br><br>Da diese Einstellung nur anonyme Leads betrifft, ist sie nur für partitionsspezifische (anonyme [&#x200B; in Web-Berichten) &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports). |

## Beispiele

### Aktivität an mehrere Abonnements senden

In diesem Beispiel wird die gesamte Web-Aktivität an die Instanzen mit den Munchkin-IDs „AAA-BBB-CCC“ und „XXX-YYY-ZZZ“ gesendet.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

### Festlegen des Trackings auf asynchron

In diesem Beispiel wird erzwungen, dass alle XMLHttpRequests asynchron vom Haupt-Thread gesendet werden.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
