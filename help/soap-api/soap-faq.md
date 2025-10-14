---
title: Häufig gestellte Fragen zu SOAP
feature: SOAP
description: Erfahren Sie, wie Sie Programme mit getMObjects auflisten, getMultipleLeads optimieren, Opportunities erstellen und personalisierte E-Mails über die Marketo SOAP-API senden oder planen.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# Häufig gestellte Fragen zu SOAP

**F:** Wie erhalte ich eine Liste aller Programme in Marketo zusammen mit ihren Metadaten?

**A:** Um eine Liste aller Programme abzurufen, können Sie [getMObjects“ verwenden](./getmobjects.md) indem Sie den Typ „Program“ übergeben und includeDetails auf „true“ setzen.

**F:** Gibt es eine Möglichkeit, die Leistung von getMultipleLeads zu beschleunigen?

**A:** Es gibt einige Optionen, um die Leistung des Aufrufs getMultipleLeads zu beschleunigen. Die erste besteht darin, die batchSize-Eigenschaft zu reduzieren, die Sie für jeden Aufruf anfordern. 200 ist die empfohlene Stapelgröße. Die zweite Option besteht darin, die Felder anzugeben, an denen Sie interessiert sind, indem Sie den Filter includeAttributes verwenden. Dadurch wird die Abfrage beschleunigt und die Payload der empfangenen Antwort wird reduziert. Der letzte Ansatz besteht darin, den LastUpdateAtSelector zu verwenden und die Werte oldestUpdatedAt und latestUpdatedAt anzugeben. Sie können verschiedene Datumsbereiche angeben und dann mehrere Anfragen gleichzeitig threaden. Wenn Sie den Thread-Ansatz verwenden, stellen Sie sicher, dass Ihr SOAP-/WSDL-Client ([&#x200B; Verbindungen) &#x200B;](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**F:** Wie kann ich Opportunities über die SOAP-API erstellen, wenn sie nicht in ein CRM wie Salesforce oder Microsoft Dynamics integriert ist?

**A:** Sie können Opportunities mithilfe der SOAP-API erstellen, indem Sie die [syncMObjects](syncmobjects.md) Aufrufe an die Typen OpportunityPersonRole und Opportunity [MObject](marketo-objects.md) verwenden.

**F:** Kann ich programmgesteuert eine E-Mail von Marketo aus senden? Wenn ja, wie kann ich für jeden E-Mail-Empfänger benutzerdefinierten Inhalt senden?

**A:** Absolut. Sie können den Versand einer E-Mail von Marketo entweder mit der [requestCampaign](requestcampaign.md) oder einer Kombination aus [importToList](importtolist.md) und [scheduleCampaign](schedulecampaign.md) SOAP-APIs anfordern. Um eine E-Mail sofort an eine oder mehrere Personen zu senden, verwenden Sie [requestCampaign](requestcampaign.md). Wenn Sie den Versand einer E-Mail zu einem bestimmten Datum und zu einer bestimmten Uhrzeit planen möchten, verwenden Sie [importToList](importtolist.md), um die Empfänger der E-Mail anzugeben, und [scheduleCampaign](schedulecampaign.md), um anzugeben, wann diese Empfänger diese E-Mail erhalten.

Wenn Sie den Inhalt für jeden E-Mail-Empfänger anpassen möchten, können Sie dies tun, indem Sie die Werte von [Programm-Token](../rest-api/tokens.md) überschreiben, die in der E-Mail-Vorlage festgelegt sind.
