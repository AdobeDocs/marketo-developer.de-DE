---
title: Webhooks
feature: Webhooks
description: Übersicht über Webhooks
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Webhooks

Marketo ermöglicht die Verwendung von Webhooks für die Kommunikation mit Webdiensten von Drittanbietern. Webhooks unterstützen die Verwendung von GET- oder POST-HTTP-Verben zum Pushen oder Abrufen von Daten von einer bestimmten URL. Detaillierte Anweisungen zur In-App-Erstellung von Webhooks und zum Hinzufügen zu Smart-Kampagnen finden Sie in den folgenden Artikeln:

- [Erstellen eines Webhooks](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Aufrufen von Webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Verwenden eines Webhooks in einer Smart-Kampagne](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Jeder einzelne Webhook verfügt über die folgenden Eigenschaften:

- [!UICONTROL URL] - Geben Sie die URL ein, mit der Sie Ihre Anforderung an den Webdienst senden.
- [!UICONTROL Anforderungstyp] - Die HTTP-Methode.
- [!UICONTROL Payload-Vorlage] - Wenn Sie Informationen im Hauptteil der POST übertragen möchten, geben Sie die Vorlage ein. Verwenden Sie jedes Datenformat, das die HTTP-POST unterstützt, einschließlich XML, JSON oder SOAP. Das Serialisierungsformat muss doppelte Anführungszeichen um Zeichenfolgen zulassen. Um ein Token in Ihre Vorlage einzufügen, klicken Sie auf **[!UICONTROL Token einfügen]**.  Token vom Typ Zeichenfolge werden automatisch in doppelte Anführungszeichen gesetzt.
- [!UICONTROL Token-Kodierung anfordern] - Wenn die Token-Werte Sonderzeichen enthalten (z. B. ein kaufmännisches Und, &#39;&amp;&#39;), geben Sie das Format Ihrer Anfrage an (JSON oder Form/URL). Die richtige Kodierung sollte für den Text ausgewählt werden, um sicherzustellen, dass der Webhook ordnungsgemäß mit dem Webdienst kommuniziert.
- [!UICONTROL Antworttyp] - Wählen Sie das Format der Antwort aus, die Sie vom Dienst erhalten (JSON oder XML). Der richtige Antworttyp muss ausgewählt werden, um die Eigenschaften der Antwort wieder den Lead-Feldern in Marketo zuzuordnen
- [!UICONTROL Benutzerdefinierte Header] - Zugriff über [!UICONTROL Webhooks-Aktionen] -> [!UICONTROL Benutzerdefinierten Header festlegen] - Dieses Menü ermöglicht das Hinzufügen beliebiger benutzerdefinierter Schlüssel-Wert-Paare als HTTP-Header.

Daten können mithilfe von [Antwortzuordnungen](response-mappings.md) zurück in Leads aus Webdienstantworten geschrieben werden

## Token

Alle ausgehenden Felder in einem Webhook (URL, Vorlage und benutzerdefinierte Kopfzeilen) füllen den Inhalt von Token im selben Kontext des Flussschritts. Das bedeutet, dass Lead- und System-Token immer verfügbar sind, während Trigger-, Kampagnen- und Programm-Token in ihren jeweiligen Bereichen verfügbar sind. Siehe token-bezogene Artikel:

- [Token-Übersicht](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [System-Token-Glossar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Token für interessante Momente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Ein häufiger Fall besteht darin, dass ein Programm oder eine Kampagne explizit einer Drittanbieterressource zugeordnet ist. Eine ID kann auf Programmebene als &quot;`My Token`&quot; festgelegt und dann als Token an die Webhook-Anfrage übergeben werden.

## Benutzerdefinierte Header

Webhooks ermöglichen es, eine beliebige Anzahl von benutzerdefinierten Kopfzeilenfeldern zusammen mit der ausgehenden Anfrage zu senden. Diese können über **[!UICONTROL Webhooks-Aktionen]** > **[!UICONTROL Benutzerdefinierten Header festlegen]** hinzugefügt werden. Jeder Header wird als einfaches Schlüssel-Wert-Paar aufgezeichnet. In diesem Bereich können Token verwendet werden.

![Benutzerdefinierte Kopfzeilen](assets/custom-headers.png)

## Tipps

- Der Schritt Webhook-Fluss aufrufen ist nur in Trigger-Kampagnen gültig.
- Aktualisierungen über Antwort-Mappings treten nur dann auf, wenn der Webdienst mit einem 2xx-HTTP-Antwortcode antwortet. Andere Arten von Codes führen nicht zu Aktualisierungen des Datensatzes.
- Sie können Webdienste verwenden, um benutzerdefinierte Daten-Anreicherungen, Validierungen oder Normalisierungen aus internen oder externen Diensten durchzuführen.
- Die Ausführungszeit von Webhook hängt von der Antwortzeit des verwendeten Dienstes ab und kann zu langen Verzögerungen bei der Ausführung von Kampagnen führen. Selbst wenn die Ausführung eines Dienstes nur 50 ms in Anspruch nimmt, ist das 1,5 Stunden bei 100.000-mal ausgeführter Ausführung.
- Marketo wartet bis zu 30 Sekunden auf einen bestimmten Dienstaufruf, bevor der Aufruf beendet wird (auch Zeitüberschreitung genannt).
- Die im URL-Feld eingebetteten Zeichen werden wie geschrieben übergeben, z. B. &quot;&amp;&quot; wird als &quot;&amp;&quot; gesendet, &quot;%26&quot; als &quot;%26&quot;
   - Wenn ein Zeichen beim Empfang durch den Empfängerserver in Prozent kodiert werden soll, sollte es explizit als die Zeichenfolge übergeben werden, die dieses Zeichen darstellt
