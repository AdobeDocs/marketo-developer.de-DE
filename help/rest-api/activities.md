---
title: Aktivitäten
feature: REST API
description: Eine API zum Verwalten von Marketo Engage-Aktivitäten.
source-git-commit: 13a567be067a8a1272e981fad4e03b0a8519f132
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---


# Aktivitäten

Marketo ermöglicht eine große Vielfalt von Aktivitätstypen im Zusammenhang mit Lead-Datensätzen.  Fast alle Änderungen, Aktionen oder Flussschritte werden im Aktivitätsprotokoll eines Leads aufgezeichnet und können über die API abgerufen oder in Smart List- und Smart Campaign-Filtern und -Triggern genutzt werden.  Aktivitäten werden immer über die leadId zurück zum Lead-Datensatz zugeordnet, die dem Id -Feld des Datensatzes entspricht, und haben auch eine eigene eindeutige ID.

Es gibt eine sehr große Anzahl potenzieller Aktivitätstypen, die von Abonnement zu Abonnement variieren und für jede Aktivität eindeutige Definitionen aufweisen können. Während jede Aktivität ihre eigenen eindeutigen Werte `id`, `leadId` und `activityDate` hat, variieren die Werte `primaryAttributeValueId` und `primaryAttributeValue` in ihrer Bedeutung.

Marketo ermöglicht auch die Erstellung benutzerdefinierter Aktivitätstypen über die Metadaten-API für benutzerdefinierte Aktivitäten. Das Hinzufügen benutzerdefinierter Aktivitäten erfolgt über die API Benutzerdefinierte Aktivitäten hinzufügen .

Die meisten Aktivitäten werden nach einiger Zeit bereinigt.

## Beschreibung

Um eine Liste der verfügbaren Typen und deren Definitionen für eine Instanz abzurufen, können Sie den Endpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) verwenden.

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

Die Antworten in der realen Welt enthalten deutlich mehr Definitionen. In diesem Beispiel ist der angezeigte Typ ein &quot;Formular ausfüllen&quot;, das das primäre Attribut &quot;Webform-ID&quot;hat, das auf die Marketo-ID des ausgefüllten Formulars zurückverweist und verwendet werden kann, um mit diesem Asset in Marketo in Beziehung zu treten. Darüber hinaus gibt es Definitionen für die möglichen Attribute eines bestimmten Aktivitätsdatensatzes dieses Typs und seiner Datentypen. Beachten Sie, dass bei leerem Feld dieses Attribut in einem einzelnen Aktivitätsdatensatz weggelassen wird.

## Abfrage

Rufen Sie den Endpunkt [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET) auf, um Aktivitäten aus Marketo abzurufen. Sie müssen zunächst ein Paging-Token für den Zeitpunkt abrufen, aus dem Sie Aktivitäten abrufen möchten. Anschließend übergeben Sie das Paging-Token im Abfrageparameter `nextPageToken` . Außerdem übergeben Sie bis zu zehn Aktivitätstyp-IDs im Abfrageparameter `activityTypeIds` als kommagetrennte Liste.

Sie können optional entweder einen listId-Abfrageparameter einbeziehen, um Ihre Suche auf die Datensätze zu beschränken, die in einer bestimmten statischen Liste enthalten sind, oder einen leadIds-Abfrageparameter, und nur aus einem bestimmten Satz von Leads nach Aktivitäten suchen. Sie können bis zu 30 leadIds als kommagetrennte Liste übergeben.

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

Verwenden Sie für den ersten Aufruf die API Paging-Token abrufen , um `nextPageToken` zu erhalten. Verwenden Sie für nachfolgende Aufrufe an diesen Endpunkt die `nextPageToken returned` aus der Antwort. Dieser Endpunkt gibt immer `the nextPageToken` zurück.

Wenn das Attribut `moreResult` &quot;true&quot;ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut `moreResult` &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

In einigen Fällen reagiert diese API mit weniger als 300 Aktivitätselementen, aber auch das Attribut `moreResult` ist auf &quot;true&quot;gesetzt.  Dies zeigt an, dass mehr Aktivitäten zurückgegeben werden können und dass der Endpunkt für neuere Aktivitäten abgefragt werden kann, indem der zurückgegebene `nextPageToken` in einen nachfolgenden Aufruf einbezogen wird.

Beachten Sie, dass in jedem Ergebnis-Array-Element das Attribut `id` integer durch das Attribut `marketoGUID` string als eindeutige Kennung ersetzt wird. 

