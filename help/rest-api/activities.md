---
title: Aktivitäten
feature: REST API
description: Eine API zur Verwaltung von Marketo Engage-Aktivitäten.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Aktivitäten

Marketo ermöglicht eine Vielzahl von Aktivitätstypen im Zusammenhang mit Lead-Datensätzen.  Fast jede Änderung, Aktion oder jeder Flussschritt wird im Aktivitätsprotokoll eines Leads aufgezeichnet und kann über die API abgerufen oder in Smart List- und Smart Campaign-Filtern und -Triggern genutzt werden.  Aktivitäten sind immer über die Lead-ID mit dem Lead-Datensatz verbunden, was dem ID-Feld des Datensatzes entspricht, und verfügen außerdem über eine eigene eindeutige ID.

Es gibt eine sehr große Anzahl potenzieller Aktivitätstypen, die von Abonnement zu Abonnement variieren können und für jeden eine eindeutige Definition haben. Während jede Aktivität ihre eigenen eindeutigen `id`, `leadId` und `activityDate` hat, variieren die `primaryAttributeValueId` und `primaryAttributeValue` Werte in ihrer Bedeutung.

Marketo ermöglicht auch die Erstellung benutzerdefinierter Aktivitätstypen über die Metadaten-API für benutzerdefinierte Aktivitäten. Das Hinzufügen benutzerdefinierter Aktivitäten erfolgt über die API Benutzerdefinierte Aktivitäten hinzufügen .

Die meisten Aktivitäten werden nach einiger Zeit bereinigt.

## beschreiben

Um eine Liste der verfügbaren Typen und ihrer Definitionen für eine Instanz abzurufen, können Sie den Endpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) verwenden.

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

Die Antworten der realen Welt umfassen weitaus mehr Definitionen. In diesem Beispiel ist der angezeigte Typ ein „Formular ausfüllen“, das das Primärattribut „Webformular-ID“ aufweist, das auf die Marketo-ID des Formulars verweist, das ausgefüllt wurde, und das verwendet werden kann, um sich wieder auf dieses bestimmte Asset in Marketo zu beziehen. Darüber hinaus gibt es Definitionen für jedes der möglichen Attribute eines bestimmten Aktivitätsdatensatzes dieses Typs und seiner Datentypen. Wenn das Feld leer ist, wird dieses bestimmte Attribut in einem einzelnen Aktivitätsdatensatz weggelassen.

## Abfrage

Um Aktivitäten aus Marketo abzurufen, rufen Sie den Endpunkt [Lead-Aktivitäten ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET)) auf. Sie müssen zunächst ein Paging-Token für die Datums-/Uhrzeitangabe abrufen, aus der Sie mit dem Abrufen von Aktivitäten beginnen möchten. Anschließend übergeben Sie das Paging-Token im `nextPageToken` Abfrageparameter . Darüber hinaus können Sie im `activityTypeIds` Abfrageparameter bis zu zehn Aktivitätstyp-IDs als kommagetrennte Liste übergeben.

Sie können optional entweder einen listId-Abfrageparameter einschließen, um Ihre Suche auf die Datensätze einzugrenzen, die in einer bestimmten statischen Liste enthalten sind, oder einen leadIds-Abfrageparameter und Aktivitäten aus einem bestimmten Satz von Leads suchen. Sie können bis zu 30 leadIds als kommagetrennte Liste übergeben.

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Verwenden Sie für den ersten Aufruf die API Paging-Token abrufen , um `nextPageToken` abzurufen. Verwenden Sie für nachfolgende Aufrufe an diesen Endpunkt die `nextPageToken returned` aus der Antwort. Dieser Endpunkt gibt immer `the nextPageToken` zurück.

Wenn das Attribut `moreResult` wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das `moreResult`-Attribut „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

In einigen Fällen reagiert diese API möglicherweise mit weniger als 300 Aktivitätselementen, aber auch das `moreResult` Attribut ist auf „true“ gesetzt.  Dies zeigt an, dass mehr Aktivitäten zurückgegeben werden können und dass der Endpunkt nach neueren Aktivitäten abgefragt werden kann, indem die zurückgegebene `nextPageToken` in einen nachfolgenden Aufruf aufgenommen wird.

Beachten Sie, dass innerhalb jedes Ergebnis-Array-Elements das `id` Ganzzahlattribut durch das `marketoGUID` Zeichenfolgenattribut als eindeutige Kennung ersetzt wird.

### Datenwertänderungen

