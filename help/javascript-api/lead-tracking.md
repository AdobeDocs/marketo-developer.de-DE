---
title: Lead-Verfolgung
description: Lead-Tracking-API
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---

# Lead-Tracking-API

Marketo Munchkin JavaScript ermöglicht das Tracking von Seitenbesuchen und -klicks von Endbenutzern auf Ihren Marketo-Landingpages und externen Webseiten. Diese werden in Marketo als &quot;Webseite besuchen&quot;und &quot;Link auf Webseite angeklickt&quot;aufgezeichnet und können dann in Triggern und Filtern für Smart-Kampagnen und Smart-Listen verwendet werden.

## Einbetten des Codes

Ihre Marketo-Instanz stellt automatisch vorkonfigurierte Trackingcode-Snippets bereit, um Code auf Ihren externen Seiten einzubetten, die Aktivitäten auf Ihrer Marketo-Instanz verfolgen. Die Verwendung des Einbettungscodes wird durch diesen [Lizenzvertrag](../munchkin-license.pdf) geregelt.

Es stehen drei Trackingcode-Typen zur Verfügung:

1. Einfach - Lädt synchron
1. Asynchron - Asynchrone Ladevorgänge
1. Asynchrone jQuery - Wird asynchron geladen und erfordert, dass jQuery zuvor geladen wird

Es wird dringend empfohlen, den asynchronen Trackingcode zum Einbetten von Munchkin auf externen Seiten zu verwenden. Um die bestmögliche Erfolgsrate für die Ausführung sicherzustellen, betten Sie den asynchronen Trackingcode in `<head>` jeder Seite ein.

Einige Content-Management-Systeme können beim Einbetten beliebiger Skripte bestimmte Methoden oder Einschränkungen aufweisen.

Als Referenz sollte Ihre endgültige Seite Code enthalten, der in `<head>` Ihres HTML-Dokuments in etwa so aussieht:

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
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
    ...
</head>
```

## Munchkin-Verhalten

Das Standardverhalten von Marketo Munchkin besteht darin, beim Laden der Seite Folgendes zu tun:

1. Überprüfen Sie, ob der aktuelle Browser über ein Munchkin-Cookie verfügt, und erstellen Sie eines, falls es nicht vorhanden ist.
1. Senden Sie mit den Informationen aus der aktuellen Seite und dem Browser ein Ereignis &quot;Webseite besuchen&quot;an die vorgesehene Marketo-Instanz. Dadurch wird eine Aktivität zum entsprechenden Datensatz in Marketo aufgezeichnet.
1. Senden Sie das Ereignis &quot;geklickter Link auf Webseite&quot;für alle Benutzerklicks, die auf Links stattfinden.

Das Verhalten von Munchkin kann durch die Verwendung von Munchkin [Konfigurationseinstellungen](lead-tracking.md#lead-tracking-api) geändert werden, z. B. ob ein Cookie für alle Leads erstellt wird, wenn die Seite mit der Einstellung `cookieAnon` aufgerufen wird, oder die Klickverzögerung mit der Einstellung `clickTime` geändert wird. Das Senden der Besuchsaktivität kann deaktiviert werden, indem die Einstellung &quot;apiOnly&quot;auf &quot;true&quot;gesetzt wird. Ab Version 162 (August 2022) werden Klicks auf `tel` - und `mailto` -Links zusätzlich zu `http/s` -Links verfolgt.

## Bekannte und anonyme Leads

Beim ersten Besuch eines Leads auf einer Seite Ihrer Domäne wird in Marketo ein neuer anonymer Lead-Datensatz erstellt. Der Primärschlüssel für diesen Datensatz ist das Munchkin-Cookie (`_mkto_trk`), das im Browser des Benutzers erstellt wird. Alle nachfolgenden Webaktivitäten in diesem Browser werden mit diesem anonymen Datensatz aufgezeichnet. Um mit einem bekannten Datensatz in Marketo verknüpft zu werden, muss eines der folgenden Ereignisse eintreten:

- Der Lead muss eine Munchkin-verfolgte Seite mit dem Parameter `mkt_tok` in der Abfragezeichenfolge eines verfolgten Marketo-E-Mail-Links besuchen.
- Der Lead muss ein Marketo-Formular ausfüllen.
- Ein SOAP [syncLead](../soap-api/leads.md) - oder REST [Lead verknüpfen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) -Aufruf muss gesendet werden.

Sobald eine dieser Bedingungen erfüllt ist, werden das Cookie und alle damit verbundenen Web-Aktivitäten mit dem bekannten Lead verknüpft.

Für jeden einzelnen Browser wird ein neuer anonymer Webaktivitätendatensatz erstellt. Wenn ein Lead Ihre Domäne also zum ersten Mal mit einem neuen Computer und/oder Browser besucht, muss diese Zuordnung erneut erfolgen.

## Domänen

Munchkin erstellt und verfolgt individuelle Cookies pro Domäne. Damit das bekannte Lead-Tracking domänenübergreifend erfolgt, muss für jede Domäne ein Lead-Verknüpfungsereignis auftreten. Wenn ich beispielsweise zwei Domänen, `marketo.com` und `example.com` beherrsche und ein Lead ein Formular auf `marketo.com` ausfüllt, dann zu `example.com` später navigiert, wird seine Aktivität auf `marketo.com` in einem bekannten Lead-Datensatz nachverfolgt, aber seine Aktivität auf `example.com` ist anonym. Bekannte Leads bleiben jedoch über Subdomänen hinweg erhalten, sodass ein bekannter Lead unter `www.example.com` auch ein bekannter Lead unter `info.example.com` ist.

Wenn Ihre Domäne auf oberster Ebene aus zwei Teilen besteht, z. B. `.co.uk`, fügen Sie Ihrem Munchkin-Snippet einen Parameter domainLevel hinzu, damit der Code korrekt verfolgt wird. Weitere Informationen finden Sie unter [hier](lead-tracking.md#domains) .

## Cookie

Das Munchkin-Cookie verwendet den Schlüssel `_mkto_trk` und hat einen Wert, der diesem Muster folgt:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

Munchkin-Cookies sind für jede Domäne der zweiten Ebene spezifisch, d. h. `example.com`. Die Standardlebensdauer des Cookies beträgt 2 Jahre (730 Tage).

## Beta

Um sich für Ihre Landingpages am Munchkin-Betakanal zu beteiligen, gehen Sie zum Menü [Admin -> Schatztruhe](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) und aktivieren Sie die Einstellung &quot;Munchkin Beta on Landing Pages&quot;. Dadurch werden neue Code-Snippets in der **[!UICONTROL Admin]** -> bereitgestellt.  Menü **[!UICONTROL Munchkin]** , über das Sie die Betaversion auf externen Sites verwenden können.

## Opt-out

Besucher können das Munchkin-Tracking vollständig deaktivieren, indem sie den Parameter `querystring` &quot;marketo_opt_out=true&quot;zur URL in ihrem Browser hinzufügen. Wenn die Munchkin-JavaScript diese Einstellung erkennt, versucht sie, ein neues Cookie &quot;mkto_opt_out&quot;mit dem Wert `true` zu setzen. Alle anderen Marketo-Tracking-Cookies werden gelöscht, es werden keine neuen Cookies gesetzt und von Munchkin werden keine HTTP-Anfragen gesendet, wenn diese Einstellung erkannt wird.