## Datenwertänderungen

Bei Aktivitäten zur Änderung des Datenwerts wird eine spezielle Version der Aktivitäts-API bereitgestellt. Der Endpunkt [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) gibt nur Aktivitäten von Datensätzen zur Datenwertänderung an Lead-Felder zurück. Die Benutzeroberfläche ist mit der API für Lead-Aktivitäten identisch und unterscheidet sich von zwei:

* Es gibt keinen Parameter `activityTypeIds` , da der Endpunkt nur die Aktivitäten &quot;Datenwertänderung&quot;und &quot;Neuer Lead&quot;zurückgibt.
* Der Abfrageparameter `fields` ist erforderlich. Hier können Sie eine kommagetrennte Liste von Feldern übergeben, um anzugeben, für welche Felder Sie Änderungen abrufen möchten.

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

Jede Aktivität in der Antwort verfügt über ein Feld-Array, einschließlich einer Liste der Änderungen in der Aktivität, in der die `id` und `name` des geänderten Felds sowie die neuen und alten Werte relativ zur Änderung angegeben werden.

Beachten Sie, dass in jedem Ergebnis-Array-Element das Attribut `id` integer durch das Attribut `marketoGUID` string als eindeutige Kennung ersetzt wird.

## Gelöschte Leads

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

Beachten Sie, dass in jedem Ergebnis-Array-Element das Attribut `id` integer durch das Attribut `marketoGUID` string als eindeutige Kennung ersetzt wird.

## Durchsuchen von Ergebnissen

Standardmäßig geben die in diesem Abschnitt erwähnten Endpunkte 300 Aktivitätselemente gleichzeitig zurück.  Wenn das Attribut `moreResult` &quot;true&quot;ist, sind weitere Ergebnisse verfügbar. Rufen Sie den Endpunkt auf, bis das Attribut `moreResult` &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse mehr verfügbar sind. Der von diesem Endpunkt zurückgegebene `nextPageToken` sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

In einigen Fällen kann dieser Endpunkt mit weniger als 300 Aktivitätselementen reagieren, aber auch das Attribut `moreResult` muss auf &quot;true&quot;gesetzt sein.  Dies gibt an, dass zusätzliche Aktivitäten zurückgegeben werden können und dass der Endpunkt für neuere Aktivitäten abgefragt werden kann, indem der zurückgegebene `nextPageToken` in einen nachfolgenden Aufruf aufgenommen wird. Beachten Sie, dass das `nextPageToken` in der Anfrage URL-kodiert sein muss.

## Benutzerspezifische Aktivitätstypen

Benutzerspezifische Aktivitäten funktionieren genau wie Standardaktivitäten, mit der Ausnahme, dass das Schema von Drittanbietern und nicht von Marketo verwaltet wird. Instanzen von benutzerdefinierten Aktivitäten werden mit Lead-Datensätzen über den `leadId` genauso wie Standardaktivitäten verknüpft, aber sowohl die primären als auch die sekundären Attribute werden willkürlich definiert. Wenn ein benutzerdefinierter Aktivitätstyp genehmigt wird, werden ein entsprechender Smart-List-Trigger und -Filter erstellt, damit Leads basierend auf aktuellen oder historischen benutzerdefinierten Aktivitätsdaten verarbeitet werden können.

* Maximale Anzahl benutzerdefinierter Aktivitäten: 10
* Maximale Anzahl an Attributen pro benutzerspezifischer Aktivität: 20

Das Abrufen benutzerdefinierter Aktivitätsdaten erfolgt auf dieselbe Weise wie das Abrufen von Standardaktivitäten über die API [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET) .

## Abfragetypen

Zusätzlich zum Standardendpunkt &quot;Get Activity Types&quot;geben die Endpunkte [Get Custom Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) und [Describe Custom Activity Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) Details zu den in der Marketo-Instanz bereitgestellten Aktivitätstypen sowie Metadaten zu den Attributen für einen bestimmten Typ zurück. Die normalen [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) geben weiterhin Metadaten zu benutzerdefinierten Aktivitäten zurück, geben jedoch nicht an, ob ein bestimmter Typ benutzerdefiniert ist.

### Abrufen von Typen

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

Für Typbeschreibungen müssen Sie `apiName` als Pfadparameter übergeben. Standardmäßig erhalten Sie die genehmigte Version der Aktivität. Sie können optional den Parameter `draft=true` übergeben, um die Entwurfsversion der Aktivität abzurufen.

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

