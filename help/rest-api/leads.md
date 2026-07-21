---
title: Leads
feature: REST API
description: Erkunden Sie die Funktionen der Marketo Leads-REST-API, einschließlich Beschreiben, Abfragen nach ID oder Filter, Standardfeldern, Beschränkungen und Abrufen von ECIDs.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
TQID: https://experienceleague.adobe.com/jZ-ecWTmHwq9gvp4fMaeuuGba6cgwYx0QCCyfkrEDHQ
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2728
ht-degree: 3%

---

# Leads

[Leads-Endpunktreferenz](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads)

Die Marketo Leads-API unterstützt CRUD-Vorgänge für Lead-Datensätze. Sie können auch die Zugehörigkeit eines Leads zu statischen Listen und Programmen ändern und die intelligente Kampagnenverarbeitung für Leads starten.

## beschreiben

Verwenden Sie Leads beschreiben , um die über die REST-API verfügbaren Felder und die Metadaten für die einzelnen Felder abzurufen:

- Datentyp
- REST API-Name
- Länge, falls zutreffend
- Schreibgeschützter Status
- Freundliche Kennzeichnung

Describe ist die primäre Quelle der Wahrheit für die Feldverfügbarkeit und Metadaten.

### Anfrage

```http
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

Tatsächliche Antworten enthalten mehr Felder im Ergebnis-Array. Jedes Element stellt ein für den Lead-Datensatz verfügbares Feld dar und enthält mindestens eine ID, einen displayName und einen Datentyp.

Die untergeordneten REST- und SOAP-Objekte werden nur angezeigt, wenn das Feld für die entsprechende API gültig ist. Die `readOnly`-Eigenschaft gibt an, ob die entsprechende API das Feld aktualisieren kann. Wenn vorhanden, gibt die Length-Eigenschaft die maximale Feldlänge an, und die DataType-Eigenschaft gibt den Datentyp des Felds an.

## Abfrage

Verwenden Sie eine von zwei primären Methoden zum Abrufen von Leads:

- Lead nach ID abrufen akzeptiert eine Lead-ID als Pfadparameter und gibt einen Lead-Datensatz zurück.
- Leads nach Filtertyp abrufen Sucht Datensätze, deren ausgewähltes Feld mit einem der angegebenen Werte übereinstimmt.

Bei „Lead nach ID abrufen“ können Sie optional einen Feldparameter mit einer kommagetrennten Liste von Feldnamen übergeben, die zurückgegeben werden sollen. Wenn in der Anfrage Felder ausgelassen werden, umfasst die Antwort `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` und `id`. Wenn ein angefordertes Feld nicht zurückgegeben wird, ist sein Wert impliziert null.

### Anfrage

```http
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

„Lead nach ID abrufen“ gibt immer einen Datensatz an der ersten Position des Ergebnis-Arrays zurück.

Leads nach Filtertyp abrufen gibt denselben Datensatztyp zurück und kann bis zu 300 Datensätze pro Seite zurückgeben. Die Abfrageparameter `filterType` und `filterValues` sind erforderlich.

`filterType` akzeptiert alle benutzerdefinierten Felder und die am häufigsten verwendeten Felder. Rufen Sie den Endpunkt `Describe2` auf, um die durchsuchbaren Felder abzurufen, die für `filterType` zulässig sind. Bei der Suche nach benutzerdefiniertem Feld werden die Datentypen `string`, `email` und `integer` unterstützt. Verwenden Sie die Describe-Methode, um Felddetails wie Beschreibung und Typ abzurufen.

`filterValues` akzeptiert bis zu 300 kommagetrennte Werte. Der Aufruf gibt Datensätze zurück, bei denen das ausgewählte Lead-Feld mit einem dieser Werte übereinstimmt. Wenn mehr als 1.000 Leads mit dem Filter übereinstimmen, gibt die API „1003, zu viele Ergebnisse stimmen mit dem Filter überein“ zurück.

Wenn die Gesamtzahl der GET-Anfragen 8 KB überschreitet, gibt die API unter RFC 7231 „414, URI zu lang“ zurück. Um dieses Limit zu umgehen, ändern Sie GET in POST, fügen Sie den Parameter _method=GET hinzu und legen Sie die Abfragezeichenfolge im Anfragetext fest.

### Anfrage

