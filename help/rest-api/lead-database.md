---
title: Lead-Datenbank
feature: REST API, Database
description: Handbuch zu Marketo-Lead-Datenbank-APIs mit Informationen zu Objekten, CRUD-Methoden und beschreibenden Methoden, Abfragemustern, Batch-Beschränkungen und CRM-Integrationsbeschränkungen.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
TQID: https://experienceleague.adobe.com/7lGbhE92lvIE-XkMyUIaK9GrreZVRdM-WVZTpHARhxE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1058
ht-degree: 1%

---

# Lead-Datenbank

Die Marketo-Lead-Datenbank-APIs tauschen Personen- und personenbezogene Daten mit Marketo aus. Diese Daten umfassen Aktivitäten, Chancen und Unternehmen.

## Objekte

Die Lead-Datenbank enthält die folgenden Objekte:

- Leads
- Firmen/Konten
- Benannte Konten
- Opportunitys
- OpportunityRoles
- Vertriebspersonen
- Benutzerdefinierte Objekte
- Aktivitäten
- Liste und Programmmitgliedschaft

Die meisten Lead-Datenbankobjekte unterstützen die Methoden Create, Read, Update und Delete. Die Describe-Methode stellt die verfügbaren Felder für jeden Objekttyp bereit. Bei Nicht-Lead-Objekten werden auch Felder für die Deduplizierung und Felder, die beim Abrufen von Datensätzen durchsuchbar sind, identifiziert.

Lead-Objekte unterstützen die meisten Funktionen, da Leads in Marketo-Anwendungen die größte Anwendungsvielfalt aufweisen.

## API

