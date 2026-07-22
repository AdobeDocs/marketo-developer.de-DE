---
title: Paging-Token
feature: REST API
description: Verwenden Sie Paging-Token der Marketo REST-API, um Aktivitäten und Leads abzurufen. Sie umfassen datums- und positionsbasierte Token, ISO 8601 SinceDatetime und 414-Fehler.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
TQID: https://experienceleague.adobe.com/Ut05n-Y-qPJnvcNRs9liwE3NVBMbJlvaGyv-nExRsek
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 387
ht-degree: 2%

---

# Paging-Token

Marketo stellt Paging-Token bereit, mit denen Ergebnisse durchsucht oder Daten abgerufen werden können, die zu einem bestimmten Datum aktualisiert wurden.

Einige Antworten geben lange Paging-Token-Zeichenfolgen zurück, was zu einem HTTP 414-Fehler führen kann. Siehe Informationen zum Umgang mit diesen [&#x200B; (](error-codes.md)).

Weitere Informationen finden Sie in [&#x200B; Dokumentation zur Paging](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET)Token-API .

## Tokentypen

Marketo bietet zwei miteinander verknüpfte, aber unterschiedliche Typen von Paging-Token:

- Datumsbasierte Token rufen Datensätze ab, die nach einem bestimmten Datum/einer bestimmten Uhrzeit auftreten.
- Positionsbasierte Token durchlaufen Datensätze in einem Ergebnissatz.

## datumsbasiert

Ein datumsbasiertes Paging-Token stellt eine Datums-/Uhrzeitangabe dar. Verwenden Sie sie, um Aktivitäten, Datenwertänderungen und gelöschte Leads abzurufen, die nach diesem Datum/dieser Uhrzeit auftreten.

Generieren Sie ein datumsbasiertes Token, indem Sie den Endpunkt [Paging-Token abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET) mit einem Datum/Uhrzeit-Wert aufrufen:

```http
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Der `sinceDateTime`-Parameter muss die [ISO 8601](https://de.wikipedia.org/wiki/ISO_8601) Standarddatumsnotation verwenden. Um optimale Ergebnisse zu erzielen, sollten Sie ein vollständiges Datum und eine vollständige Zeitzone angeben.

Stellen Sie die Zeitzone als Versatz zur GMT im folgenden Format dar:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Verwenden Sie alternativ ein großes „Z“ zur Darstellung von UTC:

`yyyy-mm-ddThh:mm:ssZ`

Beispiel:

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Da `sinceDateTime` ein Abfrageparameter ist, kodieren Sie seinen Wert mit einer URL.

Übergeben Sie die zurückgegebene `nextPageToken`-Zeichenfolge an einen Aufruf [Lead-](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) abrufen, [Lead-](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) abrufen oder [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Der Aufruf ruft Datensätze ab, die nach dem Datum/der Uhrzeit liegen, das/die der Get Paging Token-API angegeben wurde.

```http
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## positionsbasiert

Ein positionsbasiertes Paging-Token kann von jedem Batch-Abruf an eine Lead-Datenbank-API zurückgegeben werden. Das Token funktioniert wie ein Datenbank-Cursor und ermöglicht das Durchlaufen von Datensätzen.

Beispielsweise kann ein Aufruf Leads nach Filtertyp abrufen einen Ergebnissatz zurückgeben, der größer ist als die angeforderte Batch-Größe, die in der Regel einen maximalen und einen Standardwert von 300 hat. Wenn weitere Ergebnisse verfügbar sind, setzt die Antwort das Feld moreResult auf „true“ und gibt eine `nextPageToken` zurück.

Um die nächste Seite abzurufen, führen Sie einen weiteren Aufruf durch und übergeben Sie den `nextPageToken` aus der vorherigen Antwort. Die Antwort gibt die nächste Seite in der Ergebnismenge zurück.