```http
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

Dieser Aufruf gibt Datensätze zurück, deren IDs mit den Werten in `filterValues` übereinstimmen.

Wenn keine Datensätze übereinstimmen, zeigt die Antwort Erfolg an und enthält ein leeres Ergebnis-Array.

### Antwort

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Sowohl Lead nach ID abrufen als auch Leads nach Filtertyp abrufen akzeptieren einen Feldabfrageparameter, der eine kommagetrennte Liste von API-Feldern enthält. Wenn Felder vorhanden sind, enthält jeder Antwortdatensatz die aufgelisteten Felder. Wenn sie ausgelassen wird, umfasst die Antwort `id`, `email`, `updatedAt`, `createdAt`, `firstName` und `lastName`.

## ADOBE ECID

Wenn die Zielgruppenfreigabe von Adobe Experience Cloud aktiviert ist, werden bei der Cookie-Synchronisierung Adobe Experience Cloud-ID-Werte (ECID) mit Marketo-Leads verknüpft. Um verknüpfte ECID-Werte mit den vorherigen Lead-Abrufmethoden abzurufen, schließen Sie `ecids` in den Feldparameter ein. Beispiel: `&fields=email,firstName,lastName,ecids`.

## Erstellen und aktualisieren

Die Lead-API kann Lead-Datensätze erstellen, aktualisieren und löschen. Vorgänge zum Erstellen und Aktualisieren verwenden denselben Endpunkt, wobei der Vorgangstyp in der Anfrage definiert ist. Eine Anfrage kann bis zu 300 Datensätze erstellen oder aktualisieren.

>[!NOTE]
>
> Die Aktualisierung von Unternehmensfeldern mit dem Endpunkt [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) wird nicht unterstützt. Verwenden [ stattdessen den Endpunkt ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST)Unternehmen synchronisieren“.

>[!NOTE]
>
> Beim Erstellen oder Aktualisieren des E-Mail-Werts in einem Personendatensatz werden nur ASCII-Zeichen im E-Mail-Adressfeld unterstützt.

### Anfrage

```http
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

Die Anfrage verwendet zwei wichtige Felder:

- `action` gibt den Vorgangstyp an: `createOrUpdate`, `createOnly`, `updateOnly` oder `createDuplicate`. Wenn sie ausgelassen wird, wird standardmäßig `createOrUpdate` verwendet.
- `lookupField` gibt den Schlüssel an, wenn die Aktion `createOrUpdate` oder `updateOnly` wird. Wenn sie ausgelassen wird, wird standardmäßig `email` verwendet.

Standardmäßig verwendet der Vorgang die Standardpartition. Der optionale `partitionName` funktioniert nur, wenn die Aktion `createOnly` oder `createOrUpdate` ist. Um `partitionName` als zusätzliche Deduplizierungskriterien zu verwenden, schließen Sie es in den Quelltyp für benutzerdefinierte Deduplizierungsregeln ein.

Während einer Aktualisierung gibt die API einen Fehler zurück, wenn der Lead nicht in der angegebenen Partition vorhanden ist oder wenn der Benutzer, der nur über eine API verfügt, nicht auf diese Partition zugreifen kann.

Da `id` ein vom System verwalteter eindeutiger Schlüssel ist, schließen Sie ihn nur mit der `updateOnly` ein.

Die Anfrage muss einen `input` enthalten, der ein Array von Lead-Datensätzen enthält. Jeder Lead-Datensatz ist ein JSON-Objekt mit einer beliebigen Anzahl von Lead-Feldern. Schlüssel müssen innerhalb jedes Datensatzes eindeutig sein, und alle JSON-Zeichenfolgen müssen UTF-8-Codierung verwenden.

Verwenden Sie `externalCompanyId`, um einen Lead-Datensatz mit einem Firmendatensatz zu verknüpfen. Verwenden Sie `externalSalesPersonId`, um einen Lead-Datensatz mit einem Verkaufspersonendatensatz zu verknüpfen.

Gleichzeitige oder zeitlich eng abgestimmte Upsert-Anfragen können doppelte Datensätze erstellen, wenn mehrere Anfragen denselben Schlüsselwert verwenden, bevor die erste Anfrage zurückgibt. Um Duplikate zu vermeiden, verwenden Sie je nach Bedarf `createOnly` oder `updateOnly`. Alternativ können Sie Aufrufe in die Warteschlange stellen und warten, bis jeder Aufruf zurückgegeben wird, bevor Sie eine weitere Upsert-Nachricht mit demselben Schlüssel senden.

## Felder