Eine vollständige Liste der Endpunkte, Parameter und Modellierungsinformationen für die Lead-Datenbank-API finden Sie in der [Referenz zur Lead-Datenbank-API](https://developer.adobe.com/marketo-apis/api/mapi).

Wenn eine Instanz über eine native Microsoft Dynamics- oder Salesforce.com CRM-Integration verfügt, sind die APIs für Unternehmen, Opportunity, Opportunity und Vertriebsperson deaktiviert. Das CRM verwaltet diese Datensätze, sodass Sie nicht über Marketo-APIs auf sie zugreifen oder sie aktualisieren können.

- Max. Batch-Größe (Standard): 300 Datensätze
- Max. Batch-Größe (Bulk): 10 MB Datei
- Standard-Batch-Größe: 300 Datensätze
- Inhaltstyp-Header (Standard): application/json
- Inhaltstyp-Header (Bulk): multipart/form-data

## beschreiben

Die Beschreiben-API ist für Leads, Unternehmen, Opportunities, Rollen, Vertriebspersonen und benutzerdefinierte Objekte verfügbar. Verwenden Sie diese Option, um Objektmetadaten und die für Aktualisierungen und Abfragen verfügbaren Felder abzurufen.

Mit Ausnahme von „Leads beschreiben“ gibt jeder „Endpunkt beschreiben“ Folgendes zurück:

- `dedupeFields`: Schlüssel für die Deduplizierung verfügbar.
- `searchableFields`: Für Abfragen verfügbare Schlüssel.

```http
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

In diesem Beispiel ist `dedupeFields` ein zusammengesetzter Schlüssel. Wenn Sie den `dedupeFields` für zukünftige Erstellungen und Aktualisierungen verwenden, schließen Sie `externalOpportunityId`, `leadId` und `role` für jede Rolle ein.

Das `searchableFields`-Array listet die Felder auf, die für die Abfrage von Rollendatensätzen verfügbar sind. Diese Liste enthält den zusammengesetzten Schlüssel `externalOpportunityId`, `leadId` und `role`.

Der `fields`-Antwortparameter stellt die folgenden Informationen für jedes Feld bereit:

- Name.
- `displayName` wie in der Benutzeroberfläche von Marketo gezeigt.
- Datentyp.
- Ob das Feld nach der Erstellung aktualisiert werden kann.
- Feldlänge, falls zutreffend.

## Abfrage

Lead-Datenbankobjekte verwenden ein grundlegendes Abfragemuster für einfache Schlüssel, die auf ein Feld verweisen.

```http
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Wählen Sie für alle Objekte außer Leads `{field to query}` aus `searchableFields` in der entsprechenden Antwort „Beschreiben“ aus. Geben Sie eine kommagetrennte Liste mit bis zu 300 Werten an.

Sie können auch die folgenden optionalen Abfrageparameter einbeziehen:

- `batchSize`: Eine Ganzzahl, die die Anzahl der zurückzugebenden Ergebnisse angibt. Der Standard- und Höchstwert ist 300.
- `nextPageToken`: Ein Token, das von einem vorherigen Paging-Aufruf zurückgegeben wurde. Weitere Informationen finden [ unter ](paging-tokens.md) von Paging-Token .
- `fields`: Eine kommagetrennte Liste von Feldnamen, die für jeden Datensatz zurückgegeben werden sollen. Gültige Felder finden Sie in der entsprechenden Beschreibung. Wenn Sie ein Feld anfordern, das nicht zurückgegeben wird, ist sein Wert impliziert null.
- `_method`: Sendet Abfragen mithilfe der POST-HTTP-Methode. Weitere Informationen zur Verwendung finden Sie im Abschnitt _method=GET .

Im folgenden Beispiel werden Opportunities abgefragt:

```http
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

Der `filterType` in diesem Aufruf ist „idField“, nicht „marketoGUID“. Sowohl „idField“ als auch „dedupeFields“ sind Sonderfälle, in denen Sie einen Alias für das/die entsprechende(n) Feld(er) verwenden können. Obwohl der Aufruf nicht explizit „marketoGUID“ festlegt, bleibt er das Suchfeld.

Die Felder oder Feldsätze, die durch `idField` und `dedupeFields` in einer Objektbeschreibung identifiziert werden, sind immer gültige `filterTypes` für eine Abfrage. Dieser Aufruf gibt Datensätze zurück, die den GUIDs in filterValues entsprechen. Wenn keine Datensätze übereinstimmen, zeigt die Antwort Erfolg an und gibt ein leeres Ergebnis-Array zurück.

Wenn der übereinstimmende Datensatzsatz 300 oder den angegebenen `batchSize` überschreitet (je nachdem, welcher Wert kleiner ist), enthält die Antwort `moreResult` mit dem Wert „true“ und einem `nextPageToken`. Schließen Sie das Token in einen nachfolgenden Aufruf ein, um weitere Datensätze abzurufen. Weitere Informationen finden [ unter ](paging-tokens.md) von Paging-Token .

### Lange URIs

Ein URI kann das Limit von 8 KB des REST-Services überschreiten, z. B. wenn Sie eine Abfrage nach GUIDs durchführen. Verwenden Sie in diesem Fall die HTTP-POST-Methode anstelle von GET und fügen Sie den `_method=GET` Abfrageparameter hinzu.

Übergeben Sie die verbleibenden Abfrageparameter im POST-Hauptteil als Zeichenfolge „application/x-www-form-urlencoded“. Übergeben Sie auch die zugehörige Kopfzeile für den Inhaltstyp .

```http
POST /rest/v1/opportunities.json?_method=GET
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Der `_method=GET`-Parameter wird auch bei der Abfrage von zusammengesetzten Schlüsseln benötigt.

### Zusammengesetzte Schlüssel

Um einen zusammengesetzten Schlüssel abzufragen, senden Sie eine POST-Anfrage mit einem JSON-Hauptteil. Verwenden Sie dieses Muster nur, wenn die `filterType` eine `dedupeFields` Option mit mehreren Feldern ist.

Zusammengesetzte Schlüssel werden derzeit nur von Opportunity-Rollen und einigen benutzerdefinierten Objekten verwendet. Im folgenden Beispiel werden Opportunity-Rollen mit dem zusammengesetzten Schlüssel aus `dedupeFields` abgefragt:

```http
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

Das JSON-Objekt akzeptiert alle Abfrageparameter, die für einfache Schlüsselabfragen mit Ausnahme von `filterValues` verwendet werden. Geben Sie anstelle von `filterValues` ein „Eingabe“-Array von JSON-Objekten an. Jedes Objekt muss jedes Feld im zusammengesetzten Schlüssel enthalten. In diesem Beispiel sind die Felder `externalOpportunityId`, `leadId` und `role`.

Die Anfrage fragt nach den bereitgestellten Eingaben `roles` und gibt übereinstimmende Ergebnisse zurück. Wenn die Antwort `moreResult=true` und einen `nextPageToken` enthält, schließen Sie alle ursprünglichen Eingaben und den `nextPageToken` in die nächste Anfrage ein.

## Erstellen und aktualisieren

Erstellen und aktualisieren Sie Lead-Datenbank-Datensätze, indem Sie POST-Anfragen mit JSON-Bodys senden. Opportunities, Rollen, benutzerdefinierte Objekte, Unternehmen und Vertriebspersonen verwenden dieselbe Benutzeroberfläche. Leads verwenden eine andere Benutzeroberfläche, die in der Lead-Dokumentation beschrieben wird.

Der einzige erforderliche Parameter ist `input`, ein Array von bis zu 300 Objekten. Jedes Objekt enthält die einzufügenden oder zu aktualisierenden Felder.

Sie können auch die folgenden optionalen Parameter einbeziehen:

- `action`: Akzeptiert `createOnly`, `updateOnly` oder `createOrUpdate`. Wenn dies ausgelassen wird, ist der Modus standardmäßig auf `createOrUpdate` eingestellt.
- `dedupeBy`: Akzeptiert `idField` oder `dedupeFields`, wenn für die Aktion „createOnly“ oder &quot;`createOrUpdate`&quot; festgelegt ist. Wenn dies ausgelassen wird, ist der Modus standardmäßig auf `dedupeFields` eingestellt.

Wenn `dedupeBy` `idField` ist, wird die in der Beschreibung aufgeführte `idField` für die Deduplizierung verwendet und muss in jedem Datensatz enthalten sein. `idField` Modus ist nicht mit dem `createOnly` Modus kompatibel.

Wenn `dedupeBy` `dedupeFields` ist, schließen Sie jedes `dedupeFields` Feld, das in der Objektbeschreibung aufgeführt ist, in jeden Datensatz ein.

Wenn Sie Feldwerte übergeben, schreibt die Datenbank den Wert `null` oder eine leere Zeichenfolge als `null`.

```http
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

Mit Ausnahme der Leads-API geben die Aufrufe zum Erstellen und Aktualisieren ein `seq` Feld in jedem Objekt im `result`-Array zurück. Die Zahl entspricht der Position des aktualisierten Datensatzes in der Anfrage.

Jedes Ergebnis gibt auch den `idField` des Objekttyps und die `status` „erstellt“, „aktualisiert“ oder „übersprungen“ zurück. Wenn der Status übersprungen wird, enthält das Ergebnis ein Array „Gründe“. Jedes Reason-Objekt enthält einen Code und eine Meldung, die erklären, warum der Datensatz übersprungen wurde. Siehe [Fehlercodes](error-codes.md) für weitere Informationen.

### Löschen

Mit Ausnahme von Leads verwenden Lead-Datenbankobjekte eine standardmäßige Löschschnittstelle. Neben der Eingabe ist der einzige erforderliche Parameter `deleteBy,` , der idField oder dedupeFields akzeptiert.

Im folgenden Beispiel werden benutzerdefinierte Objekte gelöscht:

```http
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

Die Antwort umfasst `seq`, `status` und `marketoGUID`. Bei übersprungenen Datensätzen enthält sie auch `reasons`.

Weitere Informationen zu CRUD-Vorgängen für einen bestimmten Objekttyp finden Sie in der Dokumentation für dieses Objekt.
