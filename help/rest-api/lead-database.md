---
title: Lead-Datenbank
feature: REST API, Database
description: Bearbeiten Sie die Hauptdatenbank für Leads.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 1%

---

# Lead-Datenbank

Die Marketo-Lead-Datenbank-APIs sind die am häufigsten verwendeten APIs, die Marketo bereitstellt, da sie den Datenaustausch von Personen- und personenbezogenen Daten aus Marketo ermöglichen, z. B. Aktivitäten, Chancen und Unternehmen.

## Objekte

Lead-Datenbankobjekte umfassen Folgendes:

- Leads
- Firmen/Konten
- Benannte Konten
- Opportunitys
- OpportunityRoles
- Vertriebspersonen
- Benutzerdefinierte Objekte
- Aktivitäten
- Liste und Programmmitgliedschaft

Die meisten dieser Objekte umfassen mindestens die Methoden Create, Read, Update und Delete. Ebenfalls enthalten ist eine Methode „Describe“ (Beschreiben), die für jeden Typ eine Liste der verfügbaren Felder sowie eine Liste der Felder bereitstellt, die für die Deduplizierung verwendet werden (für Nicht-Lead-Objekte) und welche Felder zum Abrufen von Datensätzen durchsuchbar sind. Der umfangreichste Satz wird für Leads bereitgestellt, da sie über die größte Vielfalt an Funktionen in Marketo-Anwendungen verfügen.

## API