Das Lead-Objekt enthält Standardfelder und optionale benutzerdefinierte Felder. Standardfelder sind in jedem Marketo Engage-Abonnement enthalten, während Benutzerinnen und Benutzer nach Bedarf benutzerdefinierte Felder erstellen.

Jede Felddefinition enthält Metadatenattribute wie Anzeigename, API-Name und Datentyp.

Verwenden Sie die folgenden Endpunkte, um Felder im Lead-Objekt abzufragen, zu erstellen und zu aktualisieren. Die Rolle des API-Benutzers muss über die Berechtigung Standardfeld für Lese-/Schreibschemafelder, benutzerdefinierte Schemafelder für Lese-/Schreibzugriff oder über beides verfügen.

## Abfragefelder

Ein Lead-Feld nach API-Namen abfragen oder alle Lead-Felder abfragen. Abhängig von den Rollenberechtigungen kann die Antwort Standardfelder, benutzerdefinierte Felder und ausgeblendete Felder enthalten.

## Nach Name

Der Endpunkt Lead-Feld nach Name abrufen ruft Metadaten für ein Lead-Feld ab. Der erforderliche Pfadparameter fieldApiName gibt den API-Namen des Felds an.

Die Antwort ähnelt der Antwort von Describe Lead, enthält jedoch zusätzliche Metadaten. Beispielsweise gibt das Attribut isCustom an, ob es sich um ein benutzerdefiniertes Feld handelt.

### Anfrage

```http
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

Der Endpunkt Lead-Felder abrufen ruft Metadaten für alle Felder im Lead-Objekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Verwenden Sie den `batchSize` Abfrageparameter, um diese Zahl zu reduzieren.

Wenn `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Übergeben Sie die zurückgegebene `nextPageToken` bei jedem nachfolgenden Aufruf, bis `moreResult` „false“ ist.

### Anfrage

```http
GET /rest/v1/leads/schema/fields.json
```

### Antwort (gekürzt)

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

Der Endpunkt Lead-Felder erstellen erstellt ein oder mehrere benutzerdefinierte Felder im Lead-Objekt und bietet Funktionen, die mit der Marketo Engage-Benutzeroberfläche vergleichbar sind. Sie können mit diesem Endpunkt bis zu 100 benutzerdefinierte Felder erstellen.

Berücksichtigen Sie jedes Feld sorgfältig, bevor Sie es in einer Produktionsinstanz erstellen. Nachdem ein Feld erstellt wurde, können Sie es ausblenden, aber nicht löschen. Nicht verwendete Felder sorgen für Unordnung in der Instanz.

Der erforderliche Eingabeparameter ist ein Array von Lead-Feld-Objekten. Für jedes Objekt sind die folgenden Attribute erforderlich:

- `displayName` ist der Anzeigename der Benutzeroberfläche des Felds.
- `name` ist der API-Name des Felds.
- `dataType` ist der Feldtyp.

Optionale Attribute sind `description`, `isHidden`, `isHtmlEncodingInEmail` und `isSensitive`.

Das Namensattribut muss eindeutig sein, mit einem Buchstaben beginnen und nur Buchstaben, Zahlen oder Unterstriche enthalten. Der `displayName` muss eindeutig sein und darf keine Sonderzeichen enthalten.

Eine gängige Konvention wendet Binnenmajuskeln auf `displayName` an, um einen Namen zu produzieren. Beispiel: Eine `displayName` von „Mein benutzerdefiniertes Feld“ erzeugt den Namen „myCustomField“.

### Anfrage

```http
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

Der Endpunkt Lead-Feld aktualisieren aktualisiert ein benutzerdefiniertes Feld im Lead-Objekt. Die meisten in der Marketo Engage-Benutzeroberfläche verfügbaren Feldaktualisierungen sind auch über die API verfügbar. In der folgenden Tabelle sind die Unterschiede aufgeführt.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Attribut</strong></td>
<td style="width: 35%;" colspan="2"><strong>Standardfeld</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Benutzerdefiniertes Feld</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Von API aktualisierbar?</strong></td>
<td style="width: 17.551%;"><strong>Von der Benutzeroberfläche aktualisierbar?</strong></td>
<td style="width: 19.3878%;"><strong>Von API aktualisierbar?</strong></td>
<td style="width: 18.8776%;"><strong>Von der Benutzeroberfläche aktualisierbar?</strong></td>
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
<td style="width: 19.3878%;">Ja (wenn von API erstellt)</td>
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

Der erforderliche `fieldApiName` gibt den API-Namen des zu aktualisierenden Felds an. Der erforderliche Eingabeparameter ist ein Array, das ein Lead-Feldobjekt mit einem oder mehreren Attributen enthält.

### Anfrage

```http
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

