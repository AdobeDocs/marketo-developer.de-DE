---
title: Leads
feature: REST API
description: Details zu den Leads-API-Aufrufen
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: 8c1c620614408dd2df0b0848e6efc027adb71834
workflow-type: tm+mt
source-wordcount: '3343'
ht-degree: 3%

---

# Leads

[Leads-Endpunktverweis](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

Die API des Marketo Leads bietet eine Vielzahl von Funktionen für einfache CRUD-Anwendungen, die Lead-Datensätze vergleichen, sowie die Möglichkeit, die Mitgliedschaft eines Leads in statischen Listen und Programmen zu ändern und die Verarbeitung intelligenter Kampagnen für Leads zu initiieren.

## Beschreibung

Eine der wichtigsten Funktionen der Leads-API ist die Deskriptionsmethode. Verwenden Sie &quot;Leads beschreiben&quot;, um eine vollständige Liste der Felder abzurufen, die für die Interaktion sowohl über die REST-API als auch die SOAP-API verfügbar sind, sowie Metadaten für jede Komponente:

* Datentyp
* REST- und SOAP-API-Namen
* Länge (falls zutreffend)
* Schreibgeschützt
* Anzeigename

&quot;Describe&quot;ist die primäre &quot;Source of Truth&quot;, die angibt, ob Felder zur Verwendung verfügbar sind, und Metadaten zu diesen Feldern.

### Anfrage

```
GET /rest/v1/leads/describe.json
```

### Antwort

```json
{  
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[  
      {  
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{  
            "name":"company",
            "readOnly":false
         },
         "soap":{  
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Normalerweise enthalten Antworten einen viel größeren Satz von Feldern im Ergebnis-Array, aber wir lassen sie zu Demonstrationszwecken weg. Jedes Element im Ergebnis-Array entspricht einem Feld, das im Lead-Datensatz verfügbar ist, und hat mindestens eine ID, einen displayName und einen Datentyp. Die untergeordneten Objekte &quot;rest&quot;und &quot;soap&quot;können für ein bestimmtes Feld vorhanden sein oder nicht, und seine Präsenz zeigt an, ob das Feld für die Verwendung in den REST- oder SOAP-APIs gültig ist. Die Eigenschaft `readOnly` gibt an, ob das Feld über die entsprechende API (REST oder SOAP) schreibgeschützt ist. Die length -Eigenschaft gibt die maximale Länge des Felds an, sofern vorhanden. Die dataType -Eigenschaft gibt den Datentyp des Felds an.

## Abfrage

Es gibt zwei Hauptmethoden zum Abrufen von Leads: die Methoden &quot;Lead nach ID abrufen&quot;und &quot;Leads nach Filtertyp abrufen&quot;. &quot;Get Lead by Id&quot;verwendet eine einzelne Lead-ID als Pfadparameter und gibt einen einzelnen Lead-Datensatz zurück.

Optional können Sie einen Feldparameter übergeben, der eine kommagetrennte Liste mit zurückzugebenden Feldnamen enthält. Wenn der Feldparameter nicht in dieser Anfrage enthalten ist, werden die folgenden Standardfelder zurückgegeben: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` und `id`. Wenn beim Anfordern einer Feldliste ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

### Anfrage

```
GET /rest/v1/lead/{id}.json
```

### Antwort

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

Für diese Methode gibt es immer einen einzelnen Datensatz an der ersten Position des Ergebnisarray.

&quot;Leads nach Filtertyp abrufen&quot;gibt denselben Datensatztyp zurück, kann jedoch bis zu 300 pro Seite zurückgeben. Dazu sind die Abfrageparameter `filterType` und `filterValues` erforderlich.

`filterType` akzeptiert ein beliebiges benutzerdefiniertes Feld oder die meisten häufig verwendeten Felder. Rufen Sie den Endpunkt `Describe2` auf, um eine umfassende Liste durchsuchbarer Felder zu erhalten, die in `filterType` verwendet werden dürfen. Bei der Suche nach benutzerdefinierten Feldern werden nur die folgenden Datentypen unterstützt: `string`, `email`, `integer`. Sie können Felddetails abrufen (Beschreibung, Typ usw.) mit der oben genannten Deskriptionsmethode.

`filterValues` akzeptiert bis zu 300 Werte im kommagetrennten Format. Der Aufruf sucht nach Datensätzen, in denen das Lead-Feld mit einem der eingeschlossenen `filterValues` übereinstimmt. Wenn die Anzahl der Leads, die mit dem Lead-Filter übereinstimmen, größer als 1.000 ist, wird ein Fehler zurückgegeben: &quot;1003, Zu viele Ergebnisse stimmen mit dem Filter überein&quot;.

Wenn die Gesamtlänge Ihrer GET-Anfrage 8 KB überschreitet, wird ein HTTP-Fehler zurückgegeben: &quot;414, URI zu lang&quot;(gemäß RFC 7231). Als Problemumgehung können Sie Ihre GET in POST ändern, den Parameter _method=GET hinzufügen und eine Abfragezeichenfolge im Anfragetext platzieren.

### Anfrage

```
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Antwort

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Dieser Aufruf sucht nach Datensätzen, die mit den in `filterValues` enthaltenen IDs übereinstimmen, und gibt übereinstimmende Datensätze zurück.

Wenn keine Datensätze gefunden werden, zeigt die Antwort den Erfolg an, das Ergebnis-Array ist jedoch leer.

### Antwort

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Sowohl &quot;Lead nach ID abrufen&quot;als auch &quot;Leads nach Filtertyp abrufen&quot;akzeptieren auch einen Feldabfrageparameter, der eine kommagetrennte Liste von API-Feldern akzeptiert. Wenn dies enthalten ist, enthält jeder Datensatz in der Antwort die aufgeführten Felder.  Wenn es weggelassen wird, wird ein Standardsatz von Feldern zurückgegeben: `id`, `email`, `updatedAt`, `createdAt`, `firstName` und `lastName`.

## ADOBE ECID

Wenn die Adobe Experience Cloud-Funktion zur Zielgruppenfreigabe aktiviert ist, wird ein Cookie-Synchronisierungsprozess durchgeführt, der die Adobe Experience Cloud ID (ECID) mit Marketo-Leads verknüpft.  Die oben genannten Lead-Abrufmethoden können zum Abrufen der zugehörigen ECID-Werte verwendet werden.  Geben Sie dazu im Feldparameter &quot;`ecids`&quot;an. Beispiel: `&fields=email,firstName,lastName,ecids`.

## Erstellen und Aktualisieren

Zusätzlich zum Abrufen von Lead-Daten können Sie Lead-Datensätze über die API erstellen, aktualisieren und löschen. Das Erstellen und Aktualisieren von Leads verwendet denselben Endpunkt, während der in der Anfrage definierte Vorgangstyp verwendet wird. Es können bis zu 300 Datensätze gleichzeitig erstellt oder aktualisiert werden.

>[!NOTE]
>
> Das Aktualisieren von Firmenfeldern mit dem Endpunkt [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) wird nicht unterstützt. Verwenden Sie stattdessen den Endpunkt [Unternehmen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) .

>[!NOTE]
>
> Beim Erstellen oder Aktualisieren des E-Mail-Werts für einen Personendatensatz werden im Feld E-Mail-Adresse nur ASCII-Zeichen unterstützt.

### Anfrage

```
POST /rest/v1/leads.json
```

### Textkörper

```json
{  
   "action":"createOnly",
   "lookupField":"email",
   "input":[  
      {  
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {  
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {  
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Antwort

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "id":50,
         "status":"created"
      },
      {  
         "id":51,
         "status":"created"
      },
      {  
         "id":52,
         "status":"created"
      }
   ]
}
```

In dieser Anfrage sehen Sie zwei wichtige Felder: `action` und `lookupField`.  `action` gibt den Vorgangstyp der Anfrage an und kann `createOrUpdate`, `createOnly`, `updateOnly` oder `createDuplicate` sein. Wenn es weggelassen wird, wird standardmäßig `createOrUpdate` für die Aktion verwendet.  Der Parameter `lookupField` gibt den Schlüssel an, der verwendet werden soll, wenn die Aktion entweder `createOrUpdate` oder `updateOnly` lautet. Wenn `lookupField` weggelassen wird, lautet der Standardschlüssel `email`.

Standardmäßig wird die Standardpartition verwendet. Optional können Sie den Parameter `partitionName` angeben, der nur funktioniert, wenn die Aktion `createOnly` oder `createOrUpdate` ist. Damit `partitionName` als zusätzliche Deduplizierungskriterien funktioniert, muss es Teil des Quelltyps in benutzerdefinierten Deduplizierungsregeln sein. Wenn während eines Aktualisierungsvorgangs in der angegebenen Partition kein Lead vorhanden ist, wird ein Fehler zurückgegeben. Wenn der Nur-API-Benutzer keine Zugriffsberechtigung für die angegebene Partition hat, wird ein Fehler zurückgegeben.

Das Feld `id` kann nur bei Verwendung der Aktion `updateOnly` als Parameter eingefügt werden, da `id` ein vom System verwalteter eindeutiger Schlüssel ist.

Die Anfrage muss auch einen `input` -Parameter aufweisen, der ein Array von Lead-Datensätzen ist. Jeder Lead-Datensatz ist ein JSON-Objekt mit einer beliebigen Anzahl von Lead-Feldern. Die in einem Datensatz enthaltenen Schlüssel sollten für diesen Datensatz eindeutig sein, und alle JSON-Zeichenfolgen sollten UTF-8-kodiert sein. Das Feld `externalCompanyId` kann verwendet werden, um den Lead-Datensatz mit einem Unternehmensdatensatz zu verknüpfen. Das Feld `externalSalesPersonId` kann verwendet werden, um den Lead-Datensatz mit einem Datensatz einer Vertriebsperson zu verknüpfen.

Hinweis: Bei gleichzeitigen oder in kurzer Folge durchgeführten Lead-Up-Anfragen können bei mehreren Anfragen mit demselben Schlüsselwert doppelte Datensätze auftreten, wenn vor der ersten Rückgabe ein nachfolgender Aufruf mit demselben Wert erfolgt. Dies lässt sich vermeiden, indem Sie entweder die `createOnly` oder die `updateOnly` verwenden oder indem Sie Aufrufe in die Warteschlange stellen und darauf warten, dass Ihr Aufruf zurückgegeben wird, bevor Sie nachfolgende Aufrufe mit demselben Schlüssel ausführen.

## Felder

Das Lead-Objekt enthält Standardfelder und optional benutzerdefinierte Felder. Standardfelder sind in jedem Marketo Engage-Abonnement vorhanden, benutzerdefinierte Felder werden vom Benutzer nach Bedarf erstellt. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind der Anzeigename, der API-Name und der dataType. Diese Attribute werden kollektiv als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder für das Lead-Objekt abfragen, erstellen und aktualisieren. Für diese APIs muss der Eigentümer-API-Benutzer über eine oder beide der Berechtigungen &quot;Schema Standard-Feld lesen/schreiben&quot;oder &quot;Benutzerdefiniertes Feld lesen und schreiben&quot;verfügen.

## Abfragefelder

Die Abfrage von Lead-Feldern ist einfach. Sie können ein einzelnes Lead-Feld nach API-Namen abfragen oder den Satz aller Lead-Felder abfragen. Je nach den verwendeten Rollenberechtigungen können sowohl Standardfelder als auch benutzerdefinierte Felder abgerufen werden. Ausgeblendete Felder werden ebenfalls abgerufen.

## Nach Name

Der Endpunkt Lead-Feld nach Name abrufen ruft Metadaten für ein einzelnes Feld im Lead-Objekt ab. Der erforderliche fieldApiName -Pfadparameter gibt den API-Namen des Felds an. Die Antwort entspricht dem Endpunkt Lead beschreiben , enthält jedoch zusätzliche Metadaten wie das Attribut isCustom , das angibt, ob es sich bei dem Feld um ein benutzerdefiniertes Feld handelt.

### Anfrage

```
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Antwort

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Durchsuchen

Der Endpunkt Lead-Felder abrufen ruft Metadaten für alle Felder im Lead-Objekt ab, einschließlich. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den Abfrageparameter `batchSize` verwenden, um diese Zahl zu reduzieren. Wenn das Attribut `moreResult` &quot;true&quot;ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut `moreResult` &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

### Anfrage

```
GET /rest/v1/leads/schema/fields.json
```

### Antwort (abgeschnitten)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Erstellen von Feldern

Der Endpunkt Lead-Felder erstellen erstellt ein oder mehrere benutzerdefinierte Felder auf dem Lead-Objekt. Dieser Endpunkt bietet Funktionen, die mit den Funktionen der Marketo Engage-Benutzeroberfläche vergleichbar sind. Mit diesem Endpunkt können Sie bis zu 100 benutzerdefinierte Felder erstellen.
Achten Sie sorgfältig darauf, jedes Feld, das Sie in Ihrer Produktionsinstanz von Marketo Engage mithilfe der API erstellen.  Nachdem ein Feld erstellt wurde, kann es nicht mehr gelöscht werden (es kann nur ausgeblendet werden). Die Verbreitung nicht verwendeter Felder ist eine schlechte Vorgehensweise, die Ihre Instanz übersichtlicher macht.

Der erforderliche Eingabeparameter ist ein Array von Lead-Feldobjekten. Jedes Objekt enthält ein oder mehrere Attribute. Erforderliche Attribute sind die `displayName`, `name` und `dataType`, die dem Anzeigenamen des Felds in der Benutzeroberfläche, dem API-Namen des Felds und dem Feldtyp entsprechen.  Optional können Sie `description`, `isHidden`, `isHtmlEncodingInEmail` und `isSensitive` angeben.

Es gibt einige Regeln, die mit dem Namen und der Benennung von `displayName` verknüpft sind. Das Attribut name muss eindeutig sein, mit einem Brief beginnen und nur Buchstaben, Zahlen oder Unterstriche enthalten. Die `displayName` muss eindeutig sein und darf keine Sonderzeichen enthalten.  Eine gängige Namenskonvention besteht darin, die Binnenmajuskel-Schreibweise auf `displayName` anzuwenden, um einen Namen zu erstellen. Beispielsweise würde ein `displayName` &quot;Mein benutzerdefiniertes Feld&quot;den Namen &quot;myCustomField&quot;ergeben.

### Anfrage

```
POST /rest/v1/leads/schema/fields.json
```


### Textkörper

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```


### Antwort

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Feld aktualisieren

Der Endpunkt Lead-Feld-Update aktualisiert ein einzelnes benutzerdefiniertes Feld auf dem Lead-Objekt. Die meisten mit der Marketo Engage-Benutzeroberfläche durchgeführten Feldaktualisierungsvorgänge lassen sich mithilfe der API realisieren. Die folgende Tabelle enthält einige Unterschiede.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Attribut</strong></td>
<td style="width: 35%;" colspan="2"><strong>Standardfeld</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Benutzerdefiniertes Feld</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Mit API aktualisieren?</strong></td>
<td style="width: 17.551%;"><strong>Aktualisierbar nach Benutzeroberfläche?</strong></td>
<td style="width: 19.3878%;"><strong>Mit API aktualisieren?</strong></td>
<td style="width: 18.8776%;"><strong>Aktualisierbar nach Benutzeroberfläche?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">nein</td>
<td style="width: 19.3878%;">nein</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">Beschreibung</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">nein</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">nein</td>
<td style="width: 19.3878%;">nein</td>
<td style="width: 18.8776%;">nein</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja (falls von der API erstellt)</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensitive</td>
<td style="width: 17.449%;">ja</td>
<td style="width: 17.551%;">ja</td>
<td style="width: 19.3878%;">ja</td>
<td style="width: 18.8776%;">ja</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">nein</td>
<td style="width: 19.3878%;">nein</td>
<td style="width: 18.8776%;">nein</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">nein</td>
<td style="width: 17.551%;">nein</td>
<td style="width: 19.3878%;">nein</td>
<td style="width: 18.8776%;">nein</td>
</tr>
</tbody>
</table>

Der erforderliche Pfadparameter `fieldApiName` gibt den API-Namen des zu aktualisierenden Felds an. Der erforderliche Eingabeparameter ist ein Array, das ein einzelnes Lead-Feldobjekt enthält.  Das field -Objekt enthält ein oder mehrere Attribute.

### Anfrage

```
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Textkörper

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Antwort

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Lead zu Marketo pushen

Push-Lead ist eine Alternative zum Synchronisieren von Leads mit Marketo. Diese Funktion wurde in erster Linie entwickelt, um einen höheren Grad an Trigger als die standardmäßigen SynchronisierungsLeads zu ermöglichen (ähnlich wie bei Marketo-Formularen). Zusätzlich zur Synchronisierung von Lead-Feldern ermöglicht dieser Endpunkt die Lead-Zuordnung basierend auf Cookie-Werten, die an den -Endpunkt übergeben werden. Dies geschieht durch Übergabe des `mkt_tok` -Werts, der durch Klicken auf eine Marketo-E-Mail generiert wurde, oder durch Übergabe eines Programmnamens im -Aufruf. Dieser Endpunkt erstellt außerdem eine einzelne auslösbare Aktivität, die mit einem Programm und/oder einer Kampagne in Marketo verknüpft ist. Dies ermöglicht das Auslösen von Ereignissen zur Lead-Erfassung, die einer bestimmten Kampagne oder einem bestimmten Programm zugeordnet sind, um zugehörige Workflows aus Marketo heraus zu starten.

Die Push-Lead-Benutzeroberfläche ähnelt der SynchronisierungsLeads sehr. Alle gleichen Primärschlüssel sind gültig und dieselben API-Namen werden für Felder verwendet (es gibt keinen Aktionsparameter, da es sich immer um einen Upsert-Vorgang handelt). Die Parameter `programName` und Eingabe sind erforderlich und die Parameter `lookupField`, `source` und `reason` sind optional. Der Eingabeparameter ist ein Array von Lead-Objekten. Die resultierende Aktivität wird dem entsprechenden benannten Programm zugeordnet. Die Parameter `source` und `reason` sind beliebige Zeichenfolgenfelder, die der Anfrage hinzugefügt werden können, um diese Werte in die resultierenden Aktivitäten einzubetten. Diese können als Einschränkungen in den entsprechenden Triggern (Lead wird an Marketo übergeben) und Filtern (Lead wurde an Marketo übergeben) verwendet werden.

Hinweis zu anonymen Aktivitäten. Wenn Sie dem neu erstellten Lead vorherige anonyme Aktivitäten zuweisen möchten, geben Sie das Cookie-Attribut nicht im Lead-Objekt an und rufen Sie &quot;Associate Lead&quot;nach &quot;Push Lead&quot;auf. Wenn Sie einen neuen Lead ohne Aktivitätsverlauf erstellen möchten, geben Sie einfach das Cookie-Attribut im Lead-Objekt an.

### Anfrage

```
POST /rest/v1/leads/push.json
```

### Textkörper

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/23434456",
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/42434",
             "jobTitle": "Sonny"
         }
     ]
}
```

### Antwort

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Um den Parameter `mkt_tok` zu übergeben, weisen Sie den Wert wie folgt dem mktToken-Element innerhalb eines Lead-Datensatzes im Eingabeparameter zu.

### Textkörper

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Formular senden

&quot;Submit Form&quot;ist eine Alternative zum Synchronisieren von Leads mit Marketo und bietet Funktionen, die einer Marketo-Formularübermittlung entsprechen. Dies ermöglicht das Auslösen von Ereignissen zur Lead-Erfassung, die einer bestimmten Kampagne oder einem bestimmten Programm zugeordnet sind, um zugehörige Workflows aus Marketo heraus zu starten.

Der Endpunkt Formular senden unterstützt die folgenden Funktionen:

* Aktualisiert einen Lead-Datensatz mit dem Feld E-Mail als Primärschlüssel
* Erstellt eine Aktivität vom Typ &quot;Formular ausfüllen&quot;, die mit einem Programm und/oder einer Kampagne verknüpft ist.
* Ermöglicht die Lead-Zuordnung basierend auf dem Cookie-Wert
* Durchführen der Formularfeldvalidierung

Das Senden eines Formulars folgt dem standardmäßigen Muster der Lead-Datenbank. Ein einzelner Objektdatensatz wird an das erforderliche Eingabeelement des JSON-Hauptteils einer POST-Anfrage übergeben. Das erforderliche `formId` -Element enthält die Marketo-Formular-ID der Zielgruppe.

Das optionale `programId` kann verwendet werden, um das Programm anzugeben, dem der Lead hinzugefügt werden soll, und/oder um das Programm anzugeben, dem benutzerdefinierte Felder für Programmmitglieder hinzugefügt werden sollen. Wenn `programId` angegeben wird, wird der Lead zum Programm hinzugefügt und alle im Formular vorhandenen Felder für Programmmitglieder werden ebenfalls hinzugefügt. Beachten Sie, dass sich das angegebene Programm im selben Arbeitsbereich wie das Formular befinden muss. Wenn das Formular keine benutzerdefinierten Felder für Programmmitglieder enthält und `programId` nicht angegeben ist, wird Lead nicht zum Programm hinzugefügt. Wenn sich das Formular in einem Programm befindet und `programId` nicht bereitgestellt wird, wird dieses Programm verwendet, wenn ein oder mehrere benutzerdefinierte Felder für Programmmitglieder im Formular vorhanden sind.

Innerhalb des Eingabedatensatzes ist das Objekt `leadFormFields` erforderlich. Dieses Objekt enthält ein oder mehrere Name/Wert-Paare, die den zu füllenden Formularfeldern entsprechen.  Alle angegebenen Felder müssen innerhalb des angegebenen Formulars definiert werden. Der Name ist der REST-API-Name für das Feld. Beachten Sie, dass das Feld `email` erforderlich ist.

Das Mitgliederobjekt `visitorData` ist optional und enthält Name/Wert-Paare, die den Seitenbesuchsdaten einschließlich `pageURL`, `queryString`, `leadClientIpAddress` und `userAgentString` entsprechen. Kann zum Filtern und Auslösen von zusätzlichen Aktivitätsfeldern verwendet werden.

Die Zeichenfolge der Cookie-Mitglieder ist optional und ermöglicht die Zuordnung eines Munchkin-Cookies zu einem Personendatensatz in Marketo. Wenn ein neuer Lead erstellt wird, werden alle vorherigen anonymen Aktivitäten mit diesem Lead verknüpft, es sei denn, der Cookie-Wert wurde zuvor einem anderen bekannten Datensatz zugeordnet. Wenn der Cookie-Wert zuvor zugeordnet war, werden neue Aktivitäten anhand des Datensatzes verfolgt, alte Aktivitäten werden jedoch nicht aus dem vorhandenen bekannten Datensatz entfernt. Um einen neuen Lead ohne Aktivitätsverlauf zu erstellen, lassen Sie einfach das Cookie-Mitglied weg.

Neue Leads werden in der primären Partition für den Arbeitsbereich erstellt, in dem sich das Formular befindet.

### Anfrage

```
POST /rest/v1/leads/submitForm.json
```

### Header

```
Content-Type: application/json
```

### Textkörper

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Antwort

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

Hier können wir die entsprechenden Details zur Aktivität &quot;Formular ausfüllen&quot;über die Marketo Engage-Benutzeroberfläche sehen:

![Ausfüllen der Formular-Benutzeroberfläche](assets/fill_out_form_activity_details.png)

## Zusammenführen

Manchmal ist es erforderlich, doppelte Datensätze zusammenzuführen, und Marketo erleichtert dies über die API &quot;Leads zusammenführen&quot;. Beim Zusammenführen von Leads werden die Aktivitätsprotokolle, Programme, Kampagnen und Listenmitgliedschaften und CRM-Informationen kombiniert sowie alle Feldwerte zu einem Datensatz zusammengeführt. &quot;Leads zusammenführen&quot;akzeptiert eine Lead-ID als Pfadparameter und entweder eine einzelne `leadId` als Abfrageparameter oder eine Liste mit kommagetrennten IDs im Parameter `leadIds` .

### Anfrage

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Antwort

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Der im Pfadparameter angegebene Lead ist der siegreiche Lead. Wenn also Felder vorliegen, die zwischen den zusammengeführten Datensätzen in Konflikt stehen, wird der Wert des Gewinners übernommen, es sei denn, das Feld im Gewinnerdatensatz ist leer und das entsprechende Feld im verlorenen Datensatz ist nicht vorhanden. Die Leads, die im Parameter `leadId` oder `leadIds` angegeben sind, sind die verlorenen Leads.

Wenn Sie ein Abonnement mit aktivierter SFDC-Synchronisierung haben, können Sie auch den Parameter `mergeInCRM` in Ihrer Anfrage verwenden. Wenn der Wert auf &quot;true&quot;gesetzt ist, wird auch die entsprechende Zusammenführung in Ihrem CRM-System durchgeführt. Wenn sich beide Leads in SFDC befinden und einer ein CRM-Lead und der andere ein CRM-Kontakt ist, ist der Gewinner der CRM-Kontakt (unabhängig davon, welcher Lead als Gewinner angegeben wird). Wenn einer der Leads in SFDC ist und der andere nur in Marketo, dann ist der Gewinner der SFDC-Lead (unabhängig davon, welcher Lead als Gewinner angegeben wird).

## Webaktivität verknüpfen

Über das Lead-Tracking (Munchkin) zeichnet Marketo die Web-Aktivität für Besucher Ihrer Website und Marketo-Landingpages auf. Diese Aktivitäten, Besuche und Klicks, werden mit einem Schlüssel aufgezeichnet, der einem Cookie &quot;_mkto_trk&quot;entspricht, der im Browser des Leads gesetzt wird, und Marketo verwendet diesen Schlüssel, um die Aktivitäten derselben Person zu verfolgen. Normalerweise erfolgt die Zuordnung zu Lead-Datensätzen, wenn ein Lead von einer Marketo-E-Mail aus klickt oder ein Marketo-Formular ausfüllt. Manchmal kann jedoch eine Zuordnung durch einen anderen Ereignistyp ausgelöst werden. Dazu können Sie den Endpunkt Lead verknüpfen verwenden. Der Endpunkt akzeptiert die ID des bekannten Lead-Datensatzes als Pfadparameter und den Cookie-Wert &quot;_mkto_trk&quot;im Cookie-Abfrageparameter.

### Anfrage

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Antwort

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Wenn ein Cookie bereits mit einem bekannten Lead-Datensatz verknüpft ist, bewirkt die Verwendung dieser API für einen anderen Lead-Datensatz, dass neue Web-Aktivitäten für diesen Datensatz aufgezeichnet werden, jedoch keine vorhandene Web-Aktivität in den neuen Datensatz.
Mitgliedschaft

Lead-Datensätze können auch basierend auf einer Mitgliedschaft in einer statischen Liste oder einem Programm abgerufen werden. Darüber hinaus können Sie alle statischen Listen, Programme oder Smart-Kampagnen abrufen, denen ein Lead angehört.

Die Antwortstruktur und die optionalen Parameter sind identisch mit denen von &quot;Get Leads by Filter Type&quot;, auch wenn filterType und filterValues mit dieser API nicht verwendet werden können.
Um über die Marketo-Benutzeroberfläche auf die Listen-ID zuzugreifen, navigieren Sie zur Liste. Die Liste `id` befindet sich in der URL der statischen Liste, `https://app-****.marketo.com/#ST1001A1`. In diesem Beispiel ist 1001 der `id` für die Liste.