Für Aktivitäten mit Datenwertänderung wird eine spezielle Version der Aktivitäts-API bereitgestellt. Der Endpunkt [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) gibt nur Aktivitäten von Datenwert-Änderungsdatensätzen an Lead-Felder zurück. Die Benutzeroberfläche entspricht der API für Abrufen von Lead-Aktivitäten mit zwei Unterschieden:

* Es gibt keinen `activityTypeIds`, da der Endpunkt nur die Aktivitäten Datenwertänderung und Neuer Lead zurückgibt.
* Der `fields` Abfrageparameter ist erforderlich, wobei Sie eine kommagetrennte Liste von Feldern übergeben können, um anzugeben, für welche Felder Sie Änderungen abrufen möchten.

```
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Jede Aktivität in der Antwort verfügt über ein Feld-Array, einschließlich einer Liste der Änderungen in der Aktivität, in der die `id` und `name` des geänderten Felds sowie die neuen und alten Werte relativ zur Änderung angegeben sind.

Beachten Sie, dass innerhalb jedes Ergebnis-Array-Elements das `id` Ganzzahlattribut durch das `marketoGUID` Zeichenfolgenattribut als eindeutige Kennung ersetzt wird.

### Gelöschte Leads

Es gibt auch einen speziellen Endpunkt [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) zum Abrufen gelöschter Aktivitäten aus Marketo.

```
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

Beachten Sie, dass innerhalb jedes Ergebnis-Array-Elements das `id` Ganzzahlattribut durch das `marketoGUID` Zeichenfolgenattribut als eindeutige Kennung ersetzt wird.

### Seitendurchlauf der Ergebnisse

Standardmäßig geben die in diesem Abschnitt erwähnten Endpunkte 300 Aktivitätselemente auf einmal zurück.  Wenn das `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Rufen Sie den Endpunkt auf, bis das `moreResult`-Attribut „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse mehr verfügbar sind. Die von diesem Endpunkt zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

In einigen Fällen reagiert dieser Endpunkt möglicherweise mit weniger als 300 Aktivitätselementen, aber auch das `moreResult` Attribut ist auf „true“ gesetzt.  Dies zeigt an, dass zusätzliche Aktivitäten zurückgegeben werden können und dass der Endpunkt nach neueren Aktivitäten abgefragt werden kann, indem die zurückgegebene `nextPageToken` in einen nachfolgenden Aufruf aufgenommen wird. Beachten Sie, dass die `nextPageToken` in der Anfrage URL-kodiert sein muss.

## Benutzerdefinierte Aktivitätstypen

Benutzerdefinierte Aktivitäten funktionieren genau wie Standardaktivitäten, mit der Ausnahme, dass das Schema von Drittanbietern und nicht von Marketo verwaltet wird. Instanzen von benutzerdefinierten Aktivitäten werden wie Standardaktivitäten mit Lead-Datensätzen über die `leadId` verknüpft. Es werden jedoch sowohl primäre als auch sekundäre Attribute willkürlich definiert. Wenn ein benutzerdefinierter Aktivitätstyp genehmigt wird, werden ein entsprechender Smart List-Trigger und ein entsprechender Filter erstellt, sodass Leads basierend auf Daten zu aktuellen oder früheren benutzerdefinierten Aktivitäten verarbeitet werden können.

* Maximale Anzahl benutzerdefinierter Aktivitäten: 10
* Maximale Anzahl von Attributen pro benutzerdefinierter Aktivität: 20

Das Abrufen benutzerdefinierter Aktivitätsdaten erfolgt auf die gleiche Weise wie bei Standardaktivitäten über die [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET)-API.

## Abfragetypen

Zusätzlich zum standardmäßigen Endpunkt Abrufen von Aktivitätstypen geben die Endpunkte [Abrufen benutzerdefinierter Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) und [Beschreiben benutzerdefinierter Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) Details zu den in der Marketo-Instanz bereitgestellten Aktivitätstypen sowie Metadaten zu den Attributen für einen bestimmten Typ zurück. Die normale [Abrufen von Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) gibt weiterhin Metadaten zu benutzerdefinierten Aktivitäten zurück, gibt jedoch nicht an, ob ein bestimmter Typ benutzerdefiniert ist.

### Typen abrufen

```
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Typen beschreiben

Für Typbeschreibungen müssen Sie `apiName` als Pfadparameter übergeben. Standardmäßig wird die genehmigte Version der Aktivität angezeigt. Sie können optional den `draft=true` übergeben, um die Entwurfsversion der Aktivität abzurufen.

