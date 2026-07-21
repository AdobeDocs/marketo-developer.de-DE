---
title: Aktivitäten
feature: REST API
description: Verwenden Sie die Marketo Engage-Aktivitäts-REST-API, um Aktivitätstypen aufzulisten, Lead-Aktivitäten mit Paging-Token abzurufen und benutzerdefinierte Änderungen und Änderungen von Datenwerten zu verarbeiten.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
TQID: https://experienceleague.adobe.com/62keaj4uNoxIPCzr9AQzKrIsfuHBvC25knYisZRUvF4
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1758
ht-degree: 0%

---

# Aktivitäten

Marketo unterstützt viele Aktivitätstypen im Zusammenhang mit Lead-Datensätzen. Nahezu jede Änderung, jede Aktion oder jeder Flussschritt wird im Aktivitätsprotokoll eines Leads aufgezeichnet. Sie können diese Aktivitäten über die API abrufen oder in Smart List- und Smart Campaign-Filtern und -Triggern verwenden.

Jede Aktivität verfügt über eine eindeutige `id` und ist über `leadId` mit einem Lead-Datensatz verbunden, der dem ID-Feld des Datensatzes entspricht. Jede Aktivität hat auch eine `activityDate`.

Die verfügbaren Aktivitätstypen variieren je nach Abonnement und jeder Typ hat eine eigene Definition. Die Bedeutung von `primaryAttributeValueId` und `primaryAttributeValue` hängt vom Aktivitätstyp ab.

Verwenden Sie die Metadaten-API für benutzerdefinierte Aktivitäten, um benutzerdefinierte Aktivitätstypen zu erstellen. Verwenden Sie die API Benutzerdefinierte Aktivitäten hinzufügen , um benutzerdefinierte Aktivitätsdatensätze hinzuzufügen.

Die meisten Aktivitäten werden nach einiger Zeit bereinigt.

## beschreiben

Verwenden Sie den Endpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) um die verfügbaren Aktivitätstypen und ihre Definitionen für eine Instanz abzurufen.

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

Die tatsächlichen Antworten enthalten weitere Definitionen. Dieses Beispiel zeigt den Aktivitätstyp „Formular ausfüllen“. Das primäre Attribut „Webform ID“ bezieht sich auf die Marketo-ID des übermittelten Formulars und verknüpft die Aktivität mit diesem Asset.

Die Antwort definiert auch jedes mögliche Attribut für den Aktivitätstyp und seinen Datentyp. Wenn ein Feld leer ist, wird dieses Attribut im einzelnen Aktivitätsdatensatz weggelassen.

## Abfrage

Verwenden Sie den Endpunkt [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) um Aktivitäten abzurufen. Rufen Sie zunächst ein Paging-Token für die Uhrzeit ab, zu der der Aktivitätsabruf beginnen soll. Übergeben Sie dieses Token im `nextPageToken` Abfrageparameter.

Übergeben Sie bis zu zehn Aktivitätstyp-IDs als kommagetrennte Liste im `activityTypeIds` Abfrageparameter.

Optional können Sie die Abfrage mit einem der folgenden Parameter eingrenzen:

- `listId` beschränkt die Ergebnisse auf Datensätze in einer bestimmten statischen Liste.
- `leadIds` beschränkt die Ergebnisse auf Aktivitäten für bis zu 30 Leads, die als kommagetrennte Liste bereitgestellt werden.

>[!CAUTION]
>
>Ab dem 30.12.2026 schlagen Aufrufe an die `Get Lead Activities`- und `Get Lead Changes`-Endpunkte, die den `listId` enthalten, fehl (Fehlercode 1003), wenn die Target-Listen 10.000 oder mehr Leads enthalten. Um Service-Unterbrechungen zu vermeiden, stellen Sie sicher, dass -Aufrufe einen ordnungsgemäßen Umfang haben, um diese Beschränkung zu vermeiden. Siehe [Migrationshandbuch](migration.md).

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

Verwenden Sie für den ersten Aufruf die API Paging-Token abrufen , um `nextPageToken` abzurufen. Übergeben Sie für jeden nachfolgenden Aufruf die von der vorherigen Antwort zurückgegebene `nextPageToken`. Dieser Endpunkt gibt immer `nextPageToken` zurück.

