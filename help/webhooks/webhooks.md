---
title: Webhooks
feature: Webhooks
description: Übersicht über Webhooks
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Webhooks

Marketo ermöglicht die Verwendung von Webhooks für die Kommunikation mit Webdiensten von Drittanbietern. Webhooks unterstützen die Verwendung der GET- oder POST-HTTP-Verben zum Pushen oder Abrufen von Daten von einer bestimmten URL. Detaillierte Anweisungen zur Erstellung von Webhooks innerhalb der Anwendung und zum Hinzufügen zu Smart-Kampagnen finden Sie in den folgenden Artikeln:

- [Erstellen eines Webhooks](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Webhook aufrufen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Verwenden eines Webhooks in einer intelligenten Kampagne](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Jeder einzelne Webhook hat die folgenden Eigenschaften:

- [!UICONTROL URL] - Geben Sie die URL ein, mit der Sie Ihre Anfrage an den Webservice senden.
- [!UICONTROL Request Type] - die HTTP-Methode.
- [!UICONTROL Payload-Vorlage] - Wenn Sie Informationen im Textkörper des POST übertragen möchten, geben Sie die Vorlage ein. Verwenden Sie ein beliebiges Datenformat, das HTTP-POST unterstützt, einschließlich XML, JSON oder SOAP. Das Serialisierungsformat muss doppelte Anführungszeichen um Zeichenfolgen zulassen. Um ein Token in Ihre Vorlage einzufügen, klicken Sie auf **[!UICONTROL Token einfügen]**.  Token vom Typ Zeichenfolge werden automatisch in doppelte Anführungszeichen gesetzt.
- [!UICONTROL Anfragetokenkodierung] - Wenn die Tokenwerte Sonderzeichen enthalten (z. B. ein kaufmännisches Und-Zeichen, &quot;&amp;„), geben Sie das Format Ihrer Anfrage an (JSON oder Formular/URL). Für den Hauptteil sollte die richtige Codierung ausgewählt werden, um sicherzustellen, dass der Webhook mit dem Webservice ordnungsgemäß kommuniziert.
- [!UICONTROL Antworttyp] - Wählen Sie das Format der Antwort aus, die Sie vom Dienst erhalten (JSON oder XML). Es muss der richtige Antworttyp ausgewählt werden, um Eigenschaften der Antwort wieder den Lead-Feldern in Marketo zuzuordnen
- [!UICONTROL Benutzerdefinierte Kopfzeilen] - Zugriff über [!UICONTROL Webhooks-Aktionen] -> [!UICONTROL Benutzerdefinierte Kopfzeile festlegen]. In diesem Menü können beliebig viele benutzerdefinierte Schlüssel-Wert-Paare als HTTP-Kopfzeilen hinzugefügt werden.

Daten können aus Web-Service-Antworten mithilfe von „Antwort-Mappings“ in [ Leads zurückgeschrieben ](response-mappings.md)

## Token

Alle ausgehenden Felder in einem Webhook (URL, Vorlage und benutzerdefinierte Kopfzeilen) füllen den Inhalt von Token im selben Kontext des Flussschritts aus. Das bedeutet, dass Lead- und System-Token immer verfügbar sind, während Trigger-, Kampagnen- und Programm-Token in ihren jeweiligen Bereichen verfügbar sind. Siehe Artikel zu Token:

- [Token-Übersicht](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossar zu System-Token](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Token für interessante Momente](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Ein gängiges Beispiel hierfür ist, wenn ein Programm oder eine Kampagne explizit einer Ressource eines Drittanbieters zugeordnet wird. Eine ID kann auf Programmebene als `My Token` festgelegt und dann als Token an die Webhook-Anfrage übergeben werden.

## Benutzerdefinierte Header

Webhooks ermöglichen die Verwendung einer beliebigen Anzahl von benutzerdefinierten Header-Feldern, die zusammen mit der ausgehenden Anfrage gesendet werden können. Diese können über **[!UICONTROL Webhooks-Aktionen]** > **[!UICONTROL Benutzerdefinierte Kopfzeile festlegen)]**. Jede Kopfzeile wird als einfaches Schlüssel-Wert-Paar aufgezeichnet. In diesem Bereich können Token verwendet werden.

![Benutzerdefinierte Kopfzeilen](assets/custom-headers.png)

## Tipps

- Der Schritt Webhook-Fluss aufrufen ist nur in Trigger-Kampagnen gültig.
- Aktualisierungen über Antwortzuordnungen werden nur vorgenommen, wenn der Webservice mit einem 2xx-HTTP-Antwort-Code antwortet. Andere Code-Typen führen nicht zu Aktualisierungen des Datensatzes.
- Sie können Webservices verwenden, um benutzerdefinierte Datenanreicherung, Validierung oder Normalisierung von internen oder externen Services aus durchzuführen.
- Die Webhook-Ausführungszeit hängt von der Antwortzeit des verwendeten Services ab und kann zu langen Verzögerungen bei der Kampagnenausführung führen. Selbst wenn die Ausführung eines Services nur 50 ms dauert, sind es 1,5 Stunden bei einer 100.000-fachen Ausführung.
- Marketo wartet bis zu 30 Sekunden auf einen bestimmten Service-Aufruf, bevor der Aufruf beendet wird (auch als Zeitüberschreitung bezeichnet).
- Im URL-Feld eingebettete Zeichen werden als geschrieben übergeben, z. B. wird &quot;&amp;&quot; als &quot;&amp;&quot; und &quot;%26“ als &quot;%26“ gesendet.
   - Wenn ein Zeichen beim Empfang durch den Empfänger-Server prozentual kodiert werden soll, sollte es explizit als die Zeichenfolge übergeben werden, die dieses Zeichen darstellt
