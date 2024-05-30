---
title: "SOAP API"
feature: SOAP
description: "Übersicht über Marketo SOAP"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---


# SOAP-API

Die SOAP-API befindet sich nicht mehr in der aktiven Entwicklung. Die Aufrufe funktionieren weiterhin, aber unsere Entwicklung konzentriert sich auf [REST](https://developer.adobe.com/marketo-apis/) in Zukunft.

Die SOAP-API von Marketo ermöglicht das Erstellen, Abrufen und Entfernen von Entitäten und Daten, die in Marketo gespeichert sind. Sie finden die [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) auf GitHub. Es gibt auch [Client-Bibliotheken](https://github.com/Marketo/Community-Supported-Client-Libraries) um Ihnen etwas Zeit zu sparen.

Neueste API-Version: 3_1

## SOAP WSDL

Rufen Sie zum Abrufen des SOAP WSDL-Dokuments Ihren SOAP-API-Endpunkt von Ihrem **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** Menü.

![SOAP-Endpunkt](assets/endpoint-soap.png)

Ihre WSDL-URL lautet:

`<SOAP API Endpoint> + ?WSDL`

Verwenden Sie nicht den in der WSDL definierten Endpunkt. Jede Marketo-Instanz verfügt über einen eindeutigen Endpunkt, an den Aufrufe durchgeführt werden können.

## Beschränkungen

- **Tägliche Quote:** Die meisten Abonnements erhalten täglich 10.000 API-Aufrufe (die täglich um 12.00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Kundenbetreuer erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz auf 100 Aufrufe pro 20 Sekunden begrenzt.
- **Concurrency Limit:**  Maximal zehn gleichzeitige API-Aufrufe.

Wir empfehlen, dass die Stapelgrößen nicht größer als 300 sind. Größere Größen werden nicht unterstützt und können dazu führen, dass Timeouts auftreten und in Extremfällen gedrosselt werden.

## SOAP-API-Einstellungen in Marketo

1. Gehen Sie zum Abschnitt &quot;Admin&quot;und klicken Sie auf &quot;Web Services&quot;.

![admin-web-services2](assets/admin-web-services2.png)

1. Legen Sie einen geeigneten Verschlüsselungsschlüssel fest, klicken Sie auf &quot;Änderungen speichern&quot;und verwenden Sie die Werte &quot;SOAP API Endpoint&quot;, &quot;Benutzer-ID&quot;und &quot;Verschlüsselungsschlüssel&quot;, um die richtigen Werte zu generieren [Authentifizierungssignatur](authentication-signature.md) für jeden SOAP-API-Aufruf.

![admin-web-services3](assets/admin-web-services3.png)
