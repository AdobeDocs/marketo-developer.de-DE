---
title: Lead-Verfolgung
description: Erfahren Sie, wie Sie Marketo Munchkin JavaScript einbetten, Besuche und Klicks verfolgen, bekannte oder anonyme Leads verwalten, domänenübergreifende Cookies verwenden und sich gegen intelligente Kampagnen entscheiden.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
TQID: https://experienceleague.adobe.com/nGUcLLgL9X7PBKf2E5IzppDj8e-SyEtxmkQaESd90mE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 720
ht-degree: 0%

---

# Lead-Tracking-API

Marketos Munchkin JavaScript verfolgt Seitenbesuche und Link-Klicks auf Marketo-Landingpages und externen Webseiten. Marketo zeichnet diese Interaktionen als Aktivitäten „Web-Seite besuchen“ und „Auf Web-Seite geklickt“ auf.

Verwenden Sie die Aktivitäten in Triggern und Filtern für Smart-Kampagnen und Smart-Listen.

## Einbetten des Codes

Ihre Marketo-Instanz stellt vorkonfigurierte Code-Snippets zum Tracking von Aktivitäten von externen Seiten bereit. Die Verwendung des Einbettungs-Codes wird durch diese [Lizenzvereinbarung](../munchkin-license.pdf) geregelt.

Es stehen drei Trackingcode-Typen zur Verfügung:

1. Einfach - Lädt synchron.
1. asynchron - Lädt asynchron.
1. Asynchrone jQuery - Lädt asynchron und erfordert, dass jQuery zuerst geladen wird.

Verwenden Sie den asynchronen Trackingcode, um Munchkin auf externen Seiten einzubetten. Um eine möglichst hohe Ausführungserfolgsrate zu erzielen, platzieren Sie den Code im `<head>`-Element jeder Seite.

Einige Content-Management-Systeme verfügen möglicherweise über bestimmte Methoden oder Einschränkungen beim Einbetten beliebiger Skripte.

Die endgültige Seite sollte im `<head>` des HTML-Dokuments einen Code enthalten, der dem folgenden Beispiel ähnelt:

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

Standardmäßig führt Marketo Munchkin beim Laden einer Seite die folgenden Aktionen aus:

1. Überprüft, ob im aktuellen Browser ein Munchkin-Cookie vorhanden ist, und erstellt ggf. eines.
1. Sendet ein „Web-Seite besuchen“-Ereignis an die vorgesehene Marketo-Instanz anhand von Informationen aus der aktuellen Seite und dem aktuellen Browser. Dieses Ereignis zeichnet eine Aktivität für den entsprechenden Marketo-Datensatz auf.
1. Sendet das Ereignis „Auf Link auf Webseite geklickt“, wenn der Benutzer einen Link auswählt.

Verwenden Sie Munchkin [Konfigurationseinstellungen](configuration.md) um dieses Verhalten zu ändern. Verwenden Sie beispielsweise `cookieAnon`, um zu steuern, ob Munchkin ein Cookie für alle Leads erstellt, die die Seite besuchen, oder verwenden Sie `clickTime`, um die Klickverzögerung zu ändern.

Um die Aktivität Besuch zu deaktivieren, setzen Sie `apiOnly` auf „true“. Ab Version 162 (August 2022) verfolgt Munchkin neben `http/s` Links auch Klicks auf `tel` und `mailto` Links.

## Bekannte und anonyme Leads

Wenn ein Lead zum ersten Mal eine Seite in Ihrer Domain besucht, erstellt Marketo einen anonymen Lead-Eintrag. Der Primärschlüssel für diesen Datensatz ist das Munchkin-Cookie (`_mkto_trk`), das im Browser des Benutzers erstellt wird.

Marketo zeichnet nachfolgende Web-Aktivitäten von diesem Browser im anonymen Datensatz auf. Um die Aktivität mit einem bekannten Marketo-Eintrag zu verknüpfen, muss eines der folgenden Ereignisse eintreten:

- Der Lead muss eine von Munchkin verfolgte Seite mit einem `mkt_tok` in der Abfragezeichenfolge über einen verfolgten Marketo-E-Mail-Link besuchen.
- Der Lead muss ein Marketo-Formular ausfüllen.
- Ein REST[Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/associateLeadUsingPOST)-Aufruf muss gesendet werden.

Wenn eines dieser Ereignisse eintritt, verknüpft Marketo das Cookie und alle zugehörigen Web-Aktivitäten mit dem bekannten Lead.

Marketo erstellt für jeden Browser einen anonymen Web-Aktivitätseintrag. Wenn ein Lead Ihre Domain von einem neuen Computer oder Browser aus besucht, muss die Verknüpfung erneut auftreten.

## Domains

Munchkin erstellt und verfolgt Cookies pro Domain. Um einen bekannten Lead domänenübergreifend zu verfolgen, muss in jeder Domain ein Lead-Zuordnungsereignis auftreten.

Angenommen, Sie steuern `marketo.com` und `example.com`. Ein Lead sendet ein Formular auf `marketo.com` und geht später zu `example.com`. Die Aktivität auf `marketo.com` ist mit dem bekannten Lead verknüpft, die Aktivität auf `example.com` jedoch anonym.

Bekannte Leads bleiben über Subdomains hinweg bestehen. Ein bekannter Lead auf `www.example.com` ist auch ein bekannter Lead auf `info.example.com`.

Wenn Ihre Domain auf oberster Ebene aus zwei Teilen besteht, z. B. `.co.uk`, fügen Sie Ihrem Munchkin-Snippet einen `domainLevel` hinzu. Weitere Informationen finden Sie unter [Konfiguration](configuration.md#domainlevel).

## Cookie

Das Munchkin-Cookie verwendet die `_mkto_trk` und einen Wert, der einem der folgenden Muster folgt:

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

Or

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

Munchkin-Cookies sind spezifisch für jede Domain der zweiten Ebene, z. B. `example.com`. Die standardmäßige Cookie-Lebensdauer beträgt 2 Jahre (730 Tage).

## Beta

Um sich für Ihre Landingpages für den Munchkin Beta-Kanal zu registrieren, gehen Sie zu [Admin -> Schatztruhe](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) und aktivieren Sie die Einstellung &quot;Munchkin Beta auf Landingpages“.

Mit dieser Einstellung werden dem Menü **[!UICONTROL Admin]** -> **[!UICONTROL Munchkin]** Codeausschnitte hinzugefügt. Verwenden Sie diese Snippets, um die Betaversion auf externen Sites auszuführen.

## Opt-out

Besucher können das Munchkin-Tracking deaktivieren, indem sie den `querystring` „marketo_opt_out=true“ zur URL im Browser hinzufügen. Wenn Munchkin JavaScript diese Einstellung erkennt, versucht es, ein neues „mkto_opt_out“-Cookie mit dem Wert `true` zu setzen.

Munchkin löscht dann alle anderen Tracking-Cookies von Marketo, setzt keine neuen Cookies und führt keine HTTP-Anfragen durch.
