---
title: Leistung
feature: REST API
description: Steigern Sie die Leistung der Marketo REST-API mit HTTP-Komprimierung. Aktivieren Sie gzip, um die Bandbreite zu reduzieren. Bulk-APIs werden nicht unterstützt und unter 1024 Byte nicht komprimiert.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 1%

---

# Leistung

Diese Seite enthält eine Liste leistungsbezogener Themen, mit denen Sie die Leistung Ihrer Integration steigern können.

## HTTP-Komprimierung

Die Marketo REST-API unterstützt die HTTP-Komprimierung von Antworttexten mithilfe von Standards, die in der HTTP 1.1-Spezifikation definiert sind. Die Aktivierung der Komprimierung wird empfohlen, da sie die Bandbreitennutzung und die Zeit zum Abrufen von Daten reduziert.

>[!NOTE]
>
>Payloads mit weniger als 1024 Byte werden nicht komprimiert und Bulk-APIs unterstützen keine Komprimierung.

Um die Komprimierung zu aktivieren, fügen Sie die folgende HTTP-Kopfzeile in die Anfrage ein:

```html
Accept-Encoding: gzip
```

Die Marketo-REST-API komprimiert den Antworttext und enthält diese Kopfzeile:

```html
Content-Encoding: gzip
```

Im Folgenden finden Sie ein Beispiel unter Verwendung von cURL zum Aufrufen [&#x200B; Endpunkts „Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) um 5 Leads abzurufen:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
