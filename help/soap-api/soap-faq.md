---
title: "SOAP FAQ"
feature: SOAP
description: "SOAP FAQ"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# FAQ zu SOAP

**F:** Wie erhalte ich eine Liste aller Programme in Marketo mit ihren Metadaten?

**A:** Um eine Liste aller Programme abzurufen, können Sie [getMObjects](./getmobjects.md) Übergeben des Typs &quot;Programm&quot;und Festlegen von includeDetails auf &quot;true&quot;.

**F:** Gibt es eine Möglichkeit, die Leistung von getMultipleLeads zu beschleunigen?

**A:** Es gibt einige Optionen, um die Leistung des getMultipleLeads-Aufrufs zu beschleunigen. Zunächst müssen Sie die batchSize, die Sie für jeden Aufruf anfordern, reduzieren. 200 ist die empfohlene Stapelgröße. Die zweite Option besteht darin, die Felder anzugeben, die Sie mit dem Filter includeAttributes verwenden möchten. Dadurch wird die Abfrage beschleunigt und die Nutzlast der erhaltenen Antwort verringert. Der letzte Ansatz besteht darin, den LastUpdateAtSelector zu verwenden und die Werte &quot;oldestUpdatedAt&quot;und &quot;latestUpdatedAt&quot;anzugeben. Sie können verschiedene Datumsbereiche angeben und dann mehrere Anforderungen gleichzeitig in den Thread einbinden. Wenn Sie den Thread-Ansatz verwenden, stellen Sie sicher, dass Ihr SOAP-/WSDL-Client [persistente Verbindungen](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**F:** Wie kann ich Chancen über die SOAP-API erstellen, wenn diese nicht in ein CRM-System wie SalesForce oder Microsoft Dynamics integriert ist?

**A:** Sie können Chancen mithilfe der SOAP-API mit dem [syncMObjects](syncmobjects.md) Aufrufen des Schreibens an die OpportunityPersonRole und Opportunity [MObject](marketo-objects.md) Typen.

**F:** Kann ich programmatisch eine E-Mail von Marketo senden? Wenn ja, wie kann ich benutzerdefinierte Inhalte für jeden E-Mail-Empfänger senden?

**A:** Absolut. Sie können eine E-Mail von Marketo anfordern, indem Sie entweder [requestCampaign](requestcampaign.md) oder Kombination von [importToList](importtolist.md) und [scheduleCampaign](schedulecampaign.md) SOAP-APIs. Um eine E-Mail sofort an eine oder mehrere Personen zu senden, verwenden Sie [requestCampaign](requestcampaign.md). Wenn Sie planen möchten, dass eine E-Mail zu einem bestimmten Zeitpunkt gesendet wird, verwenden Sie [importToList](importtolist.md) die Empfänger der E-Mail anzugeben und [scheduleCampaign](schedulecampaign.md) um anzugeben, wann diese Empfänger diese E-Mail erhalten werden.

Wenn Sie den Inhalt für jeden E-Mail-Empfänger anpassen möchten, können Sie dies tun, indem Sie die Werte von [Programm-Token](../rest-api/tokens.md) die in der E-Mail-Vorlage festgelegt sind.
