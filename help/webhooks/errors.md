---
title: Fehler
feature: Webhooks
description: Fehlercodes für Webhooks
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# Fehler

Auf dieser Seite werden Fehlerantwortcodes für Webhooks in Marketo aufgelistet.

1000 und 1001 werden von Marketo generiert, und 2xx bis 5xx sind Fehler, die vom System zurückgegeben werden, das der Marketo-Webhook aufruft.

Damit Marketo Werte wieder einem Feld zuordnen kann, muss der Webhook-Antwort-Code von der Variante 2xx sein. Wenn der Webhook die Werte im Marketo-Lead-Datensatz über die Antwort ändern soll, muss der aufgerufene Web-Service 2xx zurückgeben, sodass alle anderen Antwort-Codes dazu führen, dass der Webhook zum Zwecke der Aktualisierung von Lead-Datensatzwerten ignoriert wird.

| Antwortcode | Beschreibung |
| --- | --- |
| 1000 | Dies zeigt an, dass die Flussaktion „Webhook aufrufen“ in einer Batch-Kampagne gespeichert wird. Webhooks können nur aus Trigger-Kampagnen ausgelöst werden. |
| 1001 | Dies zeigt an, dass der Webservice einen leeren Antworttext ausgegeben hat. |

## Webhook-Fehler abfangen

Fehler aus Webhooks können vom Trigger [!UICONTROL Webhook wird aufgerufen) erfasst &#x200B;] verarbeitet werden:

![Webhook wird aufgerufen](assets/webhook-called.png)

* Antwort - Antwort ist die wörtliche Antwort-Payload, die von der Anfrage empfangen wurde.
* Fehlertyp - Dies entspricht der Reason-Phrase der HTTP-Statusmeldung.

Diese können verwendet werden, um vorhersehbare Fehler und Ausnahmen zu verarbeiten und darauf zu reagieren. Je nachdem, mit welchem Service Sie integrieren, kann es möglich sein, bestimmte Fehlerklassen automatisch wiederherzustellen, während Warnhinweise erstellt werden können, um Benutzer über unerwartete Fehler zu informieren.
