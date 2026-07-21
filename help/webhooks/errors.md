---
title: Fehler
feature: Webhooks
description: Erfahren Sie mehr über Marketo Webhook-Fehler-Codes, warum 2xx-Antworten erforderlich sind, um Lead-Felder zu aktualisieren, und wie Sie Fehler mit Webhook abfangen und behandeln können.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 3%

---

# Fehler

Auf dieser Seite werden Fehlerantwort-Codes für Marketo-Webhooks beschrieben und der Umgang mit Webhook-Fehlern erläutert.

Marketo generiert die Fehlercodes 1000 und 1001. Das vom Marketo-Webhook aufgerufene System gibt Antwort-Codes von 2xx bis 5xx zurück.

Marketo ordnet einem Feld nur dann Antwortwerte zu, wenn der Webservice einen 2xx-Antwort-Code zurückgibt. Wenn mit einer Webhook-Antwort Werte in einem Marketo-Lead-Datensatz geändert werden sollen, veranlassen alle anderen Antwort-Codes Marketo, die Antwort für Feldaktualisierungen zu ignorieren.

| Antwortcode | Beschreibung |
| --- | --- |
| 1.000 | Dies zeigt an, dass die Flussaktion „Webhook aufrufen“ in einer Batch-Kampagne gespeichert wird. Webhooks können nur aus Trigger-Kampagnen ausgelöst werden. |
| 1001 | Dies zeigt an, dass der Webservice einen leeren Antworttext ausgegeben hat. |

## Webhook-Fehler abfangen

Verwenden Sie den **[!UICONTROL Webhook wird aufgerufen]**-Trigger, um Webhook-Fehler zu erfassen und zu behandeln:

![Webhook wird aufgerufen](assets/webhook-called.png)

* **Response** - Die literale Antwort-Payload, die von der Anfrage empfangen wurde.
* **Fehlertyp** - Die Reason-Phrase der HTTP-Statusmeldung.

Verwenden Sie diese Werte, um auf vorhersehbare Fehler und Ausnahmen zu reagieren. Je nach integriertem Service können Sie automatisch einige Fehlerklassen wiederherstellen und Warnungen für unerwartete Fehler erstellen.