```
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Typ erstellen

Jeder benutzerdefinierte Aktivitätstyp erfordert einen Anzeigenamen, einen API-Namen, einen Trigger-Namen, einen Filternamen und ein Primärattribut.

Um die Konsistenz Ihrer Typen mit den Konventionen von Marketo sicherzustellen und Konflikte zu vermeiden, müssen Sie beim Erstellen Ihrer Typen einige Richtlinien befolgen:

**Anzeigename:** Der Anzeigename des Aktivitätstyps sollte kurz beschreiben, was ein Aktivitätsdatensatz darstellt, z. B. „E-Mail senden“ oder „Datenwert ändern“. Diese Namen sollten normalerweise in der infinitiven Form vorliegen, d. h. „An einer Veranstaltung teilnehmen“.  Anzeigenamen akzeptieren alphanumerische Zeichen, Leerzeichen und Unterstriche. Anzeigenamen müssen mindestens einen Buchstaben enthalten.

**API-Name:** Der API-Name besteht aus alphanumerischen Zeichen (maximal 255 Zeichen). Wenn Sie ein LaunchPoint-Partner sind, sollten Sie Ihren API-Namen vom Aktivitätstyp einen repräsentativen Namespace voranstellen. Dadurch sollen Konflikte mit vom Kunden bereitgestellten Typen vermieden werden.  Die Konvention besagt, dass nur Kleinbuchstaben oder Binnenmajuskeln verwendet werden, um zwischen anderen Textzeichenfolgen zu unterscheiden.

**Beschreibung:** Für Aktivitäten, die möglicherweise ein nicht offensichtliches Verhalten aufweisen, sollte eine Beschreibung dessen enthalten, was der Aktivitätstyp in Bezug auf den Lead darstellt.

**Trigger-Name:** Jeder Aktivitätstyp muss einen eindeutigen, für Menschen lesbaren Trigger-Namen haben. Die Namen der Trigger sollten im Anwesenheitszeitalter der dritten Person angegeben werden, z. B. „Teilnimmt an einem Event teil“. LaunchPoint-Partner sollten ihren Firmennamen in die Aktivität aufnehmen, z. B. „Teilnahme am Webinar - Firma Acme“.

**Filtername:**  Jeder Aktivitätstyp muss über einen eindeutigen, für Menschen lesbaren Filternamen verfügen. Filternamen sollten in der Vergangenheitsform der dritten Person sein, z. B. „Teilgenommen an einem Ereignis“. LaunchPoint-Partner sollten ihren Firmennamen in die Aktivität aufnehmen, d. h. „Teilnahme am Webinar - Acme Company“.

**Primäres Attribut** Das Hauptattribut einer benutzerdefinierten Aktivität sollte das für den Aktivitätstyp bedeutendste Feld sein. Bei einer Aktivität des Typs „Teilgenommene Veranstaltung“ wäre dies beispielsweise der Name der Veranstaltung. Primäre Attribute werden standardmäßig in jeder Instanz eines Triggers oder Filters für diesen Aktivitätstyp als Parameter eingeschlossen. Der Wert wird im Aktivitätsprotokoll eines Personendatensatzes angezeigt, ohne dass eine Aufschlüsselung der Aktivität erforderlich ist.

Wenn eine benutzerdefinierte Aktivität erstellt wird, wird sie als Entwurf erstellt und muss genehmigt werden, bevor sie zum Hinzufügen von Aktivitätsdatensätzen dieses Typs verwendet werden kann. Alle Aktualisierungen werden implizit auf die Entwurfsversion des Typs angewendet. Um die Änderungen in der Live-Version des Typs widerzuspiegeln, muss er genehmigt werden. Wenn ein benutzerdefinierter Aktivitätstyp genehmigt und verwendet wird, können keine Änderungen an den oben genannten Feldern vorgenommen werden.

Beim Erstellen eines Typs ist der Beschreibungsparameter optional, während alle folgenden Parameter erforderlich sind: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

```
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Update-Typ

Die Aktualisierung eines Typs ist sehr ähnlich, mit der Ausnahme, dass der apiName der einzige erforderliche Parameter als Pfadparameter ist.