Wenn `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Rufen Sie den Endpunkt mit dem zurückgegebenen `nextPageToken` weiter auf, bis `moreResult` auf „false“ gesetzt ist.

Die API kann weniger als 300 Aktivitätselemente zurückgeben, während `moreResult` auf „true“ gesetzt wird. Schließen Sie in diesem Fall die zurückgegebenen `nextPageToken` in einem anderen Aufruf ein, um neuere Aktivitäten abzurufen.

Innerhalb jedes Ergebnis-Array-Elements ersetzt das `marketoGUID` Zeichenfolgenattribut das `id` Ganzzahlattribut als eindeutige Kennung.

### Datenwertänderungen

Verwenden Sie den Endpunkt [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) um Datenwert-Änderungsdatensätze für Lead-Felder abzurufen. Die Benutzeroberfläche unterscheidet sich auf zwei Arten von der API zum Abrufen von Lead-Aktivitäten:

- Der Endpunkt hat keinen `activityTypeIds`, da er nur die Aktivitäten Datenwertänderung und Neuer Lead zurückgibt.
- Der erforderliche `fields`-Abfrageparameter akzeptiert eine kommagetrennte Liste von Feldern, deren Änderungen Sie abrufen möchten.

>[!CAUTION]
>
>Ab dem 30.12.2026 schlagen Aufrufe an die `Get Lead Activities`- und `Get Lead Changes`-Endpunkte, die den `listId` enthalten, fehl (Fehlercode 1003), wenn die Target-Listen 10.000 oder mehr Leads enthalten. Um Service-Unterbrechungen zu vermeiden, stellen Sie sicher, dass -Aufrufe einen ordnungsgemäßen Umfang haben, um diese Beschränkung zu vermeiden. Siehe [Migrationshandbuch](migration.md).

```http
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

Jede Aktivität in der Antwort verfügt über ein Feld-Array, in dem die Änderungen aufgelistet sind. Jede Änderung gibt die `id` und `name` des Felds zusammen mit den neuen und alten Werten an.

Innerhalb jedes Ergebnis-Array-Elements ersetzt das `marketoGUID` Zeichenfolgenattribut das `id` Ganzzahlattribut als eindeutige Kennung.

### Gelöschte Leads

Verwenden Sie den Endpunkt [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) um gelöschte Lead-Aktivitäten aus Marketo abzurufen.

```http
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

Innerhalb jedes Ergebnis-Array-Elements ersetzt das `marketoGUID` Zeichenfolgenattribut das `id` Ganzzahlattribut als eindeutige Kennung.

### Seitendurchlauf der Ergebnisse

Standardmäßig geben die Endpunkte in diesem Abschnitt 300 Aktivitätselemente gleichzeitig zurück. Wenn `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Übergeben Sie die zurückgegebene `nextPageToken` bei jedem nachfolgenden Aufruf, bis `moreResult` „false“ ist.

Ein Endpunkt kann beim Festlegen von `moreResult` auf „true“ weniger als 300 Aktivitätselemente zurückgeben. Schließen Sie in diesem Fall die zurückgegebenen `nextPageToken` in einem anderen Aufruf ein, um neuere Aktivitäten abzurufen. URL-`nextPageToken` in der Anfrage kodieren

## Benutzerdefinierte Aktivitätstypen

Benutzerdefinierte Aktivitäten funktionieren wie Standardaktivitäten, aber Drittanbieter verwalten ihre Schemata. Benutzerdefinierte Aktivitätsdatensätze verknüpfen mit Lead-Datensätzen über `leadId`, und ihre primären und sekundären Attribute sind benutzerdefiniert.

Wenn ein benutzerdefinierter Aktivitätstyp genehmigt wird, erstellt Marketo einen entsprechenden Smart List-Trigger und -Filter. Sie können dann Leads basierend auf aktuellen oder historischen benutzerdefinierten Aktivitätsdaten verarbeiten.

- Maximal benutzerdefinierte Aktivitäten: 10
- Maximal Attribute pro benutzerdefinierter Aktivität: 20

Rufen Sie benutzerdefinierte Aktivitätsdaten über die API [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) auf die gleiche Weise wie Standardaktivitäten ab.

## Abfragetypen

Verwenden [Abrufen benutzerdefinierter Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) um Details zu den in einer Marketo-Instanz bereitgestellten Typen abzurufen. Verwenden Sie [Beschreibung des benutzerdefinierten Aktivitätstyps](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) um Attributmetadaten für einen bestimmten Typ abzurufen.