### Typ erstellen

Jeder benutzerdefinierte Aktivitätstyp erfordert einen Anzeigenamen, einen API-Namen, einen Trigger-Namen, einen Filternamen und ein primäres Attribut.

Um die Konsistenz Ihrer Typen mit Marketo-Konventionen sicherzustellen und Kollisionen zu vermeiden, müssen Sie beim Erstellen Ihrer Typen einige Richtlinien beachten:

**Anzeigename:** Der Anzeigename des Aktivitätstyps sollte kurz beschreiben, was ein Aktivitätsdatensatz darstellt, z. B. &quot;E-Mail senden&quot;oder &quot;Datenwert ändern&quot;. Diese Namen sollten in der Regel in infinitiver Form vorliegen, d. h. &quot;Ereignis besuchen&quot;.  Anzeigenamen akzeptieren alphanumerische Zeichen, Leerzeichen und Unterstriche. Anzeigenamen müssen mindestens einen Buchstaben enthalten.

**API-Name:** Der API-Name besteht aus alphanumerischen Zeichen (maximale Länge 255). Wenn Sie ein LaunchPoint-Partner sind, sollten Sie Ihren Aktivitätstyp-API-Namen einen repräsentativen Namespace voranstellen. Dadurch sollen Kollisionen mit kundenbereitgestellten Typen vermieden werden.  Die Konvention besteht darin, alle Kleinbuchstaben oder Binnenmajuskel-Groß-/Kleinschreibung zu verwenden, um zwischen anderen Textzeichenfolgen zu unterscheiden.

**Beschreibung:** Bei Aktivitäten mit möglicherweise nicht offensichtlichem Verhalten sollte eine Beschreibung dessen enthalten sein, was der Aktivitätstyp im Verhältnis zum Lead darstellt.

**Trigger-Name:** Jeder Aktivitätstyp muss einen eindeutigen, für Menschen lesbaren Trigger haben. Trigger-Namen sollten sich in der angespannten dritten Person befinden, z. B. &quot;An einem Ereignis teilnehmen&quot;. LaunchPoint-Partner sollten ihren Unternehmensnamen in die Aktivität aufnehmen, z. B. &quot;Webinar besuchen - Firma Acme&quot;.

**Filtername:**  Jeder Aktivitätstyp muss über einen eindeutigen, für Menschen lesbaren Filternamen verfügen. Filternamen sollten sich in der dritten Person befinden, die über eine Spannung vergangen ist, z. B. &quot;Teilnehmer an einem Ereignis&quot;. LaunchPoint-Partner sollten ihren Unternehmensnamen in die Aktivität aufnehmen, d. h. &quot;Angeteiltes Webinar - Acme Company&quot;.

**Primäres Attribut:** Das primäre Attribut einer benutzerdefinierten Aktivität sollte das für den Aktivitätstyp am wichtigsten sein. Beispielsweise wäre dies für die Aktivität &quot;Teilnehmer-Ereignis&quot;der Name des Ereignisses. Primäre Attribute werden standardmäßig in jeder Instanz eines Triggers oder Filters für diesen Aktivitätstyp als Parameter einbezogen. Der Wert wird im Aktivitätsprotokoll eines Personendatensatzes angezeigt, ohne dass ein Drilldown in die Aktivität erforderlich ist.

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

### Aktualisierungstyp

Die Aktualisierung eines Typs ist sehr ähnlich, allerdings ist apiName der einzige erforderliche Parameter als Pfadparameter.

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

Typen können wie standardmäßige Marketo-Assets mit den Typen &quot;Benutzerdefinierte Aktivität genehmigen&quot;, &quot;Benutzerdefinierten Aktivitätstyp verwerfen&quot;und &quot;Benutzerdefinierten Aktivitätstyp löschen&quot;verwaltet werden.


## Benutzerdefinierte Aktivitätstypattribute

Jeder benutzerdefinierte Aktivitätstyp kann über 0-20 sekundäre Attribute verfügen. Sekundäre Attribute können einen beliebigen gültigen Feldtyp für ein Marketo-Feld aufweisen. Sie werden separat vom übergeordneten Typ hinzugefügt, aktualisiert und entfernt. Sie können jedoch bearbeitet werden, während ein Aktivitätstyp verwendet und dann genehmigt wird. Wenn Felder für einen Live-Typ bearbeitet werden, wird für alle Aktivitäten dieses Typs, die nach der Genehmigung erstellt wurden, das neue sekundäre Attribut festgelegt. Änderungen werden nicht rückwirkend auf bestehende Aktivitäten angewendet, die diesen Typ gemeinsam nutzen.