```
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Typ genehmigen

Typen können wie bei Standard-Marketo-Assets mit den Aktivitätstypen „Benutzerdefinierten Typ validieren“, „Benutzerdefinierten Aktivitätstyp verwerfen“, „Entwurf“ und „Benutzerdefinierten Aktivitätstyp löschen“ verwaltet werden.

## Benutzerdefinierte Aktivitätstypattribute

Jeder benutzerdefinierte Aktivitätstyp kann zwischen 0 und 20 sekundäre Attribute aufweisen. Sekundäre Attribute können einen beliebigen gültigen Feldtyp für ein Marketo-Feld aufweisen. Sie werden getrennt vom übergeordneten Typ hinzugefügt, aktualisiert und entfernt, können jedoch bearbeitet werden, während ein Aktivitätstyp verwendet und dann genehmigt wird. Wenn Felder eines Live-Typs bearbeitet werden, wird für alle Aktivitäten dieses Typs, die nach der Genehmigung erstellt werden, das neue sekundäre Attribut festgelegt. Änderungen werden nicht rückwirkend auf bestehende Aktivitäten mit diesem Typ angewendet.

Gehen Sie beim Entfernen von Attributen vorsichtig vor, da dies deren Verfügbarkeit zur Verwendung in den entsprechenden Filtern beeinflusst.

Bei Aktualisierungen der Liste der sekundären Attribute wird der API-Name jedes Attributs als Primärschlüssel verwendet. Der API-Name für ein Attribut darf nicht geändert werden, er muss gelöscht und erneut mit dem gewünschten API-Namen hinzugefügt werden.

Gültige Datentypen für Attribute sind: Zeichenfolge, Boolescher Wert, Ganzzahl, Gleitkommazahl, Link, E-Mail, Währung, Datum, Datum/Uhrzeit, Telefon, Text.

Beim Ändern des primären Attributs eines Aktivitätstyps sollte jedes vorhandene primäre Attribut herabgestuft werden, indem zuerst `isPrimary` auf „false“ gesetzt wird.

### Erstellen von Attributen

Zum Erstellen eines Attributs wird ein erforderlicher `apiName` verwendet. Außerdem sind die Parameter `name` und `dataType` erforderlich.`The description and` `isPrimary` sind optional.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Attribute aktualisieren

Beim Aktualisieren von Attributen ist der `apiName` des Attributs der Primärschlüssel. Der `apiName` Parameter muss vorhanden sein, damit die Aktualisierung erfolgreich ist (d. h., Sie können den `apiName` Parameter nicht mithilfe von „update“ ändern).

```
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Attribute löschen

Beim Löschen eines Attributs wird ein erforderlicher `apiName` verwendet, bei dem es sich um den benutzerdefinierten Aktivitäts-API-Namen handelt.  Außerdem ist ein Attributparameter erforderlich, der ein Array von Attributobjekten ist.  Jedes -Objekt muss einen `apiName` enthalten, bei dem es sich um den benutzerdefinierten Aktivitätstyp-API-Namen handelt.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Hinzufügen benutzerdefinierter Aktivitäten

Benutzerdefinierte Aktivitäten sind Einmalschreibaufzeichnungen historischer Aktivitäten, die sich auf einzelne Personendatensätze in Marketo beziehen. Diese Aktivitäten verfügen über ein Schema, das von Marketo-Administratoren oder remote über eine API-Integration verwaltet wird. Benutzerdefinierte Aktivitäten werden über den Endpunkt [Benutzerdefinierte Aktivitäten hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) zu Lead-Datensätzen hinzugefügt und über das `leadId` Feld mit jedem Lead-Datensatz verknüpft. Benutzerdefinierte Aktivitäten können in der Benutzeroberfläche über das Aktivitätsprotokoll des Leads angezeigt oder über den Endpunkt „Lead-Aktivitäten abrufen“ abgerufen werden, indem die Typ-ID der benutzerdefinierten Aktivität angegeben wird.

Benutzerdefinierte Aktivitäten eignen sich für die Aufzeichnung von Daten, die sich auf einen Datensatz für eine einzelne Person beziehen und nicht aktualisiert oder überschrieben werden müssen. Ein Beispiel wäre die Aufzeichnung einer Person, die an einer Veranstaltung teilnimmt, als Aktivität „Teilgenommen“. Für Datensätze, die sich im Zusammenhang mit einer Person ändern können, z. B. bei der Registrierung für einen Studenten, sollten stattdessen benutzerdefinierte Objekte verwendet werden, da sie aktualisiert werden können, wo benutzerdefinierte Aktivitäten möglicherweise nicht.

Das Eingabeelement ist ein Array von Aktivitätsobjekten. Es können maximal 300 Aktivitätsdatensätze gleichzeitig eingereicht werden.

Die Member `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` und attributes sind erforderlich. Das Attribut-Array muss das nicht-primäre Attribut enthalten. Dies kann entweder mit Name (Feldname) oder apiName (API-Name) und einem Wert angegeben werden, der dem von Ihnen festgelegten Wert entspricht.

```
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Zeitüberschreitungen

Aktivitäts-Endpunkte haben eine Zeitüberschreitung von 30 s, sofern unten nicht anders angegeben.

* Paging-Token abrufen: 300s
* Benutzerdefinierte Aktivität hinzufügen: 90er