Der Standardendpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) gibt auch benutzerdefinierte Aktivitätsmetadaten zurück, aber er identifiziert nicht, ob ein Typ benutzerdefiniert ist.

### Typen abrufen

```http
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

Um einen Typ zu beschreiben, übergeben Sie `apiName` als Pfadparameter. Standardmäßig gibt der Endpunkt die genehmigte Version der Aktivität zurück. Um die Entwurfsversion abzurufen, übergeben Sie den optionalen `draft=true`.

```http
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

Jeder benutzerdefinierte Aktivitätstyp erfordert einen Anzeigenamen, einen API-Namen, einen Trigger-Namen, einen Filternamen und ein Primärattribut. Verwenden Sie die folgenden Richtlinien, um die Typen mit den Konventionen von Marketo konsistent zu halten und Namenskollisionen zu vermeiden:

- **Anzeigename:** Beschreiben Sie kurz, was ein Aktivitätsdatensatz darstellt, z. B. „E-Mail senden“ oder „Datenwert ändern“. Verwenden Sie das infinitive Formular, z. B. „An einer Veranstaltung teilnehmen“. Anzeigenamen akzeptieren alphanumerische Zeichen, Leerzeichen und Unterstriche und müssen mindestens einen Buchstaben enthalten.

- **API-Name** Verwenden Sie alphanumerische Zeichen mit einer maximalen Länge von 255. Wenn Sie LaunchPoint-Partner sind, stellen Sie den Namen der Aktivitätstyp-API einen repräsentativen Namespace voran, um Konflikte mit vom Kunden bereitgestellten Typen zu vermeiden. Verwenden Sie Kleinbuchstaben oder Binnenmajuskeln, um API-Namen von anderen Zeichenfolgen zu unterscheiden.

- **Beschreibung:** Beschreiben Sie bei Aktivitäten mit nicht offensichtlichem Verhalten, was der Aktivitätstyp in Bezug auf den Lead darstellt.

- **Name des Triggers:** Geben Sie einen eindeutigen, für Menschen lesbaren Namen in der Präsenz der dritten Person an, z. B. „Teilnahme an einem Event“. LaunchPoint-Partner sollten ihren Firmennamen, z. B. „Teilnahme am Webinar - Acme Company“, angeben.

- **Filtername:** Geben Sie einen eindeutigen, für Menschen lesbaren Namen in der Vergangenheitsform der dritten Person an, z. B. „An einem Ereignis teilgenommen“. LaunchPoint-Partner sollten ihren Firmennamen, z. B. „Teilnahme am Webinar - Acme Company“, angeben.

- **Primäres Attribut** Wählen Sie für den Aktivitätstyp das Feld mit der höchsten Wertigkeit aus. Bei einer Aktivität des Typs „Teilgenommene Veranstaltung“ ist dieses Feld der Ereignisname. Das Primärattribut wird standardmäßig als Parameter in jedem Trigger oder Filter für den Aktivitätstyp angezeigt. Sein Wert wird auch im Aktivitätsprotokoll einer Person angezeigt, ohne dass eine Aufschlüsselung der Aktivität erforderlich ist.

Ein neuer benutzerdefinierter Aktivitätstyp wird als Entwurf erstellt. Validieren Sie den Typ, bevor Sie Aktivitätsdatensätze dieses Typs hinzufügen. Aktualisierungen gelten für die Entwurfsversion und müssen genehmigt werden, bevor sie in der Live-Version angezeigt werden. Nachdem ein benutzerdefinierter Aktivitätstyp genehmigt wurde und verwendet wird, können die vorherigen Felder nicht mehr geändert werden.

Beim Erstellen eines Typs ist der Beschreibungsparameter optional. Die erforderlichen Parameter sind `apiName`, `name`, `triggerName`, `filterName` und `primaryAttribute`.

```http
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

Um einen Typ zu aktualisieren, übergeben Sie den erforderlichen apiName als Pfadparameter. Andere Felder können im Anfragetext angegeben werden.

```http
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

Verwalten Sie Typen mit den Aktivitätstypen „Benutzerdefinierten Typ genehmigen“, „Benutzerdefinierten Aktivitätstyp verwerfen“, „Entwurf“ und „Benutzerdefinierten Aktivitätstyp löschen“, wie Sie es mit standardmäßigen Marketo-Assets tun würden.