Achten Sie beim Entfernen von Attributen darauf, da dies deren Verfügbarkeit für die Verwendung in den entsprechenden Filtern beeinträchtigt.

Aktualisierungen an der sekundären Attributliste verwenden den API-Namen jedes Attributs als Primärschlüssel. Der API-Name für ein Attribut darf nicht geändert werden. Er muss gelöscht und mit dem gewünschten API-Namen erneut hinzugefügt werden.

Gültige Datentypen für Attribute sind: Zeichenfolge, boolescher Wert, Ganzzahl, Gleitkommazahl, Link, E-Mail, Währung, Datum, Uhrzeit, Telefon, Text.

Beim Ändern des primären Attributs eines Aktivitätstyps sollte jedes vorhandene primäre Attribut demodiert werden, indem zuerst `isPrimary` auf &quot;false&quot;gesetzt wird.

## Erstellen von Attributen

Das Erstellen eines Attributs erfordert einen erforderlichen `apiName` -Pfadparameter. Außerdem sind die Parameter `name` und `dataType` erforderlich.` The description and` `isPrimary` Parameter sind optional.

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

## Aktualisieren von Attributen

Bei der Durchführung von Aktualisierungen an Attributen ist der Primärschlüssel der `apiName` des Attributs. Der Parameter `apiName` muss vorhanden sein, damit die Aktualisierung erfolgreich ist (d. h., Sie können den Parameter `apiName` nicht mit &quot;update&quot;ändern).

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

## Attribute löschen

Das Löschen eines Attributs erfordert einen erforderlichen `apiName` -Pfadparameter, der dem benutzerdefinierten Aktivitäts-API-Namen entspricht.  Erforderlich ist auch ein Attributparameter, der ein Array von Attributobjekten ist.  Jedes Objekt muss einen `apiName` -Parameter enthalten, der dem API-Namen des benutzerdefinierten Aktivitätstyps entspricht.

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

Benutzerdefinierte Aktivitäten sind einmalige Aufzeichnungen historischer Aktivitäten, die sich auf einzelne Personendatensätze in Marketo beziehen. Diese Aktivitäten verfügen über ein Schema, das von Marketo-Administratoren oder remote über eine API-Integration verwaltet wird. Benutzerdefinierte Aktivitäten werden hinzugefügt, um Lead-Datensätze über den Endpunkt [Benutzerdefinierte Aktivitäten hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) zu Lead-Datensätzen hinzuzufügen und über das zugehörige Feld `leadId` mit jedem Lead-Datensatz zu verknüpfen. Benutzerdefinierte Aktivitäten können in der Benutzeroberfläche über das Aktivitätsprotokoll des Leads angezeigt oder über den Endpunkt &quot;Lead-Aktivitäten abrufen&quot;abgerufen werden, indem die Typ-ID der benutzerdefinierten Aktivität angegeben wird.

Benutzerdefinierte Aktivitäten eignen sich für die Aufzeichnung von Daten, die sich auf einen Datensatz einer einzelnen Person beziehen und nicht aktualisiert oder überschrieben werden müssen. Ein Beispiel wäre die Aufzeichnung einer Person, die an einem Ereignis teilnimmt, als Aktivität &quot;Teilnehmer des Ereignisses&quot;. Für Datensätze, die sich auf eine Person beziehen, die sich ändern kann, wie z. B. die Registrierung für Studierende, sollten stattdessen benutzerdefinierte Objekte verwendet werden, da sie aktualisiert werden können, wo benutzerdefinierte Aktivitäten dies möglicherweise nicht tun.

Das Eingabeelement ist ein Array von Aktivitätsobjekten. Es können maximal 300 Aktivitätsdatensätze gleichzeitig gesendet werden.

Die Mitglieder `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` und Attribute sind erforderlich. Das Attribut-Array muss das Attribut non-primary enthalten. Dies kann entweder mit name (Feldname) oder apiName (API-Name) und einem Wert angegeben werden, der dem von Ihnen festgelegten Wert entspricht.

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

## Timeouts

Für Aktivitätsendpunkte gibt es eine Zeitüberschreitung von 30 Sekunden, sofern nicht unten angegeben.

* Paging-Token abrufen: 300s 
* Benutzerdefinierte Aktivität hinzufügen: 90 s 