---
title: Lead-Datenbank
feature: REST API, Database
description: Bearbeiten Sie die Haupt-Lead-Datenbank.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 1%

---

# Lead-Datenbank

Die Marketo-Lead-Datenbank-APIs sind die am häufigsten verwendeten APIs, die Marketo bereitstellt, da sie den Datenaustausch zwischen personenbezogenen Daten aus Marketo, wie z. B. Aktivitäten, Chancen und Unternehmen, ermöglichen.

## Objekte

Lead-Datenbankobjekte umfassen Folgendes:

- Leads
- Unternehmen/Konten
- Genannte Konten
- Opportunitys
- OpportunityRoles
- SalesPersons
- Benutzerdefinierte Objekte
- Aktivitäten
- Liste und Programmmitgliedschaft

Die meisten dieser Objekte umfassen mindestens die Methoden &quot;Erstellen&quot;, &quot;Lesen&quot;, &quot;Aktualisieren&quot;und &quot;Löschen&quot;. Dazu gehört auch die Methode &quot;Beschreiben&quot;, die eine Liste der verfügbaren Felder für jeden Typ bereitstellt, eine Liste der Felder, die zur Deduplizierung verwendet werden (für Objekte ohne Lead), und welche Felder zum Abrufen von Datensätzen durchsucht werden können. Die umfassendste Auswahl wird für Leads bereitgestellt, da sie die unterschiedlichsten Funktionen innerhalb von Marketo-Anwendungen bieten.

## API

