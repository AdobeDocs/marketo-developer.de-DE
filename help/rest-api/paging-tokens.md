---
title: Paging-Token
feature: REST API
description: Anzeigen von Paging-Token-Daten.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: a00583f367c2da36d9d1d6e0b05bfd4216573fbb
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---

# Paging-Token

Um Ergebnisse zu durchblättern oder Daten abzurufen, die relativ zu den Daten aktualisiert wurden, stellt Marketo Paging-Token bereit.

In bestimmten Fällen können lange Paging-Token-Zeichenfolgen zurückgegeben werden. Dies kann dazu führen, dass ein HTTP 414-Fehlercode auftritt. Weitere Informationen zum Umgang mit diesen [Fehlern](error-codes.md) finden Sie.

Weitere Informationen finden Sie in der Dokumentation zur [Paging-Token-API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) .

## Tokentypen

Es gibt zwei verwandte, aber unterschiedliche Typen von Paging-Token, die Marketo bereitstellt:

- Datumsbasiert
- Positionsbasiert

## Datumsbasiert

Das erste ist ein Paging-Token, das ein Datum darstellt. Diese werden zum Abrufen von Aktivitäten, Datenwertänderungen und gelöschten Leads verwendet, die nach dem Datum aufgetreten sind, das durch das Paging-Token dargestellt wird. Dieser Paging-Token-Typ wird durch Aufruf des Endpunkts [Paging-Token abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) und einschließlich einer datetime generiert.

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

Das Format des Parameters `sinceDateTime` muss der Standarddatumsnotation von [ISO 8601](https://de.wikipedia.org/wiki/ISO_8601) entsprechen. Die besten Ergebnisse erzielen Sie, wenn Sie einen vollständigen Datum verwenden, der die Zeitzone enthält. Die Zeitzone kann im folgenden Format als Offset von GMT dargestellt werden:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Oder verwenden Sie ein großes &quot;Z&quot;als Kurzzeichen, um UTC wie folgt darzustellen:

`yyyy-mm-ddThh:mm:ssZ`

Beispiele

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Da `sinceDateTime` ein Abfrageparameter ist, muss er URL-codiert sein.

Die Zeichenfolge &quot;`nextPageToken`&quot;wird dann einem Aufruf &quot;[Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET)&quot;, &quot;[Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET)&quot;oder &quot;[Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET)&quot;bereitgestellt und Aktivitäten werden nach dem Datum abgerufen, das der API &quot;Paging-Token abrufen&quot;angegeben wurde.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Positionsbasiert

Der zweite Typ des Paging-Tokens kann durch einen Batch-Abrufaufruf an eine Lead-Datenbank-API zurückgegeben werden. Diese Art von Paging-Token ähnelt dem Konzept eines Datenbank-Cursors, der das Durchlaufen von Datensätzen ermöglicht. Beispielsweise kann ein Aufruf &quot;Leads nach Filtertyp abrufen&quot;einen Satz darstellen, der größer als die angegebene Batch-Größe ist, normalerweise die maximale und standardmäßige Größe von 300. Wenn mehr Ergebnisse vorliegen, lautet das Feld moreResult in der Antwort &quot;true&quot;(wahr) und es wird ein `nextPageToken` zurückgegeben. Um die zusätzlichen Einträge im Ergebnissatz abzurufen, wird ein zusätzlicher Aufruf einschließlich `nextPageToken` mit dem empfangenen Wert aus der vorherigen Antwort im neuen Aufruf durchgeführt. Die resultierende Antwort gibt die nächste Seite im Ergebnissatz zurück.