Eine vollständige Liste der API-Endpunkte für Lead-Datenbanken, einschließlich Parametern und Modellierungsinformationen, finden Sie in der [Lead Database API Endpoint Reference](https://developer.adobe.com/marketo-apis/api/mapi/).

Bei Instanzen mit aktivierter nativer CRM-Integration (entweder Microsoft Dynamics oder Salesforce.com) sind die APIs für Unternehmen, Opportunity, Opportunity und Vertriebsperson deaktiviert. Die Datensätze werden, wenn sie aktiviert sind, über das CRM verwaltet und können nicht über die APIs von Marketo aufgerufen oder aktualisiert werden.

- Max. Batch-Größe (Standard): 300 Datensätze
- Max. Batch-Größe (Bulk): 10 MB Datei
- Standard-Batch-Größe: 300 Datensätze
- Inhaltstyp-Header (Standard): application/json
- Inhaltstyp-Header (Bulk): multipart/form-data

## beschreiben

Für Leads, Unternehmen, Opportunities, Rollen, Vertriebspersonen und benutzerdefinierte Objekte wird eine API mit einer Beschreibung bereitgestellt. Durch diesen Aufruf werden Metadaten für das -Objekt sowie eine Liste der Felder, die zum Aktualisieren und Abfragen verfügbar sind, abgerufen. Beschreiben ist ein wichtiger Teil beim Entwerfen einer ordnungsgemäßen Integration mit Marketo. Es bietet umfangreiche Metadaten darüber, wie Objekte interagiert werden können und nicht, und wie sie erstellt, aktualisiert und abgefragt werden können. Außer Leads beschreiben gibt jeder von diesen eine Liste von Schlüsseln zurück, die im `deduplication`-Antwortparameter für die `dedupeFields` verfügbar sind. Eine Liste von Feldern ist als Schlüssel für die Abfrage im `searchableFields`-Antwortparameter verfügbar.

```
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

In diesem Beispiel ist `dedupeFields` eigentlich ein zusammengesetzter Schlüssel. Das bedeutet, dass Sie bei zukünftigen Aktualisierungen und Erstellungen bei Verwendung des `dedupeFields`-Modus alle drei `externalOpportunityId`, `leadId` und `role` für jede Rolle einbeziehen müssen. Das `searchableFields`-Array stellt auch die Liste der Felder bereit, die für die Abfrage von Rollendatensätzen verfügbar sind. Dazu gehören auch die zusammengesetzten Schlüssel `externalOpportunityId`, `leadId` und `role`.

Es gibt auch einen Feldantwort-Parameter, der den Namen jedes Felds, die `displayName`, wie sie in der Marketo-Benutzeroberfläche angezeigt werden, den Datentyp des Felds, ob es nach der Erstellung aktualisiert werden kann, und die Länge des Felds, falls zutreffend, angibt.

## Abfrage

Lead-Datenbankobjekte verwenden alle dasselbe grundlegende Muster für Abfragen anhand einfacher Schlüssel, bei denen nur ein Feld referenziert wird.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Für alle Objekte außer Leads können Sie Ihre {field to query} aus den durchsuchbaren Feldern des entsprechenden Describe-Aufrufs auswählen und eine kommagetrennte Liste mit bis zu 300 Werten erstellen. Es gibt auch diese optionalen Abfrageparameter:

- `batchSize` : Eine ganzzahlige Anzahl der zurückzugebenden Ergebnisse. Standard und Maximum sind 300.
- `nextPageToken` - Token, das von einem vorherigen Paging-Aufruf zurückgegeben wurde. Weitere Informationen finden [ unter ](paging-tokens.md)Paging-Token“.
- `fields` : Eine kommagetrennte Liste von Feldnamen, die für jeden Datensatz zurückgegeben werden sollen. Eine Liste der gültigen Felder finden Sie in der entsprechenden Beschreibung. Wenn ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, ist der Wert impliziert null.
- `_method` - Wird zum Senden von Abfragen mithilfe der POST-HTTP-Methode verwendet. Weitere Informationen zur Verwendung finden Sie im Abschnitt _method=GET unten.

Ein kurzes Beispiel finden Sie unter Abfragemöglichkeiten:

```
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

Der in diesem Aufruf angegebene `filterType` lautet „idField“ und nicht „marketoGUID“. Dies und „dedupeFields“ sind beides Sonderfälle, in denen das Feld, das dem idField, bzw. dedupeFields entspricht, auf diese Weise aliasiert werden kann. Die „marketoGUID“ ist weiterhin das resultierende Suchfeld im Aufruf, wird aber nicht explizit im Aufruf festgelegt. Die Felder und/oder Feldgruppen, die durch die `idField` und `dedupeFields` einer Objektbeschreibung angegeben werden, sind immer `filterTypes` für eine Abfrage gültig. Dieser Aufruf sucht nach Datensätzen, die mit den in filterValues enthaltenen GUIDs übereinstimmen, und gibt übereinstimmende Datensätze zurück. Wenn mit dieser Methode keine Datensätze gefunden werden, zeigt die Antwort weiterhin Erfolg an, aber das Ergebnis-Array ist leer, da die Suche erfolgreich ausgeführt wurde, aber es gab keine zurückzugebenden Datensätze.

Wenn die Menge der Datensätze in der Abfrage 300 überschreitet oder die angegebene `batchSize` überschreitet, je nachdem, welcher Wert kleiner ist, verfügt die Antwort über einen `moreResult` mit dem Wert „true“ und einen `nextPageToken`, der in einen nachfolgenden Aufruf eingeschlossen werden kann, um einen größeren Teil der Menge abzurufen. Weitere Informationen finden [ unter ](paging-tokens.md)Paging-Token“.

### Lange URIs

Manchmal, z. B. bei der Abfrage anhand von GUIDs, kann der URI lang sein und die vom REST-Service zulässigen 8 KB überschreiten. In diesem Fall müssen Sie die HTTP-POST-Methode anstelle von GET verwenden und einen `_method=GET` für Abfrageparameter hinzufügen. Darüber hinaus müssen die übrigen Abfrageparameter im POST-Text als Zeichenfolge „application/x-www-form-urlencoded“ übergeben werden und die zugehörige Kopfzeile für den Inhaltstyp übergeben werden.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Neben langen URIs ist dieser Parameter auch bei der Abfrage von zusammengesetzten Schlüsseln erforderlich.

### Zusammengesetzte Schlüssel

Das Muster für die Abfrage von zusammengesetzten Schlüsseln unterscheidet sich von einfachen Schlüsseln, da eine POST mit einem JSON-Hauptteil gesendet werden muss. Dies ist nicht in allen Fällen erforderlich, sondern nur in den Fällen, in denen eine `dedupeFields` Option mit mehreren Feldern als `filterType` verwendet wird. Derzeit werden zusammengesetzte Schlüssel nur von Opportunity-Rollen und einigen benutzerdefinierten Objekten verwendet. Sehen wir uns ein Beispiel einer Abfrage für Opportunity-Rollen mit dem zusammengesetzten Schlüssel aus `dedupeFields` an:

```
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[
      {
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Die Struktur des JSON-Objekts ist größtenteils flach, und alle Abfrageparameter für Abfragen mit einfachen Schlüsseln sind gültige Member, mit Ausnahme von `filterValues`. Anstelle eines Filterwerts gibt es ein „Eingabe“-Array von JSON-Objekten, von denen jedes ein Element für jedes der Felder in Ihrem zusammengesetzten Schlüssel haben muss. In diesem Fall sind dies `externalOpportunityId`, `leadId` und `role`. Dadurch wird eine Abfrage für `roles` anhand der bereitgestellten Eingaben ausgeführt und die passenden Ergebnisse zurückgegeben. Wenn die Antwort einen -Parameter mit `moreResult=true` und einen -`nextPageToken` zurückgibt, müssen Sie alle ursprünglichen Eingaben und den `nextPageToken` einbeziehen, damit die Abfrage ordnungsgemäß ausgeführt wird.

## Erstellen und aktualisieren

Erstellt und aktualisiert Lead-Datenbank-Datensätze. Dies geschieht alles über POSTs mit JSON-Bodies. Die Benutzeroberfläche für Opportunities, Rollen, benutzerdefinierte Objekte, Unternehmen und Vertriebspersonen ist jeweils identisch. Die Benutzeroberfläche des Leads ist ein wenig anders, und Sie können dort mehr darüber lesen.

Der einzige erforderliche Parameter ist ein Array namens `input` mit bis zu 300 Objekten, von denen jedes die Felder enthält, die Sie als Member einfügen/aktualisieren möchten. Optional können Sie auch einen `action` Parameter einbeziehen, der einer der folgenden sein kann: `createOnly`, `updateOnly` oder `createOrUpdate`. Wenn die Aktion ausgelassen wird, ist der Modus standardmäßig `createOrUpdate`. `dedupeBy` ist ein weiterer optionaler Parameter, der verwendet werden kann, wenn für die Aktion entweder „createOnly“ oder &quot;`createOrUpdate`&quot; festgelegt ist. ` dedupeBy` kann entweder `idField` oder `dedupeFields` sein. Wenn `idField` ausgewählt ist, wird die in der Beschreibung aufgeführte `idField` für die Deduplizierung verwendet und muss in jedem Datensatz enthalten sein. `idField` Modus ist nicht mit dem `createOnly` Modus kompatibel. Wenn `dedupeFields` ausgewählt sind, werden die in der verwendeten Objektbeschreibung aufgelisteten `dedupeFields` verwendet, und jede einzelne muss in jedem Datensatz enthalten sein. Wenn der `dedupeBy` Parameter weggelassen wird, ist der Modus standardmäßig `dedupeFields`.

Beim Übergeben einer Liste von Feldwerten wird ein Wert von `null` oder eine leere Zeichenfolge wie `null` in die Datenbank geschrieben.

```
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

Anders als die Lead-API geben Aufrufe zum Erstellen oder Aktualisieren von Lead-Datenbankobjekten ein `seq` Feld in jedem Objekt im `result`-Array zurück. Die aufgeführte Zahl entspricht der Reihenfolge des aktualisierten Datensatzes in der Anfrage. Jedes Element gibt den Wert der `idField` für den Objekttyp und eine `status` zurück. Das Statusfeld gibt entweder „Erstellt“, „Aktualisiert“ oder „Übersprungen“ an.  Wenn der Status übersprungen wird, gibt es auch ein entsprechendes „Ursachen“-Array mit einem oder mehreren Ursachenobjekten, das einen Code und eine Meldung enthält, die angibt, warum ein Datensatz übersprungen wurde. Siehe [Fehlercodes](error-codes.md) für weitere Details.

### Löschen

Die Benutzeroberfläche für Löschvorgänge ist standardmäßig für Lead-Datenbankobjekte neben Leads. Neben der Eingabe gibt es nur einen erforderlichen Parameter `deleteBy,` , der den Wert idField oder dedupeFields haben kann. Sehen wir uns das Löschen einiger benutzerdefinierter Objekte an.

```
POST /rest/v1/customobjects/{name}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Die `seq`, `status`, `marketoGUID` und `reasons` sollten Ihnen mittlerweile alle bekannt sein.

Weitere Informationen zum Arbeiten mit CRUD-Vorgängen für jeden einzelnen Objekttyp finden Sie auf den entsprechenden Seiten.
