---
title: SOAP-API
feature: SOAP
description: Übersicht über Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 2%

---

# SOAP-API

Die SOAP-API wird derzeit nicht mehr unterstützt und ist nach dem 31. Oktober 2025 nicht mehr verfügbar.  Die gesamte neue Entwicklung sollte mit der Marketo [REST](https://developer.adobe.com/marketo-apis/) -API durchgeführt werden, und bestehende Dienste sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Dienst zu vermeiden.

## SOAP WSDL

Um das SOAP WSDL-Dokument abzurufen, rufen Sie Ihren SOAP-API-Endpunkt über das Menü **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Webdienste]** ab.

![SOAP-Endpunkt](assets/endpoint-soap.png)

Ihre WSDL-URL lautet:

`<SOAP API Endpoint> + ?WSDL`

Verwenden Sie nicht den in der WSDL definierten Endpunkt. Jede Marketo-Instanz verfügt über einen eindeutigen Endpunkt, an den Aufrufe durchgeführt werden können.

## Beschränkungen

- **Tägliches Kontingent:** Die meisten Abonnements erhalten 10.000 API-Aufrufe pro Tag (die täglich um 12.00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Kundenbetreuer erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz begrenzt auf 100 Aufrufe pro 20 Sekunden.
- **Begrenzung der Parallelität:**  Maximal zehn gleichzeitige API-Aufrufe.

Wir empfehlen, dass die Stapelgrößen nicht größer als 300 sind. Größere Größen werden nicht unterstützt und können dazu führen, dass Timeouts auftreten und in Extremfällen gedrosselt werden.

## SOAP API-Einstellungen in Marketo

1. Wechseln Sie zum Abschnitt **[!UICONTROL Admin]** und klicken Sie auf **[!UICONTROL Webdienste]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Legen Sie einen geeigneten [!UICONTROL Verschlüsselungsschlüssel] fest, klicken Sie auf **[!UICONTROL Änderungen speichern]** und verwenden Sie die Werte für die SOAP-API [!UICONTROL Endpoint], [!UICONTROL Benutzer-ID] und den [!UICONTROL Verschlüsselungsschlüssel], um die richtige [Authentifizierungssignatur](authentication-signature.md) für jeden SOAP API-Aufruf zu generieren.

![admin-web-services3](assets/admin-web-services3.png)
