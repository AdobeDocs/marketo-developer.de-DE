---
title: SOAP-API
feature: SOAP
description: Übersicht über Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# SOAP-API

Die SOAP-API wird nicht mehr aktiv entwickelt. Die Aufrufe funktionieren weiterhin, aber unsere Entwicklung konzentriert sich auf [REST](https://developer.adobe.com/marketo-apis/) in Zukunft.

Die Marketo SOAP API ermöglicht das Erstellen, Abrufen und Entfernen von Entitäten und Daten, die in Marketo gespeichert sind. Sie finden die [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) auf GitHub. Es gibt auch [Client-Bibliotheken](https://github.com/Marketo/Community-Supported-Client-Libraries) um Ihnen etwas Zeit zu sparen.

Neueste API-Version: 3_1

## SOAP WSDL

Rufen Sie das SOAP WSDL-Dokument ab, um Ihren SOAP API-Endpunkt von Ihrem **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** Menü.

![SOAP Endpunkt](assets/endpoint-soap.png)

Ihre WSDL-URL lautet:

`<SOAP API Endpoint> + ?WSDL`

Verwenden Sie nicht den in der WSDL definierten Endpunkt. Jede Marketo-Instanz verfügt über einen eindeutigen Endpunkt, an den Aufrufe durchgeführt werden können.

## Beschränkungen

- **Tägliche Quote:** Die meisten Abonnements erhalten täglich 10.000 API-Aufrufe (die täglich um 12.00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Kundenbetreuer erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz auf 100 Aufrufe pro 20 Sekunden begrenzt.
- **Concurrency Limit:**  Maximal zehn gleichzeitige API-Aufrufe.

Wir empfehlen, dass die Stapelgrößen nicht größer als 300 sind. Größere Größen werden nicht unterstützt und können dazu führen, dass Timeouts auftreten und in Extremfällen gedrosselt werden.

## SOAP API-Einstellungen in Marketo

1. Navigieren Sie zu **[!UICONTROL Admin]** und klicken Sie auf **[!UICONTROL Web-Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Festlegen einer geeigneten [!UICONTROL Verschlüsselungsschlüssel]klicken **[!UICONTROL Änderungen speichern]** und verwenden Sie die SOAP-API [!UICONTROL Endpunkt], [!UICONTROL Benutzer-ID], und [!UICONTROL Verschlüsselungsschlüssel] Werte, um die richtige [Authentifizierungssignatur](authentication-signature.md) für jeden SOAP API-Aufruf.

![admin-web-services3](assets/admin-web-services3.png)