Lead per Push übertragen ist eine Alternative zur Lead-Synchronisierung und bietet mehr Auslöseoptionen, ähnlich wie bei einem Marketo-Formular. Zusätzlich zur Synchronisierung von Lead-Feldern kann der Endpunkt einen Lead basierend auf einem Cookie-Wert verknüpfen. Übergeben Sie den `mkt_tok`, der durch einen Klick aus einer Marketo-E-Mail generiert wurde, oder übergeben Sie einen Programmnamen im Aufruf.

Der Endpunkt erstellt außerdem eine auslösbare Aktivität, die mit einem Marketo-Programm, einer-Kampagne oder beidem verknüpft ist. Verwenden Sie diese Aktivität, um Workflows aus Lead-Erfassungsereignissen zu starten, die einer bestimmten Kampagne oder einem bestimmten Programm zugeordnet wurden.

Lead per Push übertragen verwendet dieselben Primärschlüssel und Feld-API-Namen wie Leads synchronisieren. Es hat keinen Aktionsparameter, da es immer eine Upsert-Aktion ausführt.

Die `programName`- und Eingabeparameter sind erforderlich. Der Eingabeparameter ist ein Array von Lead-Objekten, und die resultierende Aktivität wird dem benannten Programm zugeordnet. Die Parameter `lookupField`, `source` und `reason` sind optional. Fügen Sie beliebige Zeichenfolgen in `source` und `reason` hinzu, um diese Werte in die resultierenden Aktivitäten einzuschließen. Sie können die Werte als Einschränkungen in den entsprechenden Triggern (Lead wird an Marketo gepusht) und Filtern (Lead wurde an Marketo gepusht) verwenden.

Um frühere anonyme Aktivitäten mit einem neu erstellten Lead zu verknüpfen, lassen Sie das Cookie-Attribut aus dem Lead-Objekt weg und rufen Sie Lead verknüpfen nach Lead-Push auf. Um einen Lead ohne Aktivitätsverlauf zu erstellen, geben Sie das Cookie-Attribut im Lead-Objekt an.

### Anfrage

```http
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
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
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

Um den `mkt_tok` zu übergeben, weisen Sie seinen Wert dem mktToken-Element in einem Lead-Datensatz innerhalb des Eingabeparameters zu.

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

Formular senden ist eine Alternative zur Synchronisierung von Leads und bietet die gleiche Funktionalität wie die Übermittlung eines Marketo-Formulars. Dient zum Starten von Workflows aus Lead-Erfassungsereignissen, die einer bestimmten Kampagne oder einem bestimmten Programm zugeordnet wurden.

Der Endpunkt „Formular senden“ unterstützt die folgenden Funktionen:

- Setzt einen Lead-Datensatz mithilfe des E-Mail-Felds als Primärschlüssel hoch.
- Erstellt eine Aktivität „Formular ausfüllen“, die mit einem Programm, einer Kampagne oder beidem verknüpft ist.
- Verknüpft einen Lead basierend auf einem Cookie-Wert.
- Überprüft Formularfelder.

Senden Sie ein Formular mit dem standardmäßigen Lead-Datenbankmuster. Übergeben Sie einen Objektdatensatz in das erforderliche Eingabeelement des JSON-Bodys der POST-Anfrage. Das erforderliche `formId` enthält die Marketo-Zielformular-ID.

Verwenden Sie die optionale `programId`, um das Programm zu identifizieren, das den Lead, die benutzerdefinierten Felder des Programmmitglieds oder beides erhält. Wenn `programId` vorhanden ist, wird der Lead zusammen mit allen Programmteilnehmerfeldern im Formular zum Programm hinzugefügt. Das Programm muss sich im selben Arbeitsbereich wie das Formular befinden.

Wenn das Formular keine benutzerdefinierten Felder für Programmmitglieder enthält und `programId` weggelassen wird, wird der Lead nicht zu einem Programm hinzugefügt. Wenn das Formular zu einem Programm gehört, ein oder mehrere benutzerdefinierte Felder für Programmmitglieder enthält und `programId` auslässt, verwendet der Endpunkt das Programm des Formulars.

Das erforderliche `leadFormFields`-Objekt enthält ein oder mehrere Name/Wert-Paare für die auszufüllenden Felder. Jedes Feld muss im angegebenen Formular definiert sein, und jeder Name muss der REST-API-Name des Felds sein. Das `email` ist ein Pflichtfeld.

Das optionale `visitorData`-Objekt enthält Daten zu Seitenbesuchen, einschließlich `pageURL`, `queryString`, `leadClientIpAddress` und `userAgentString`. Verwenden Sie diese Option, um zusätzliche Aktivitätsfelder für Filter und Trigger auszufüllen.

Der optionale Cookie-Member verknüpft ein Munchkin-Cookie mit einem Marketo-Personendatensatz. Wenn der Endpunkt einen Lead erstellt, werden diesem Lead frühere anonyme Aktivitäten zugeordnet, es sei denn, das Cookie war zuvor mit einem anderen bekannten Datensatz verknüpft.

Wenn das Cookie zuvor verknüpft war, werden neue Aktivitäten für den neuen Datensatz verfolgt, aber alte Aktivitäten verbleiben im vorhandenen bekannten Datensatz. Um einen Lead ohne Aktivitätsverlauf zu erstellen, lassen Sie das Cookie-Mitglied weg.

Neue Leads werden in der primären Partition für den Arbeitsbereich erstellt, in dem sich das Formular befindet.

### Anfrage

```http
POST /rest/v1/leads/submitForm.json
```

### Header

```text
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