### Anfrage

```
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Antwort

```json
{ 
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,  
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828" 
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com", 
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

Der Endpunkt &quot;Listen nach Lead-ID abrufen&quot;akzeptiert einen Lead-Record-Pfadparameter `id` und gibt alle statischen Listenerfassungen zurück, denen der Lead angehört.

### Anfrage

```
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Antwort

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programme

Die Programmmitgliedschaft kann auf ähnliche Weise wie Listen abgerufen werden. Dieselben optionalen Anforderungsparameter sind verfügbar, wenn der Endpunkt &quot;Get Leads by Program Id&quot;aufgerufen und der Pfadparameter `programId` übergeben wird.

Optional können Sie einen Feldparameter übergeben, der eine kommagetrennte Liste mit zurückzugebenden Feldnamen enthält. Wenn der Feldparameter nicht in dieser Anfrage enthalten ist, werden die folgenden Standardfelder zurückgegeben: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` und `id`. Wenn beim Anfordern einer Feldliste ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.

Die Antwortstruktur ist sehr ähnlich, da jedes Element im Ergebnis-Array ein Lead ist, mit der Ausnahme, dass jeder Datensatz auch ein untergeordnetes Objekt namens &quot;membership&quot;hat. Dieses Mitgliedschaftsobjekt enthält Daten über die Beziehung des Leads zum Programm, das im Aufruf angegeben ist, und zeigt immer seine `progressionStatus`, `acquiredBy`, `reachedSuccess` und `membershipDate` an. Wenn das übergeordnete Programm auch ein Interaktionsprogramm ist, hat die Mitgliedschaft die Mitglieder `stream`, `nurtureCadence` und `isExhausted`, um seine Position und Aktivität im Interaktionsprogramm anzugeben.

### Anfrage

```
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Antwort

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

Der Endpunkt Programme von Lead-ID abrufen akzeptiert einen Lead-Datensatz-ID-Pfadparameter und gibt alle Programmdatensätze zurück, denen der Lead angehört. Mit den optionalen Parametern `filterType` und `filterValues` können Sie nach Programm-ID filtern.

### Anfrage

```
GET /rest/v1/leads/{id}/programMembership.json
```

### Antwort

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Intelligente Kampagnen

Der Endpunkt Smart-Kampagnen von der Lead-ID abrufen verwendet einen Lead-Datensatz-ID-Pfadparameter und gibt alle Smart-Kampagnen-Datensätze zurück, denen der Lead angehört.

### Anfrage

```
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Antwort

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Löschen

Das Entfernen von Leads ist mit dem Endpunkt Leads löschen einfach.  Geben Sie Lead-IDs an, die mithilfe der ID-Attribute im Hauptteil gelöscht werden sollen.  Die maximale Anzahl beträgt 300 Leads pro Anfrage.  Verwenden Sie den Content-Type: application/json -Header.

### Anfrage

```
POST /rest/v1/leads/delete.json
```

### Textkörper

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Antwort

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Beziehungen

* Unternehmen über das Feld externalCompanyId im Lead-Datensatz
* SalesPersons über das Feld externalSalesPersonId für den Lead-Datensatz
* Programme durch Programmmitgliedschaft
* Listen nach Listenmitgliedschaft
* Aktivitäten durch das Feld &quot;leadId&quot;in der Aktivität
* Segmentierung durch einzelne Segmentfelder im Lead-Datensatz
* Partitionen durch leadPartitionId in Lead-Datensätzen

## Timeouts

Leads Endpunkte haben eine Zeitüberschreitung von 30 Sekunden, sofern nicht unten angegeben:

* SynchronisierungsLeads: 90s
* Lead verknüpfen: 60 s
* Leads zusammenführen: 180er
* Lead-Partition aktualisieren: 60 s
* Push Lead to Marketo: 90s
* Abrufen von Leads nach Filtertyp: 60 s
* Leads nach Listen-ID abrufen: 60 s
