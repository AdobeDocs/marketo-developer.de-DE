---
title: SOAP FAQs
feature: SOAP
description: SOAP FAQs
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# SOAP FAQs

**F:** Wie erhalte ich eine Liste aller Programme in Marketo zusammen mit ihren Metadaten?

**A:** Um eine Liste aller Programme abzurufen, können Sie [getMObjects](./getmobjects.md) verwenden, um den Typ &quot;Programm&quot;weiterzugeben und includeDetails auf true festzulegen.

**Q:** Gibt es eine Möglichkeit, die Leistung von getMultipleLeads zu beschleunigen?

**A:** Es gibt einige Optionen, um die Leistung des getMultipleLeads-Aufrufs zu beschleunigen. Zunächst müssen Sie die batchSize, die Sie für jeden Aufruf anfordern, reduzieren. 200 ist die empfohlene Stapelgröße. Die zweite Option besteht darin, die Felder anzugeben, die Sie mit dem Filter includeAttributes verwenden möchten. Dadurch wird die Abfrage beschleunigt und die Nutzlast der erhaltenen Antwort verringert. Der letzte Ansatz besteht darin, den LastUpdateAtSelector zu verwenden und die Werte &quot;oldestUpdatedAt&quot;und &quot;latestUpdatedAt&quot;anzugeben. Sie können verschiedene Datumsbereiche angeben und dann mehrere Anforderungen gleichzeitig in den Thread einbinden. Stellen Sie bei Verwendung des Thread-Ansatzes sicher, dass Ihr SOAP-/WSDL-Client [persistente Verbindungen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html) unterstützt.

**F:** Wie kann ich Chancen über die SOAP-API erstellen, wenn sie nicht in ein CRM-System wie SalesForce oder Microsoft Dynamics integriert ist?

**A:** Sie können mithilfe der SOAP-API mithilfe des Aufrufs [syncMObjects](syncmobjects.md) Möglichkeiten erstellen, indem Sie in die Typen OpportunityPersonRole und Opportunity [MObject](marketo-objects.md) schreiben.

**Q:** Kann ich programmatisch eine E-Mail von Marketo senden? Wenn ja, wie kann ich benutzerdefinierte Inhalte für jeden E-Mail-Empfänger senden?

**A:** Absolut. Sie können eine E-Mail von Marketo über die APIs [requestCampaign](requestcampaign.md) oder die Kombination aus [importToList](importtolist.md) und [scheduleCampaign](schedulecampaign.md) SOAP anfordern. Um eine E-Mail sofort an eine oder mehrere Personen zu senden, verwenden Sie [requestCampaign](requestcampaign.md). Wenn Sie planen möchten, dass eine E-Mail zu einem bestimmten Zeitpunkt gesendet wird, verwenden Sie [importToList](importtolist.md) , um die Empfänger der E-Mail anzugeben, und [scheduleCampaign](schedulecampaign.md) , um anzugeben, wann diese Empfänger diese E-Mail erhalten.

Wenn Sie den Inhalt für jeden E-Mail-Empfänger anpassen möchten, können Sie dies tun, indem Sie die in der E-Mail-Vorlage festgelegten Werte für [Programm-Token](../rest-api/tokens.md) überschreiben.