Die folgende Abbildung zeigt die entsprechenden Details zur Aktivität „Formular ausfüllen“ in der Marketo Engage-Benutzeroberfläche:

![Formular-Benutzeroberfläche ausfüllen](assets/fill_out_form_activity_details.png)

## Zusammenführen

>[!NOTE]
>
>Ab dem 31. März 2026 führen Aufrufe, die mehr als 25 IDs im `leadIds`-Parameter eines Aufrufs der Zusammenführungs-Leads-API enthalten, zu einem 1080-Fehler-Code, und der Aufruf wird übersprungen. Aufträge, für die mehr als 25 Datensätze zu einem zusammengeführt werden müssen, sollten in mehrere Aufträge aufgeteilt werden, um den Erfolg dieser Aufrufe sicherzustellen.
>

Verwenden Sie die Zusammenführen-Leads-API, um doppelte Datensätze zu einem Datensatz zu kombinieren. Eine Zusammenführung kombiniert Aktivitätsprotokolle, Programm-, Kampagnen- und Listenmitgliedschaften, CRM-Informationen und Feldwerte.

Übergeben Sie die erfolgreichste Lead-ID als Pfadparameter. Übergeben Sie entweder eine `leadId` als Abfrageparameter oder bis zu 25 kommagetrennte IDs im `leadIds`.


### Anfrage

```http
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Antwort

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Der Lead im Pfadparameter ist der erfolgreichste Lead. Wenn Feldwerte in Konflikt stehen, verwendet die Zusammenführung den Wert des Gewinners, es sei denn, dieser Wert ist leer und der Wert des verlorenen Datensatzes ist nicht leer. Die Leads im `leadId`- oder `leadIds`-Parameter sind die verlorenen Leads.

Verwenden Sie für ein Abonnement mit aktivierter SFDC-Synchronisierung den `mergeInCRM`, um die Zusammenführung auch im CRM durchzuführen. Wenn sich beide Datensätze in SFDC befinden und einer ein CRM-Lead ist, während der andere ein CRM-Kontakt ist, gewinnt der CRM-Kontakt unabhängig vom angegebenen Gewinner. Wenn sich ein Datensatz in SFDC befindet und der andere nur in Marketo vorhanden ist, gewinnt der SFDC-Lead unabhängig vom angegebenen Gewinner.

## Web-Aktivität verknüpfen

Lead-Tracking (Munchkin) zeichnet Besuche und Klicks von Besuchern Ihrer Website und Marketo-Landingpages auf. Diese Aktivitäten verwenden einen Schlüssel, der dem Cookie „_mkto_trk“ im Browser des Leads entspricht, sodass Marketo die Aktivitäten derselben Person verfolgen kann.

Die Verknüpfung mit einem Lead-Datensatz erfolgt in der Regel, wenn ein Lead einem Link aus einer Marketo-E-Mail folgt oder ein Marketo-Formular sendet. Um einen Lead nach einem anderen Ereignistyp zu verknüpfen, verwenden Sie den Endpunkt Lead verknüpfen . Übergeben Sie die bekannte Lead-Datensatz-ID als Pfadparameter und den Cookie-Wert „_mkto_trk“ im Cookie-Abfrageparameter.

### Anfrage

```http
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Antwort

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Wenn das Cookie bereits mit einem bekannten Lead verknüpft ist, zeichnet die Verwendung dieser API für einen anderen Lead eine neue Web-Aktivität für den neuen Datensatz auf. Vorhandene Web-Aktivität wird nicht in den neuen Datensatz verschoben.
Mitgliedschaft

