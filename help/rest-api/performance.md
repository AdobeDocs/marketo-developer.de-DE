---
title: Leistung
feature: REST API
description: Tipps zur Leistung für die Arbeit mit der Marketo-API.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 1%

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

Hier ist ein Beispiel für die Verwendung von Curl zum Aufrufen des Endpunkts [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) , um fünf Leads abzurufen:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
