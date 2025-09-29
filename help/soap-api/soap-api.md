---
title: SOAP-API
feature: SOAP
description: Die Marketo SOAP-API wird nach dem 31. Oktober 2025 nicht mehr unterstützt. Erfahren Sie, wie Sie zu REST migrieren, Ihre WSDL abrufen, Kontingente, Ratenbeschränkungen und Authentifizierungseinstellungen anzeigen.
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 1%

---

# SOAP-API

Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. Oktober 2025 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo [REST-API](../rest-api/rest-api.md) durchgeführt werden, und bestehende Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Service zu vermeiden. Wenn Sie über einen Service verfügen, der die SOAP-API verwendet, finden Sie im SOAP-API[Migrationshandbuch](./migration.md) Informationen zur Migration.

## SOAP WSDL

Um das SOAP-WSDL-Dokument abzurufen, rufen Sie Ihren SOAP-API-Endpunkt über das Menü **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** ab.

![SOAP-Endpunkt](assets/endpoint-soap.png)

Ihre WSDL-URL lautet:

`<SOAP API Endpoint> + ?WSDL`

Verwenden Sie nicht den in der WSDL definierten Endpunkt. Jede Marketo-Instanz verfügt über einen eindeutigen Endpunkt, an den Sie Aufrufe ausführen können.

## Beschränkungen

- **Tägliches Kontingent:** Die meisten Abonnements erhalten 10.000 API-Aufrufe pro Tag (die täglich um 12 % :00AM zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Account Manager erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz beschränkt auf 100 Aufrufe pro 20 Sekunden.
- **Parallelitätslimit:**  Maximal zehn gleichzeitige API-Aufrufe.

Unsere Empfehlung lautet, dass die Losgrößen nicht größer als 300 sind. Größere Größen werden nicht unterstützt und können zu Zeitüberschreitungen und in extremen Fällen zu Einschränkungen führen.

## SOAP-API-Einstellungen in Marketo

1. Gehen Sie zum Abschnitt **[!UICONTROL Admin]** und klicken Sie auf **[!UICONTROL Web-Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Legen Sie einen geeigneten [!UICONTROL Verschlüsselungsschlüssel] fest, klicken Sie auf **[!UICONTROL Änderungen speichern]** und verwenden Sie die Werte für die SOAP-API [!UICONTROL Endpunkt], [!UICONTROL Benutzer-ID] und [!UICONTROL Verschlüsselungsschlüssel], um für jeden SOAP-API-Aufruf die richtige [Authentifizierungssignatur](authentication-signature.md) zu generieren.

![admin-web-services3](assets/admin-web-services3.png)