Abrufen von Lead-Datensätzen basierend auf der Mitgliedschaft in einer statischen Liste oder einem Programm. Sie können auch alle statischen Listen, Programme oder Smart-Kampagnen abrufen, die einen bestimmten Lead enthalten.

Die Antwortstruktur und die optionalen Parameter stimmen mit den GET-Leads nach Filtertyp überein, aber diese API akzeptiert keine `filterType` oder `filterValues`.

Um die Listen-ID in der Marketo-Benutzeroberfläche zu finden, navigieren Sie zur Liste und überprüfen Sie deren URL. `https://app-****.marketo.com/#ST1001A1` ist 1001 die `id`.

## Abrufen von Programmen nach Lead-ID

### Anfrage

```http
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

## Abrufen von Listen nach Lead-ID

Der Endpunkt Listen nach Lead-ID abrufen nimmt einen Lead-Datensatz `id` Pfadparameter und gibt jede statische Liste zurück, die den Lead enthält.

### Anfrage

```http
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

Rufen Sie die Programmmitgliedschaft auf die gleiche Weise wie die Listenmitgliedschaft ab. Das Abrufen von Leads nach Programm-ID akzeptiert dieselben optionalen Anfrageparameter und erfordert den `programId`.

Optional können Sie einen Feldparameter übergeben, der eine kommagetrennte Liste von Feldnamen enthält. Wenn Felder ausgelassen werden, umfasst die Antwort `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` und `id`. Wenn ein angefordertes Feld nicht zurückgegeben wird, ist sein Wert impliziert null.

Jedes Element im Ergebnis-Array ist ein Lead mit einem untergeordneten Objekt namens „Mitgliedschaft“. Dieses Objekt beschreibt die Beziehung des Leads zum angeforderten Programm und umfasst immer `progressionStatus`, `acquiredBy`, `reachedSuccess` und `membershipDate`.

Wenn das übergeordnete Programm ein Interaktionsprogramm ist, umfasst die Mitgliedschaft auch `stream`, `nurtureCadence` und `isExhausted`, um die Position und Aktivität des Leads in diesem Programm zu beschreiben.

### Anfrage

```http
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

Der Endpunkt „Programme nach Lead-ID abrufen“ nimmt den Pfadparameter für die Lead-Datensatz-ID an und gibt jedes Programm zurück, das den Lead enthält. Verwenden Sie die optionalen `filterType`- und `filterValues`, um nach Programm-ID zu filtern.

### Anfrage

```http
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

Der Endpunkt Smart-Kampagnen nach Lead-ID abrufen nimmt den Pfadparameter ID des Lead-Datensatzes und gibt jede Smart-Kampagne zurück, die den Lead enthält.

### Anfrage

```http
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

Verwenden Sie den Endpunkt Leads löschen , um Lead-Datensätze zu entfernen. Geben Sie die Lead-IDs im Hauptteil mit den ID-Attributen an. Eine Anfrage kann bis zu 300 Leads löschen. Senden Sie die Kopfzeile Content-Type: application/json .

### Anfrage

```http
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

- Unternehmen über das Feld externalCompanyId im Lead-Datensatz
- SalesPersons über das Feld externalSalesPersonId im Lead-Datensatz
- Programme durch Programmmitgliedschaft
- Listen über Listenmitgliedschaft
- Aktivitäten über das Feld leadId in der Aktivität
- Segmentierung durch einzelne Segmentfelder im Lead-Datensatz
- Partitionen über das Feld leadPartitionId im Lead-Datensatz

## Zeitüberschreitungen

Lead-Endpunkte haben eine Zeitüberschreitung von 30 Sekunden, mit Ausnahme der folgenden Endpunkte:

- Leads synchronisieren: 90er
- Associate Lead: 60er
- Zusammenführen von Leads: 180er
- Lead-Partition aktualisieren: 60s
- Lead auf Marketo pushen: 90er
- Leads nach Filtertyp abrufen: 60s
- Leads abrufen nach Listen-ID: 60 Jahre
