---
title: "Fehler"
feature: Webhooks
description: "Fehlercodes für Webhooks"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---


# Fehler

Auf dieser Seite werden Fehlerantwort-Codes für Webhooks in Marketo aufgeführt.

1000 und 1001 werden von Marketo generiert, und 2xx bis 5xx sind Fehler, die vom System zurückgegeben werden, das der Marketo-Webhook aufruft.

Damit Marketo Werte wieder einem Feld zuordnen kann, muss der Webhook-Antwortcode der 2xx-Variante entsprechen. Wenn der Webhook die Werte im Marketo-Lead-Datensatz über die Antwort ändern soll und der aufgerufene Webdienst 2xx zurückgeben muss, führen alle anderen Antwort-Codes dazu, dass der Webhook ignoriert wird, um die Lead-Datensatzwerte zu aktualisieren.

| Antwortcode | Beschreibung |
| --- | --- |
| 1000 | Dies zeigt an, dass die Flow-Aktion &quot;Webhook aufrufen&quot;in einer Batch-Kampagne untergebracht ist. Webhooks können nur aus Trigger-Kampagnen ausgelöst werden. |
| 1001 | Dies gibt an, dass der Webdienst einen leeren Antworttext ausgegeben hat. |

## Webhook-Fehler erfassen

Fehler von Webhooks können vom [!UICONTROL Webhook wird aufgerufen] Trigger:

![Webhook wird aufgerufen](assets/webhook-called.png)

* Antwort - Antwort ist die literale Antwort-Payload, die von der Anfrage empfangen wurde.
* Fehlertyp - entspricht der Reason-Phrase der HTTP-Statusmeldung.

Diese können verwendet werden, um vorhersehbare Fehler und Ausnahmen zu handhaben und darauf zu reagieren. Je nachdem, mit welchem Dienst Sie die Integration durchführen, kann es möglich sein, bestimmte Fehlerklassen automatisch wiederherzustellen, während Warnhinweise erstellt werden können, um Benutzer über unerwartete Fehler zu informieren.
