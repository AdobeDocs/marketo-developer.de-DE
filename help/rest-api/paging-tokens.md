---
title: "Paging-Token"
feature: REST API
description: "Anzeigen von Paging-Token-Daten."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---


# Paging-Token

Um Ergebnisse zu durchblättern oder Daten abzurufen, die relativ zu den Daten aktualisiert wurden, stellt Marketo Paging-Token bereit.

In bestimmten Fällen können lange Paging-Token-Zeichenfolgen zurückgegeben werden. Dies kann dazu führen, dass ein HTTP 414-Fehlercode auftritt. Weitere Informationen zum Umgang mit diesen [errors](error-codes.md).

## Tokentypen

Es gibt zwei verwandte, aber unterschiedliche Typen von Paging-Token, die Marketo bereitstellt:

- Datumsbasiert
- Positionsbasiert

## Datumsbasiert

Das erste ist ein Paging-Token, das ein Datum darstellt. Diese werden zum Abrufen von Aktivitäten, Datenwertänderungen und gelöschten Leads verwendet, die nach dem Datum aufgetreten sind, das durch das Paging-Token dargestellt wird. Dieser Paging-Token-Typ wird durch Aufruf der [Paging-Token abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) -Endpunkt und einschließlich eines Datums.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Das Format der `sinceDateTime` -Parameter muss übereinstimmen mit [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) Standarddatumsnotation. Die besten Ergebnisse erzielen Sie, wenn Sie einen vollständigen Datum verwenden, der die Zeitzone enthält. Die Zeitzone kann im folgenden Format als Offset von GMT dargestellt werden:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Oder verwenden Sie ein großes &quot;Z&quot;als Kurzzeichen, um UTC wie folgt darzustellen:

`yyyy-mm-ddThh:mm:ssZ`

Beispiele

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

weil `sinceDateTime` ein Abfrageparameter ist, muss er URL-codiert sein.

Die `nextPageToken` Zeichenfolge wird dann für eine [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET)oder [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) -Aufruf und -Aktivitäten werden von nach dem Datum abgerufen, das der Paging-Token-API abgerufen wurde.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Positionsbasiert

Der zweite Typ des Paging-Tokens kann durch einen Batch-Abrufaufruf an eine Lead-Datenbank-API zurückgegeben werden. Diese Art von Paging-Token ähnelt dem Konzept eines Datenbank-Cursors, der das Durchlaufen von Datensätzen ermöglicht. Beispielsweise kann ein Aufruf &quot;Leads nach Filtertyp abrufen&quot;einen Satz darstellen, der größer als die angegebene Batch-Größe ist, normalerweise die maximale und standardmäßige Größe von 300. Wenn mehr Ergebnisse vorliegen, ist das Feld moreResult in der Antwort wahr und ein `nextPageToken` zurückgegeben. Um die zusätzlichen Datensätze in der Ergebnismenge abzurufen, wird ein zusätzlicher Aufruf einschließlich `nextPageToken` mit dem empfangenen Wert aus der vorherigen Antwort im neuen Aufruf. Die resultierende Antwort gibt die nächste Seite im Ergebnissatz zurück.
