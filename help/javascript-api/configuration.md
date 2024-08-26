---
title: Konfiguration
description: Verwenden Sie die Konfigurations-JavaScript-API, um bei Verwendung von Munchkin Konfigurationswerte festzulegen.
feature: Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: e609f9d5d58f656298412acef5e2106a19765396
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 3%

---

# Konfiguration

Munchkin kann verschiedene Konfigurationseinstellungen akzeptieren, um das Verhalten anzupassen. Konfigurationseinstellungen sind Eigenschaften eines JavaScript-Objekts, das beim Aufruf von [Munchkin.init()](lead-tracking.md#munchkin-behavior) als zweiter Parameter übergeben wird

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

Das Konfigurationseinstellungsobjekt kann eine beliebige Anzahl von Eigenschaften aus der folgenden Tabelle enthalten.

## Eigenschaften

| Name | Datentyp | Beschreibung |
|---|---|---|
| altIds | Array | Akzeptiert ein Array von Munchkin-ID-Zeichenfolgen. Wenn diese Option aktiviert ist, werden alle Web-Aktivitäten basierend auf ihrer Munchkin-ID in die zielgerichteten Abonnements dupliziert. |
| anonymizeIP | Boolesch | Die in Marketo aufgezeichnete IP-Adresse wird für neue Besucher anonymisiert. Sie können feststellen, ob Ihr Abonnement über Munchkin V2 verfügt, indem Sie überprüfen, ob Ihre `{Munchkin-Id}.mktoresp.com`-Domäne eine der folgenden Adressen hat: `192.28.144.124` `134.213.193.62` `192.28.147.68` `103.237.104.82`. Alternativ können Sie das unten stehende Skript von einer Unix-Shell aus ausführen: nslookup {munchkin-id}.mktoresp.com | grep -E -c -e &quot;(192.28.144.124,134.213.193.62,192.28.147.68,103.237.104.82)&quot; &quot;0&quot;ausgibt, wird Ihr Abonnement nicht mit Munchkin V2 bereitgestellt. Wenn es 1 oder höher ausgibt, wird es bereitgestellt. |
| apiOnly | Boolesch | Wenn der Wert auf &quot;true&quot;gesetzt ist, ruft die Funktion `Munchkin.Init()` `visitsWebPage` nicht auf. Dies ist nützlich für einseitige Webanwendungen, die vollständige Kontrolle über jedes `visitsWebPage` -Ereignis benötigen. |
| asyncOnly | Boolesch | Wenn der Wert auf &quot;true&quot;gesetzt ist, wird der asynchrone XMLHttpRequest gesendet. Der Standardwert ist &quot;false&quot;. |
| clickTime | Ganzzahl | Legt die Zeit fest, die nach einem Klick blockiert werden soll, um eine Klick-Tracking-Anfrage zu ermöglichen (in Millisekunden). Durch die Reduzierung wird die Genauigkeit des Klick-Trackings reduziert. Der Standardwert ist 350 ms. |
| cookieAnon | Boolesch | Wenn der Wert auf &quot;false&quot;gesetzt ist, verhindert das Tracking und die Erstellung von Cookies für neue anonyme Leads. Leads enthalten Cookies und werden nach dem Ausfüllen eines Marketo-Formulars oder durch Klicken auf eine Marketo-E-Mail verfolgt. Der Standardwert ist &quot;true&quot;. |
| cookieLifeDays | Ganzzahl | Legt das Ablaufdatum aller neu erstellten Munchkin-Tracking-Cookies auf diese vielen Tage in der Zukunft fest. Der Standardwert beträgt 730 Tage (2 Jahre). |
| customName | String | Benutzerdefinierter Seitenname. Nur Systemnutzung. |
| domainLevel | Ganzzahl | Legt die Anzahl der Teile aus der Domäne der Seite fest, die beim Festlegen des Domänenattributs des Cookies verwendet werden sollen. Angenommen, die aktuelle Seitendomäne lautet &quot;www.example.com&quot;.domainLevel: 2 setzt das Cookie-Domänenattribut auf &quot;.example.com&quot;domainLevel: 3 setzt das Cookie-Domänenattribut auf &quot;.www.example.com&quot;Hintergrund: Munchkin verwaltet automatisch bestimmte Domänen mit zwei Buchstaben auf oberster Ebene. Dies ist standardmäßig auf zwei Teile eingestellt, wenn die Domäne der obersten Ebene aus drei Buchstaben besteht. Beispielsweise werden die beiden am weitesten rechts gelegenen Teile &quot;www.example.com&quot;verwendet, um das Cookie &quot;.example.com&quot;zu setzen. Für zwei Buchstaben-Ländercodes wie &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot;und &quot;.uk&quot;ist der Code standardmäßig auf drei Teile eingestellt. Beispielsweise verwendet &quot;www.example.co.jp&quot;drei Teile der Domäne am weitesten rechts, &quot;.example.co.jp&quot;. Wenn das Domänenmuster ein anderes Verhalten erfordert, muss dies mithilfe des Parameters `domainLevel` angegeben werden. |
| domainSelectorV2 | Boolesch | Wenn der Wert auf &quot;true&quot;gesetzt ist, wird eine verbesserte Methode verwendet, um zu bestimmen, wie das Cookie-Domänenattribut festgelegt wird. |
| httpsOnly | Boolesch | Der Standardwert ist &quot;false&quot;. Wenn der Wert auf &quot;true&quot;gesetzt ist, setzt das Cookie auf die Verwendung der Einstellung &quot;Secure&quot;, wenn die verfolgte Seite über HTTPS bereitgestellt wurde. |
| useBeaconAPI | Boolesch | Der Standardwert ist &quot;false&quot;. Wenn der Wert auf &quot;true&quot;gesetzt ist, verwendet die Beacon-API zum Senden von nicht blockierenden Anfragen anstelle von XMLHttpRequest. Wenn der Browser diese API nicht unterstützt, greift der Munchkin auf die Verwendung von XMLHttpRequest zurück. |
| wsInfo | String | Nimmt eine Zeichenfolge zum Targeting eines Arbeitsbereichs vor. Diese Arbeitsbereichs-ID erhalten Sie, indem Sie die Workspace im Menü Admin > Integration > Munchkin auswählen. Diese Einstellung gilt nur für die anfängliche Erstellung eines anonymen Lead-Datensatzes. Sobald der Munchkin-Cookie-Wert für diesen Lead-Datensatz festgelegt wurde, kann der Parameter wsInfo nicht mehr verwendet werden, um seine Partition zu ändern. Da diese Einstellung nur anonyme Leads betrifft, ist sie nur für partitionsspezifische anonyme Besucher in Webberichten relevant. |

## Beispiele

### Aktivität an mehrere Abonnements senden

In diesem Beispiel werden alle Web-Aktivitäten an die Instanzen mit den Munchkin-IDs &quot;AAA-BBB-CCC&quot;und &quot;XXX-YYY-ZZZ&quot;gesendet.

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

### Festlegen der Verfolgung auf &quot;Asynchron&quot;

Dieses Beispiel erzwingt, dass alle XMLHttpRequests asynchron vom Haupt-Thread gesendet werden.

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
