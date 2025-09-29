---
title: Fehler
feature: Webhooks
description: Erfahren Sie mehr über Marketo Webhook-Fehler-Codes, warum 2xx-Antworten erforderlich sind, um Lead-Felder zu aktualisieren, und wie Sie Fehler mit Webhook abfangen und behandeln können.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# Fehler

Auf dieser Seite werden Fehlerantwortcodes für Webhooks in Marketo aufgelistet.

1000 und 1001 werden von Marketo generiert, und 2xx bis 5xx sind Fehler, die vom System zurückgegeben werden, das der Marketo-Webhook aufruft.

Damit Marketo Werte wieder einem Feld zuordnen kann, muss der Webhook-Antwort-Code von der Variante 2xx sein. Wenn der Webhook die Werte im Marketo-Lead-Datensatz über die Antwort ändern soll, muss der aufgerufene Web-Service 2xx zurückgeben, sodass alle anderen Antwort-Codes dazu führen, dass der Webhook zum Zwecke der Aktualisierung von Lead-Datensatzwerten ignoriert wird.

| Antwortcode | Beschreibung |
| --- | --- |
| 1.000 | Dies zeigt an, dass die Flussaktion „Webhook aufrufen“ in einer Batch-Kampagne gespeichert wird. Webhooks können nur aus Trigger-Kampagnen ausgelöst werden. |
| 1001 | Dies zeigt an, dass der Webservice einen leeren Antworttext ausgegeben hat. |

## Webhook-Fehler abfangen

Fehler aus Webhooks können vom Trigger [!UICONTROL Webhook wird aufgerufen) erfasst ] verarbeitet werden:

![Webhook wird aufgerufen](assets/webhook-called.png)

* Antwort - Antwort ist die wörtliche Antwort-Payload, die von der Anfrage empfangen wurde.
* Fehlertyp - Dies entspricht der Reason-Phrase der HTTP-Statusmeldung.

Diese können verwendet werden, um vorhersehbare Fehler und Ausnahmen zu verarbeiten und darauf zu reagieren. Je nachdem, mit welchem Service Sie integrieren, kann es möglich sein, bestimmte Fehlerklassen automatisch wiederherzustellen, während Warnhinweise erstellt werden können, um Benutzer über unerwartete Fehler zu informieren.