Eine vollständige Liste der API-Endpunkte für die Lead-Datenbank, einschließlich Parametern und Modellierungsinformationen, finden Sie in der [Referenz zum Lead-Datenbank-API-Endpunkt](https://developer.adobe.com/marketo-apis/api/mapi/).

Bei Instanzen mit aktivierter nativer CRM-Integration (entweder Microsoft Dynamics oder Salesforce.com) sind die APIs für Unternehmen, Chancen, Chancen und Vertriebsmitarbeiter deaktiviert. Die Datensätze werden bei Aktivierung über das CRM verwaltet und können nicht über die APIs von Marketo aufgerufen oder aktualisiert werden.

- Maximale Stapelgröße (Standard): 300 Datensätze
- Maximale Batch-Größe (Massen): 10 MB Datei
- Standard-Stapelgröße: 300 Datensätze
- Content-Type-Kopfzeile (Standard): application/json
- Content-type header (bulk): multipart/form-data

## Beschreibung

Für Leads, Unternehmen, Chancen, Rollen, SalesPersons und benutzerdefinierte Objekte wird eine API mit Beschreibungen bereitgestellt. Durch diesen Aufruf werden Metadaten für das Objekt sowie eine Liste der Felder abgerufen, die aktualisiert und abgefragt werden können. Die Beschreibung ist ein wesentlicher Bestandteil der Entwicklung einer ordnungsgemäßen Integration mit Marketo. Es bietet umfassende Metadaten darüber, wie Objekte interagiert werden können und wie sie erstellt, aktualisiert und abgefragt werden können. Abgesehen von &quot;Leads beschreiben&quot;gibt jede dieser Elemente eine Liste von Schlüsseln zurück, die für `deduplication` im Antwortparameter `dedupeFields` verfügbar sind. Eine Liste von Feldern ist als Schlüssel für die Abfrage im Antwortparameter `searchableFields` verfügbar.

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

In diesem Beispiel ist `dedupeFields` eigentlich ein zusammengesetzter Schlüssel. Dies bedeutet, dass Sie bei Verwendung des Modus `dedupeFields` in Zukunft alle drei Elemente `externalOpportunityId`, `leadId` und `role` für jede Rolle einbeziehen müssen und erstellen. Das Array `searchableFields` enthält auch die Liste der Felder, die zum Abfragen von Rollendatensätzen verfügbar sind. Dazu gehören auch der zusammengesetzte Schlüssel `externalOpportunityId`, `leadId` und `role`.

Es gibt auch einen Antwortparameter für Felder, der den Namen jedes Felds, den `displayName`, wie er in der Marketo-Benutzeroberfläche angezeigt wird, den Datentyp des Felds, den Datentyp, ob er nach der Erstellung aktualisiert werden kann, und gegebenenfalls die Feldlänge angibt.

## Anfrage

Lead-Datenbankobjekte weisen alle grundlegenden Muster für Abfragen anhand einfacher Schlüssel auf, wobei nur auf ein Feld verwiesen wird.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Bei allen Objekten außer Leads können Sie Ihr {Feld zur Abfrage} aus den durchsuchbaren Feldern des entsprechenden Beschreibungsaufrufs auswählen und eine kommagetrennte Liste mit bis zu 300 Werten erstellen. Es gibt auch folgende optionale Abfrageparameter:

- `batchSize` - Eine ganzzahlige Zahl der zurückzugebenden Ergebnisse. Der Standardwert und der Maximalwert sind 300.
- `nextPageToken` - Token, das von einem vorherigen Paging-Aufruf zurückgegeben wird. Weitere Informationen finden Sie unter [Paging-Token](paging-tokens.md) .
- `fields` - Eine kommagetrennte Liste von Feldnamen, die für jeden Datensatz zurückgegeben werden sollen. Eine Liste der gültigen Felder finden Sie in der entsprechenden Beschreibung. Wenn ein bestimmtes Feld angefordert, aber nicht zurückgegeben wird, wird der Wert als null impliziert.
- `_method` - Wird zum Senden von Abfragen mit der POST-HTTP-Methode verwendet. Siehe den Abschnitt _method=GET weiter unten zur Verwendung.

Sehen wir uns für ein kurzes Beispiel die Abfragemöglichkeiten an:

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

Die in diesem Aufruf angegebene `filterType` ist &quot;idField&quot; und nicht &quot;marketoGUID&quot;. Sowohl dies als auch &quot;dedupeFields&quot;sind Sonderfälle, bei denen das Feld, das dem idField oder dedupeFields entspricht, auf diese Weise als Alias bereitgestellt werden kann. Die &quot;marketoGUID&quot;ist weiterhin das resultierende Suchfeld im -Aufruf, wird jedoch nicht explizit im -Aufruf festgelegt. Die Felder und/oder Feldgruppen, die durch die Werte `idField` und `dedupeFields` einer Objektbeschreibung angegeben werden, sind für eine Abfrage immer gültig `filterTypes`. Dieser Aufruf sucht nach Datensätzen, die mit den in filterValues enthaltenen GUIDs übereinstimmen, und gibt übereinstimmende Datensätze zurück. Wenn mit dieser Methode keine Datensätze gefunden werden, zeigt die Antwort weiterhin Erfolg an. Das Ergebnis-Array ist jedoch leer, da die Suche erfolgreich ausgeführt wurde, aber keine Datensätze zurückgegeben werden konnten.

Wenn der Datensatz in der Abfrage größer als 300 oder der angegebene `batchSize` ist (je nachdem, welcher Wert kleiner ist), hat die Antwort das Mitglied `moreResult` mit dem Wert &quot;true&quot;und ein `nextPageToken`, das in einen nachfolgenden Aufruf aufgenommen werden kann, um mehr von der Menge abzurufen. Weitere Informationen finden Sie unter [Paging-Token](paging-tokens.md) .

### Lange URIs

Manchmal kann der URI, z. B. bei der Abfrage durch GUIDs, lang sein und die vom REST-Dienst zulässige Größe von 8 KB überschreiten. In diesem Fall müssen Sie die HTTP-POST-Methode anstelle von GET verwenden und den Abfrageparameter `_method=GET` hinzufügen. Darüber hinaus müssen die übrigen Abfrageparameter als Zeichenfolge &quot;application/x-www-form-urlencoded&quot;im Hauptteil der POST übergeben und die zugehörige Kopfzeile vom Typ Inhalt übergeben werden.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Neben langen URIs ist dieser Parameter auch beim Abfragen von ebenenübergreifenden Schlüsseln erforderlich.

### Zusammengesetzte Schlüssel

Das Muster zum Abfragen von zusammengesetzten Schlüsseln unterscheidet sich von einfachen Schlüsseln, da es das Senden einer POST mit einem JSON-Hauptteil erfordert. Dies ist nicht in allen Fällen erforderlich, nur in den Fällen, in denen eine `dedupeFields` -Option mit mehreren Feldern als `filterType` verwendet wird. Derzeit werden zusammengesetzte Schlüssel nur von Opportunity-Rollen und einigen benutzerdefinierten Objekten verwendet. Sehen wir uns ein Beispiel einer Abfrage für Opportunity Roles mit dem zusammengesetzten Schlüssel von `dedupeFields` an:

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

Die Struktur des JSON-Objekts ist größtenteils flach, und alle Abfrageparameter für Abfragen mit einfachen Schlüsseln sind gültige Mitglieder, mit Ausnahme von `filterValues`. Anstelle eines Filterwerts gibt es ein &quot;Eingabe&quot;-Array von JSON-Objekten, die jeweils ein Mitglied für jedes Feld im zusammengesetzten Schlüssel haben müssen. In diesem Fall handelt es sich um `externalOpportunityId`, `leadId` und `role`. Dadurch wird eine Abfrage für `roles` ausgeführt, die die bereitgestellten Eingaben berücksichtigt und die entsprechenden Ergebnisse zurückgegeben. Wenn die Antwort einen Parameter mit &quot;`moreResult=true`&quot; und &quot;`nextPageToken`&quot; zurückgibt, müssen Sie alle ursprünglichen Eingaben und die &quot;`nextPageToken`&quot;einschließen, damit die Abfrage ordnungsgemäß ausgeführt wird.

## Erstellen und Aktualisieren

Erstellt und aktualisiert Lead-Datenbankdatensätze, die alle über POSTs mit JSON-Stellen durchgeführt werden. Die Benutzeroberfläche für Chancen, Rollen, benutzerdefinierte Objekte, Unternehmen und SalesPersons ist identisch. Die Oberfläche des Leads ist etwas anders, und Sie können dort mehr darüber lesen.

Der einzig erforderliche Parameter ist ein Array namens `input` mit bis zu 300 Objekten, von denen jedes die Felder enthält, die Sie als Mitglieder einfügen/aktualisieren möchten. Sie können optional auch einen `action` -Parameter einbeziehen, der einer der folgenden sein kann: `createOnly`, `updateOnly` oder `createOrUpdate`. Wenn die Aktion weggelassen wird, ist der Modus standardmäßig `createOrUpdate`. `dedupeBy` ist ein weiterer optionaler Parameter, der verwendet werden kann, wenn die Aktion auf &quot;createOnly&quot;oder &quot;1&quot;festgelegt ist. `createOrUpdate` ` dedupeBy` kann entweder `idField` oder `dedupeFields` sein. Wenn `idField` ausgewählt ist, wird die in der Beschreibung aufgeführte `idField` zur Deduplizierung verwendet und muss in jedem Datensatz enthalten sein. Der Modus `idField` ist nicht mit dem Modus `createOnly` kompatibel. Wenn `dedupeFields` ausgewählt ist, muss der in der verwendeten Objektbeschreibung aufgeführte `dedupeFields`-Wert enthalten sein und jeder einzelne muss in jedem Datensatz enthalten sein. Wenn der Parameter `dedupeBy` weggelassen wird, ist der Modus standardmäßig `dedupeFields`.

Beim Übergeben einer Liste von Feldwerten wird der Wert `null` oder eine leere Zeichenfolge als `null` in die Datenbank geschrieben.

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

Anders als bei der Leads-API geben Aufrufe zum Erstellen oder Aktualisieren von Lead-Datenbankobjekten in jedem Objekt im Array `result` ein Feld `seq` zurück. Die aufgeführte Zahl entspricht der Reihenfolge des aktualisierten Datensatzes in der Anfrage. Jedes Element gibt den Wert von `idField` für den Objekttyp und eine `status` zurück. Das Statusfeld zeigt &quot;created&quot;, &quot;updated&quot;oder &quot;skipped&quot; an.  Wenn der Status übersprungen wird, gibt es auch ein entsprechendes &quot;Grund&quot;-Array mit einem oder mehreren &quot;Grund&quot;-Objekten, die einen Code und eine Meldung enthalten, die angibt, warum ein Datensatz übersprungen wurde. Weitere Informationen finden Sie unter [Fehlercodes](error-codes.md) .

### Löschen

Die Benutzeroberfläche für das Löschen ist standardmäßig für Lead-Datenbankobjekte, die von Leads abweichen. Neben der Eingabe gibt es nur einen erforderlichen Parameter `deleteBy,`, der den Wert idField oder dedupeFields haben kann. Sehen wir uns das Löschen einiger benutzerdefinierter Objekte an.

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

Die `seq`, `status`, `marketoGUID` und `reasons` sollten euch jetzt alle vertraut sein.

Weitere Informationen zum Arbeiten mit CRUD-Vorgängen für jeden einzelnen Objekttyp finden Sie auf den entsprechenden Seiten.
