---
title: Paging-Token
feature: REST API
description: Verwenden Sie Paging-Token der Marketo REST-API, um Aktivitäten und Leads abzurufen. Sie umfassen datums- und positionsbasierte Token, ISO 8601 SinceDatetime und 414-Fehler.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Paging-Token

Um durch Ergebnisse zu blättern oder aktualisierte Daten in Bezug auf bestimmte Daten abzurufen, stellt Marketo Paging-Token bereit.

In bestimmten Fällen können lange Paging-Token-Zeichenfolgen zurückgegeben werden. Dies kann zu einem HTTP 414-Fehler-Code führen. Weitere Informationen zum Umgang mit diesen ([) &#x200B;](error-codes.md).

Weitere Informationen finden Sie in [&#x200B; Dokumentation zur Paging](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET)Token-API .

## Tokentypen

Es gibt zwei miteinander verknüpfte, aber unterschiedliche Typen von Paging-Token, die Marketo bereitstellt:

- datumsbasiert
- positionsbasiert

## datumsbasiert

Das erste ist ein Paging-Token, das ein Datum darstellt. Diese werden verwendet, um Aktivitäten, Datenwertänderungen und gelöschte Leads abzurufen, die nach dem durch das Paging-Token dargestellten Datum aufgetreten sind. Dieser Typ des Paging-Tokens wird durch Aufruf des Endpunkts [Paging-Token abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) einschließlich eines Datetime-Datums generiert.

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

Das Format des `sinceDateTime`-Parameters muss der standardmäßigen [ISO 8601](https://de.wikipedia.org/wiki/ISO_8601) entsprechen. Die besten Ergebnisse erzielen Sie, wenn Sie einen vollständigen Datums-/Uhrzeitwert verwenden, der die Zeitzone enthält. Die Zeitzone kann entweder als Versatz zur GMT im folgenden Format dargestellt werden:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Oder verwenden Sie ein großes „Z“ als Kurzschreibweise, um UTC wie folgt darzustellen:

`yyyy-mm-ddThh:mm:ssZ`

Beispiele

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Da `sinceDateTime` ein Abfrageparameter ist, muss er URL-codiert sein.

Die `nextPageToken` Zeichenfolge wird dann für einen Aufruf [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) oder [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) bereitgestellt, und die Aktivitäten werden nach dem Datum/der Uhrzeit abgerufen, das/die der API zum Abrufen des Paging-Tokens angegeben wurde.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## positionsbasiert

Der zweite Typ des Paging-Tokens kann bei jedem Batch-Abruf an eine Lead-Datenbank-API zurückgegeben werden. Dieser Typ von Paging-Token ähnelt vom Konzept her einem Datenbank-Cursor, der das Durchlaufen von Datensätzen ermöglicht. Beispielsweise kann ein Aufruf Leads nach Filtertyp abrufen einen Satz darstellen, der größer ist als die angegebene Batch-Größe, normalerweise der maximale Wert und der Standardwert 300. Wenn weitere Ergebnisse vorliegen, ist in der Antwort das Feld „moreResult“ auf „true“ gesetzt und eine `nextPageToken` wird zurückgegeben. Um die zusätzlichen Datensätze in der Ergebnismenge abzurufen, führen Sie im neuen Aufruf einen zusätzlichen Aufruf aus, der `nextPageToken` mit dem empfangenen Wert aus der vorherigen Antwort enthält. Die resultierende Antwort gibt die nächste Seite in der Ergebnismenge zurück.
