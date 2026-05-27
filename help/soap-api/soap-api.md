---
title: SOAP-API
feature: SOAP
description: Die Marketo SOAP-API wird nach dem 31. Oktober 2025 nicht mehr unterstützt. Erfahren Sie, wie Sie zu REST migrieren, Ihre WSDL abrufen, Kontingente, Ratenbeschränkungen und Authentifizierungseinstellungen anzeigen.
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
TQID: https://experienceleague.adobe.com/Atnarr7XLzW3B2R5I8nLtayeE5kt3Bd4T46K6yIPc-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 269
ht-degree: 1%

---

# SOAP-API

Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. Juli 2026 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo [REST-API](../rest-api/rest-api.md) durchgeführt werden, und bestehende Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Service zu vermeiden. Wenn Sie über einen Service verfügen, der die SOAP-API verwendet, finden Sie im SOAP-API[Migrationshandbuch](./migration.md) Informationen zur Migration.

## SOAP WSDL

Um das SOAP-WSDL-Dokument abzurufen, rufen Sie Ihren SOAP-API-Endpunkt über das Menü **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** ab.

![SOAP-Endpunkt](assets/endpoint-soap.png)

Ihre WSDL-URL lautet:

`<SOAP API Endpoint> + ?WSDL`

Verwenden Sie nicht den in der WSDL definierten Endpunkt. Jede Marketo-Instanz verfügt über einen eindeutigen Endpunkt, an den Sie Aufrufe ausführen können.

## Beschränkungen

- **Tägliches Kontingent:** Die meisten Abonnements erhalten 10.000 API-Aufrufe pro Tag (die täglich um 12 % :00AM zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Account Manager erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz beschränkt auf 100 Aufrufe pro 20 Sekunden.
- **Parallelitätslimit:** maximal zehn gleichzeitige API-Aufrufe.

Unsere Empfehlung lautet, dass die Losgrößen nicht größer als 300 sind. Größere Größen werden nicht unterstützt und können zu Zeitüberschreitungen und in extremen Fällen zu Einschränkungen führen.

## SOAP-API-Einstellungen in Marketo

1. Gehen Sie zum Abschnitt **[!UICONTROL Admin]** und wählen Sie **[!UICONTROL Web-Services]** aus.

![admin-web-services2](assets/admin-web-services2.png)

1. Legen Sie einen geeigneten [!UICONTROL Verschlüsselungsschlüssel] fest, wählen Sie **[!UICONTROL Änderungen speichern]** und verwenden Sie die Werte für die SOAP-API [!UICONTROL Endpunkt], [!UICONTROL Benutzer-ID] und [!UICONTROL Verschlüsselungsschlüssel], um für jeden SOAP-API-Aufruf die richtige [Authentifizierungssignatur](authentication-signature.md) zu generieren.

![admin-web-services3](assets/admin-web-services3.png)