## Benutzerdefinierte Aktivitätstypattribute

Jeder benutzerdefinierte Aktivitätstyp kann 0-20 sekundäre Attribute aufweisen. Ein sekundäres Attribut kann einen beliebigen gültigen Marketo-Feldtyp verwenden. Sekundäre Attribute getrennt vom übergeordneten Typ hinzufügen, aktualisieren und entfernen.

Sie können Attribute bearbeiten, während ein Aktivitätstyp verwendet wird, und dann die Änderungen genehmigen. Aktivitäten, die nach der Genehmigung erstellt wurden, verwenden den neuen sekundären Attributsatz. Die Änderungen gelten nicht rückwirkend für bestehende Aktivitäten dieser Art.

Durch das Entfernen von Attributen wird auch deren Verfügbarkeit in den entsprechenden Filtern entfernt.

Bei Aktualisierungen der Liste der sekundären Attribute wird der API-Name jedes Attributs als Primärschlüssel verwendet. Um einen API-Namen zu ändern, löschen Sie das Attribut und fügen Sie es erneut mit dem gewünschten API-Namen hinzu.

Gültige Datentypen für Attribute sind: Zeichenfolge, Boolescher Wert, Ganzzahl, Gleitkommazahl, Link, E-Mail, Währung, Datum, Datum/Uhrzeit, Telefon, Text.

Bevor Sie das primäre Attribut eines Aktivitätstyps ändern, stufen Sie das vorhandene primäre Attribut herunter, indem Sie `isPrimary` auf „false“ setzen.

### Erstellen von Attributen

Um ein Attribut zu erstellen, übergeben Sie den erforderlichen `apiName`. Die Parameter `name` und `dataType` sind ebenfalls erforderlich. Die Parameter „description“ und &quot;`isPrimary`&quot; sind optional.

```http
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

Beim Aktualisieren von Attributen ist das Attribut `apiName` der Primärschlüssel und muss bereits vorhanden sein. Sie können `apiName` nicht durch eine Aktualisierung ändern.

```http
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

Um ein Attribut zu löschen, übergeben Sie den erforderlichen `apiName` für die benutzerdefinierte Aktivität. Übergeben Sie außerdem den erforderlichen Attributparameter als Array von Attributobjekten. Jedes Objekt muss einen `apiName` für den benutzerdefinierten Aktivitätstyp enthalten.

```http
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

Benutzerdefinierte Aktivitäten sind Einmalschreibaufzeichnungen historischer Aktivitäten für einzelne Personendatensätze. Marketo-Administratoren können ihr Schema in Marketo verwalten, oder eine API-Integration kann es remote verwalten.

Verwenden Sie den Endpunkt [Benutzerdefinierte Aktivitäten hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST), um Lead-Datensätzen benutzerdefinierte Aktivitäten hinzuzufügen. Das Feld `leadId` verknüpft jede Aktivität mit einem Lead. Zeigen Sie benutzerdefinierte Aktivitäten im Aktivitätsprotokoll des Leads an oder rufen Sie sie über Lead-Aktivitäten abrufen ab, indem Sie die benutzerdefinierte Aktivitätstyp-ID angeben.

Verwenden Sie benutzerdefinierte Aktivitäten für Daten, die sich auf eine Person beziehen und nicht aktualisiert oder überschrieben werden müssen. Zeichnen Sie z. B. die Anwesenheit eines Events als Aktivität „Teilgenommen“ auf.

Verwenden Sie benutzerdefinierte Objekte für personenbezogene Datensätze, die sich ändern können, z. B. für die Registrierung von Studierenden. Benutzerdefinierte Objekte können aktualisiert werden, benutzerdefinierte Aktivitäten jedoch nicht.

Das Eingabeelement ist ein Array von Aktivitätsobjekten. Sie können maximal 300 Aktivitätsdatensätze gleichzeitig senden.

Die Member `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` und attributes sind erforderlich. Das Attribut-Array muss das nicht-primäre Attribut enthalten. Geben Sie ihn entweder mit Name (Feldname) oder API-Name (API-Name) und dem Wert für den festzulegenden Wert an.

```http
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

Aktivitäts-Endpunkte haben eine Zeitüberschreitung von 30 Sekunden, mit Ausnahme der folgenden Endpunkte:

- Paging-Token abrufen: 300s
- Benutzerdefinierte Aktivität hinzufügen: 90er
