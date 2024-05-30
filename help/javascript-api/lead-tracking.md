---
title: "Lead-Tracking"
description: "Lead-Tracking-API"
feature: Munchkin Tracking Code, Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# Lead-Tracking-API

Marketo Munchkin-JavaScript ermöglicht das Tracking von Seitenbesuchen und -klicks von Endbenutzern auf Ihren Marketo-Landingpages und externen Webseiten. Diese werden in Marketo als &quot;Webseite besuchen&quot;und &quot;Link auf Webseite angeklickt&quot;aufgezeichnet und können dann in Triggern und Filtern für Smart-Kampagnen und Smart-Listen verwendet werden.

## Einbetten des Codes

Ihre Marketo-Instanz stellt automatisch vorkonfigurierte Trackingcode-Snippets bereit, um Code auf Ihren externen Seiten einzubetten, die Aktivitäten auf Ihrer Marketo-Instanz verfolgen. Die Verwendung des Einbettungscodes unterliegt diesem [Lizenzvereinbarung](../munchkin-license.pdf).

Es stehen drei Trackingcode-Typen zur Verfügung:

1. Einfach - Lädt synchron
1. Asynchron - Asynchrone Ladevorgänge
1. Asynchrone jQuery - Wird asynchron geladen und erfordert, dass jQuery zuvor geladen wird

Es wird dringend empfohlen, den asynchronen Trackingcode zum Einbetten von Munchkin auf externen Seiten zu verwenden. Um die bestmögliche Erfolgsrate für die Ausführung sicherzustellen, betten Sie den asynchronen Trackingcode in `<head>` auf jeder Seite.

Einige Content-Management-Systeme können beim Einbetten beliebiger Skripte bestimmte Methoden oder Einschränkungen aufweisen.

Als Referenz sollte Ihre endgültige Seite Code enthalten, der diesem in ähnelt. `<head>` Ihres HTML-Dokuments:

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

Das Verhalten von Munchkin kann durch die Verwendung von Munchkin geändert werden [Konfigurationseinstellungen](lead-tracking.md#lead-tracking-api), z. B. ob ein Cookie für alle Leads beim Besuch der Seite mit der `cookieAnon` die Klickverzögerung durch `clickTime` -Einstellung. Das Senden der Besuchsaktivität kann deaktiviert werden, indem die Einstellung &quot;apiOnly&quot;auf &quot;true&quot;gesetzt wird. Ab Version 162 (August 2022) werden Klicks `tel` und `mailto` -Links werden zusätzlich zu `http/s` Links.

## Bekannte und anonyme Leads

Beim ersten Besuch eines Leads auf einer Seite Ihrer Domäne wird in Marketo ein neuer anonymer Lead-Datensatz erstellt. Der Primärschlüssel für diesen Datensatz ist das Munchkin-Cookie (`_mkto_trk`), die im Browser des Benutzers erstellt wird. Alle nachfolgenden Webaktivitäten in diesem Browser werden mit diesem anonymen Datensatz aufgezeichnet. Um mit einem bekannten Datensatz in Marketo verknüpft zu werden, muss eines der folgenden Ereignisse eintreten:

- Der Lead muss eine Munchkin-verfolgte Seite mit einer `mkt_tok` in der Abfragezeichenfolge eines verfolgten Marketo-E-Mail-Links.
- Der Lead muss ein Marketo-Formular ausfüllen.
- SOAP [syncLead](../soap-api/leads.md) oder REST [Lead verknüpfen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) -Aufruf muss gesendet werden.

Sobald eine dieser Bedingungen erfüllt ist, werden das Cookie und alle damit verbundenen Web-Aktivitäten mit dem bekannten Lead verknüpft.

Für jeden einzelnen Browser wird ein neuer anonymer Webaktivitätendatensatz erstellt. Wenn ein Lead Ihre Domäne also zum ersten Mal mit einem neuen Computer und/oder Browser besucht, muss diese Zuordnung erneut erfolgen.

## Domänen

Munchkin erstellt und verfolgt individuelle Cookies pro Domäne. Damit das bekannte Lead-Tracking domänenübergreifend erfolgt, muss für jede Domäne ein Lead-Verknüpfungsereignis auftreten. Wenn ich beispielsweise zwei Domänen beherrsche, `marketo.com`, und `example.com`und ein Lead ein Formular ausfüllt auf `marketo.com`, navigieren Sie dann zu `example.com` später wird ihre Aktivität auf `marketo.com` auf einem bekannten Lead-Datensatz verfolgt wird, deren Aktivität jedoch auf `example.com` ist anonym. Bekannte Leads bleiben jedoch über Subdomänen hinweg erhalten, sodass ein bekannter Lead auf `www.example.com` ist auch ein bekannter Lead bei `info.example.com`.

Wenn Ihre Domäne auf oberster Ebene aus zwei Teilen besteht, z. B. `.co.uk`und fügen Sie dann einen Parameter domainLevel zu Ihrem Munchkin-Snippet hinzu, damit der Code korrekt verfolgt wird. Siehe [here](lead-tracking.md#domains) für weitere Details.

## Cookie

Das Munchkin-Cookie verwendet den Schlüssel `_mkto_trk`und einen Wert hat, der diesem Muster folgt:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

Munchkin-Cookies sind für jede Domäne der zweiten Ebene spezifisch, d. h. `example.com`. Die Standardlebensdauer des Cookies beträgt 2 Jahre (730 Tage).

## Beta

Um sich für Ihre Landingpages am Kanal Munchkin Beta anzumelden, gehen Sie zu Ihrem [Admin -> Schatztruhe](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) und aktivieren Sie die Einstellung &quot;Munchkin Beta on Landing Pages&quot;. Dadurch werden neue Codefragmente im **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]** -Menü, damit Sie die Betaversion auf externen Sites verwenden können.

## Opt-out

Besucher können das Munchkin-Tracking vollständig deaktivieren, indem sie die `querystring` den Parameter &quot;marketo_opt_out=true&quot;zur URL in ihrem Browser hinzufügen. Wenn das Munchkin-JavaScript diese Einstellung erkennt, versucht es, ein neues Cookie &quot;mkto_opt_out&quot;mit dem Wert `true`. Alle anderen Marketo-Tracking-Cookies werden gelöscht, es werden keine neuen Cookies gesetzt und von Munchkin werden keine HTTP-Anfragen gesendet, wenn diese Einstellung erkannt wird.
