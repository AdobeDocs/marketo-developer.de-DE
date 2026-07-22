---
title: Leistung
feature: REST API
description: Steigern Sie die Leistung der Marketo REST-API mit HTTP-Komprimierung. Aktivieren Sie gzip, um die Bandbreite zu reduzieren. Bulk-APIs werden nicht unterstützt und unter 1024 Byte nicht komprimiert.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 1%

---

# Leistung

Verwenden Sie die Leistungsoptionen auf dieser Seite, um die Effizienz Ihrer Integration zu verbessern.

## HTTP-Komprimierung

Die Marketo-REST-API unterstützt die HTTP-Antworttextkomprimierung, wie in der HTTP 1.1-Spezifikation definiert. Aktivieren Sie Komprimierung, um die Bandbreitennutzung und die Zeit zum Datenabruf zu reduzieren.

>[!NOTE]
>
>Payloads mit weniger als 1024 Byte werden nicht komprimiert und Bulk-APIs unterstützen keine Komprimierung.

Um die Komprimierung zu aktivieren, fügen Sie die folgende HTTP-Kopfzeile in die Anfrage ein:

```html
Accept-Encoding: gzip
```

Die Marketo-REST-API komprimiert den Antworttext und enthält die folgende Kopfzeile:

```html
Content-Encoding: gzip
```

Im folgenden cURL-Beispiel wird der Endpunkt [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET) aufgerufen, um fünf Leads abzurufen:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
