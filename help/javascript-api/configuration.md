---
title: Konfiguration
description: Verwenden Sie die Konfigurations-JavaScript-API , um Konfigurationswerte bei der Verwendung von Munchkin festzulegen.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 3%

---

# Konfiguration

Munchkin kann verschiedene Konfigurationseinstellungen akzeptieren, um das Verhalten anzupassen. Konfigurationseinstellungen sind Eigenschaften eines JavaScript-Objekts, das beim Aufruf von [Munchkin.init() als zweiter Parameter übergeben wird](api-reference.md#munchkin_init)

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
| altIds | Array | Akzeptiert ein Array von Munchkin ID-Zeichenfolgen. Wenn diese Option aktiviert ist, werden alle Web-Aktivitäten basierend auf ihrer Munchkin-ID mit den Zielabonnements dupliziert. |
| anonymizeIP | Boolesch | Anonymisiert die in Marketo aufgezeichnete IP-Adresse für neue Besucher. |
| apiOnly | Boolesch | Wenn auf „true“ gesetzt, ruft `Munchkin.Init()` Funktion `visitsWebPage` nicht auf. Dies ist für Single-Page-Web-Anwendungen nützlich, die die volle Kontrolle über jedes `visitsWebPage`-Ereignis benötigen. |
| asyncOnly | Boolesch | Wenn auf „true“ gesetzt, sendet asynchron die XMLHttpRequest. Der Standardwert ist „false“. |
| clickTime | Ganzzahl | Legt die Zeit in Millisekunden fest, die nach einem Klick blockiert werden soll, um die Klick-Tracking-Anforderung zu ermöglichen. Wenn Sie dies reduzieren, wird die Genauigkeit des Klick-Trackings reduziert. Der Standardwert ist 350 ms. |
| cookieAnon | Boolesch | Wenn dieser Wert auf „false“ gesetzt ist, verhindert das Tracking und die Cookie-Erstellung neuer anonymer Leads. Leads verfügen über Cookies und werden verfolgt, nachdem ein Marketo-Formular ausgefüllt oder eine Marketo-E-Mail durchgeklickt wurde. Der Standardwert ist „true“. |
| cookieLifeDays | Ganzzahl | Legt das Ablaufdatum aller neu erstellten Munchkin-Tracking-Cookies auf diese Anzahl von Tagen in der Zukunft fest. Der Standardwert ist 730 Tage (2 Jahre). |
| customName | String | Name der benutzerdefinierten Seite. Nur Systembenutzung. |
| <a name="domainlevel"></a>domainLevel | Ganzzahl | Legt die Anzahl der Teile aus der Domain der Seite fest, die beim Festlegen des Domänenattributs des Cookies verwendet werden sollen.Angenommen, die aktuelle Seitendomäne lautet &quot;www.example.com&quot;.domainLevel: 2 setzt das Cookie-Domänenattribut auf &quot;.example.com„domainLevel: 3 setzt das Cookie-Domänenattribut auf &quot;.www.example.com„Hintergrund:Munchkin verwaltet automatisch bestimmte Domains mit zwei Buchstaben auf oberster Ebene. Dies ist in normalen Fällen, in denen die Domain auf oberster Ebene aus drei Buchstaben besteht, standardmäßig zweiteilig. Zum Beispiel &quot;www.example.com&quot; werden die beiden ganz rechts liegenden Teile verwendet, um das Cookie &quot;.example.com“ festzulegen. Für zwei Buchstabenländercodes wie &quot;.jp“, &quot;.us“, &quot;.cn“ und &quot;.uk“ ist der Code standardmäßig dreiteilig. Beispiel: &quot;www.example.co.jp&quot; verwendet drei Teile der Domain, die sich ganz rechts befinden, &quot;.example.co.jp&quot;. Wenn das Domain-Muster ein anderes Verhalten erfordert, muss dies mit dem Parameter &quot;`domainLevel`&quot; angegeben werden. |
| domainSelectorV2 | Boolesch | Bei Festlegung auf „true“ wird eine verbesserte Methode verwendet, um zu bestimmen, wie das Cookie-Domain-Attribut festgelegt wird. |
| httpsOnly | Boolesch | Die Standardeinstellung ist false. Wenn auf „true“ gesetzt, setzt Cookie auf die Verwendung der Einstellung „Sicher“, wenn die verfolgte Seite über HTTPS bereitgestellt wurde. |
| useBeaconAPI | Boolesch | Die Standardeinstellung ist false. Bei Festlegung auf „true“ verwendet [Beacon-API](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) um nicht blockierende Anfragen anstelle von &quot;[XMLHttpRequest](https://developer.mozilla.org/de-DE/docs/Web/API/XMLHttpRequest) zu senden. Wenn der Browser diese API nicht unterstützt, greift der Munchkin auf die Verwendung von XMLHttpRequest zurück. |
| wsInfo | String | Nimmt eine Zeichenfolge als Ziel für einen Arbeitsbereich. Diese Workspace-ID erhalten Sie, indem Sie die Workspace im Menü Admin > Integration > Munchkin auswählen. Diese Einstellung gilt nur für die anfängliche Erstellung eines anonymen Lead-Datensatzes. Sobald der Munchkin-Cookie-Wert für diesen Lead-Datensatz festgelegt wurde, kann der Parameter „wsInfo“ nicht mehr verwendet werden, um die Partition zu ändern. Da diese Einstellung nur anonyme Leads betrifft, ist sie nur für partitionsspezifische (anonyme [ in Web-Berichten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) relevant. |

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

In diesem Beispiel wird erzwungen, dass alle XMLHttpRequest asynchron vom Haupt-Thread gesendet werden.

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
