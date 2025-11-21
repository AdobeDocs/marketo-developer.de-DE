---
title: Lead-Verfolgung
description: Erfahren Sie, wie Sie Marketo Munchkin JavaScript einbetten, Besuche und Klicks verfolgen, bekannte oder anonyme Leads verwalten, domänenübergreifende Cookies verwenden und sich gegen intelligente Kampagnen entscheiden.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: c1b9763835b25584f0c085274766b68ddf5c7ae2
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 0%

---

# Lead-Tracking-API

Marketo Munchkin JavaScript ermöglicht das Tracking von Endbenutzer-Seitenbesuchen und -Klicks auf Ihre Marketo-Landingpages und externen Web-Seiten. Diese werden in Marketo als Aktivitäten des Typs „Web-Seite besuchen“ und „Auf Link auf Web-Seite klicken“ aufgezeichnet, die dann in Triggern und Filtern für Smart-Kampagnen und Smart-Listen verwendet werden können.

## Einbetten des Codes

Ihre Marketo-Instanz stellt automatisch vorkonfigurierte Trackingcode-Snippets bereit, um Code auf Ihren externen Seiten einzubetten und so Aktivitäten zurück zu Ihrer Marketo-Instanz zu verfolgen. Die Verwendung des Einbettungs-Codes wird durch diese [Lizenzvereinbarung](../munchkin-license.pdf) geregelt.

Es stehen drei Trackingcode-Typen zur Verfügung:

1. Einfach - Lädt synchron
1. Asynchron - Lädt asynchron
1. Asynchrone jQuery - Lädt asynchron und erfordert das vorherige Laden von jQuery

Es wird dringend empfohlen, den asynchronen Trackingcode zum Einbetten von Munchkin auf externen Seiten zu verwenden. Um eine möglichst hohe Erfolgsrate für die Ausführung sicherzustellen, betten Sie den asynchronen Trackingcode in `<head>` jeder Seite ein.

Einige Content-Management-Systeme verfügen möglicherweise über bestimmte Methoden oder Einschränkungen beim Einbetten beliebiger Skripte.

Als Referenz sollte Ihre letzte Seite Code enthalten, der dem in `<head>` Ihres HTML-Dokuments ähnelt:

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

Das Standardverhalten von Marketo Munchkin besteht darin, beim Laden der Seite Folgendes auszuführen:

1. Überprüfen Sie, ob der aktuelle Browser über ein Munchkin-Cookie verfügt, und erstellen Sie ein Cookie, wenn es nicht vorhanden ist.
1. Senden Sie mit den Informationen auf der aktuellen Seite und im aktuellen Browser ein „Web-Seite besuchen“-Ereignis an die vorgesehene Marketo-Instanz. Dadurch wird eine Aktivität auf den entsprechenden Datensatz in Marketo aufgezeichnet.
1. Senden Sie das Ereignis „Auf Link auf Web-Seite geklickt“ für alle Benutzerklicks auf Links.

Das Verhalten von Munchkin kann durch die Verwendung von Munchkin [Konfigurationseinstellungen](configuration.md) geändert werden, z. B. ob beim Besuch der Seite mit der `cookieAnon` ein Cookie für alle Leads erstellt wird oder durch Änderung der Klickverzögerung mit `clickTime` Einstellung. Das Senden der Aktivität Besuch kann deaktiviert werden, indem die Einstellung „apiOnly“ auf „true“ gesetzt wird. Ab Version 162 (August 2022) werden neben `tel` Links auch Klicks `mailto` und `http/s` Links verfolgt.

## Bekannte und anonyme Leads

Beim ersten Besuch eines Leads auf einer Seite Ihrer Domain wird in Marketo ein neuer anonymer Lead-Datensatz erstellt. Der Primärschlüssel für diesen Datensatz ist das Munchkin-Cookie (`_mkto_trk`), das im Browser des Benutzers erstellt wird. Alle nachfolgenden Web-Aktivitäten in diesem Browser werden für diesen anonymen Datensatz aufgezeichnet. Um mit einem bekannten Datensatz in Marketo verknüpft zu sein, muss eines der folgenden Ereignisse eintreten:

- Der Lead muss eine von Munchkin verfolgte Seite mit einem `mkt_tok` in der Abfragezeichenfolge über einen verfolgten Marketo-E-Mail-Link besuchen.
- Der Lead muss ein Marketo-Formular ausfüllen.
- Ein REST[Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST)-Aufruf muss gesendet werden.

Wenn eine dieser Bedingungen erfüllt ist, werden das Cookie und alle zugehörigen Web-Aktivitäten mit dem bekannten Lead verknüpft.

Für jeden einzelnen Browser wird ein neuer anonymer Webaktivitätseintrag erstellt. Wenn also ein Lead zum ersten Mal mit einem neuen Computer und/oder Browser Ihre Domain besucht, muss diese Zuordnung erneut stattfinden.

## Domains

Munchkin erstellt und verfolgt einzelne Cookies domänenweise. Damit das bekannte Lead-Tracking domänenübergreifend erfolgt, muss für jede Domain ein Lead-Zuordnungsereignis vorliegen. Wenn ich beispielsweise zwei Domains, `marketo.com` und `example.com`, kontrolliere und ein Lead ein Formular bei `marketo.com` ausfüllt und dann zu `example.com` navigiert, wird seine Aktivität bei `marketo.com` in einem bekannten Lead-Eintrag verfolgt, aber seine Aktivität bei `example.com` ist anonym. Bekannte Leads bleiben jedoch über Subdomains hinweg bestehen, sodass ein bekannter Lead auf `www.example.com` auch ein bekannter Lead auf `info.example.com` ist.

Falls Ihre Domain auf oberster Ebene aus zwei Teilen besteht, z. B. `.co.uk`, fügen Sie Ihrem Munchkin-Snippet einen domainLevel-Parameter hinzu, damit der Code ordnungsgemäß verfolgt wird. Weitere Informationen [&#x200B; Sie &#x200B;](configuration.md#domainlevel)hier).

## Cookie

Das Munchkin-Cookie verwendet die `_mkto_trk` und hat einen Wert, der diesem Muster folgt:

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

Or

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

Munchkin-Cookies sind spezifisch für jede Domain der zweiten Ebene, d. h. `example.com`. Die Standardlebensdauer des Cookies beträgt 2 Jahre (730 Tage).

## Beta

Um sich für Ihre Landingpages für den Munchkin Beta-Kanal zu registrieren, gehen Sie zum Menü [Admin -> Schatztruhe](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) und aktivieren Sie die Einstellung &quot;Munchkin Beta auf Landingpages“. Dadurch werden neue Code-Snippets in der Datei **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]**-Menü, über das Sie die Betaversion auf externen Sites verwenden können.

## Opt-out

Besucher können das Munchkin-Tracking vollständig deaktivieren, indem sie den `querystring` „marketo_opt_out=true“ zur URL im Browser hinzufügen. Wenn Munchkin JavaScript diese Einstellung erkennt, versucht es, ein neues Cookie „mkto_opt_out“ mit dem Wert `true` zu setzen. Alle anderen Tracking-Cookies von Marketo werden gelöscht, neue Cookies werden nicht gesetzt und von Munchkin werden keine HTTP-Anfragen gesendet, wenn diese Einstellung erkannt wird.
