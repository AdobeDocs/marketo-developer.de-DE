---
title: "Leistung"
feature: REST API
description: "Leistungstipps für die Arbeit mit der Marketo-API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---


# Leistung

Diese Seite enthält eine Liste von Themen zur Leistung, mit denen Sie die Leistung Ihrer Integration steigern können.

## HTTP-Komprimierung

Die Marketo REST-API unterstützt die HTTP-Komprimierung von Antwortkörpern anhand von Standards, die in der HTTP 1.1-Spezifikation definiert sind.  Die Aktivierung der Komprimierung wird empfohlen, da dadurch die Bandbreitennutzung und die Zeit für das Abrufen von Daten reduziert werden.

**Hinweis:**  Nutzlasten unter 1024 Byte werden nicht komprimiert.

Um die Komprimierung zu aktivieren, fügen Sie den folgenden HTTP-Header in die Anfrage ein:

```html
Accept-Encoding: gzip
```

Die Marketo REST-API komprimiert den Antworttext und enthält diesen Header:

```html
Content-Encoding: gzip
```

Im Folgenden finden Sie ein Beispiel mit Curl zum Aufrufen der [Abrufen von Leads nach Filtertyp](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) Endpunkt zum Abrufen von 5 Leads:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
