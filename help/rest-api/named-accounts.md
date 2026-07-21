---
title: Benannte Konten
feature: REST API
description: Marketo-REST-Handbuch für CRUD zu spezifischen Konten für ABM mit Beschreibung, Abfrage, Aktualisierungsbeispielen, durchsuchbaren Feldern, Deduplizierungsregeln und keiner Lead-Verknüpfung.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
TQID: https://experienceleague.adobe.com/iY3UYVelm3aKuuDBCTxaVCbkXfwnJzDjV3Kvn9rcNbA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 590
ht-degree: 1%

---

# Benannte Konten

[Endpunkt-Referenz für benannte Konten](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts)

Marketo stellt APIs zum Ausführen von CRUD-Vorgängen für benannte Konten zur Verwendung mit Marketo ABM bereit. Diese APIs folgen dem Standard-Schnittstellenmuster für Lead-Datenbanken und bieten Optionen zum Beschreiben, Erstellen/Aktualisieren, Löschen und Abfragen.

Derzeit unterstützen Marketo-APIs nur CRUD-Vorgänge für benannte Konten. Sie können Leads nicht über die APIs mit benannten Konten verknüpfen.

## beschreiben

Beschreiben von benannten Konten gibt Metadaten für die Verwendung benannter Konten über Marketo-APIs zurück. Die Antwort enthält gültige durchsuchbare Felder und alle für die API verfügbaren Felder.

Die `idField` eines benannten Kontos ist immer `marketoGUID`. Das `name` des -Objekts ist der einzige verfügbare `dedupeField` und Erstellungsschlüssel.

```http
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
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
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Abfrage

Abfragen von benannten Konten mithilfe eines „filterType“ und bis zu 300 kommagetrennten „filterValues“. Der „filterType“ kann jedes einzelne Feld sein, das im `searchableFields` der „Describe“-Antwort zurückgegeben wird. Jeder filterValues-Eintrag muss ein gültiger Wert für den Datentyp des Felds sein.

Um bestimmte Felder zurückzugeben, übergeben Sie einen -Feldparameter mit einer kommagetrennten Liste von Feldern. Eine Abfrageseite enthält maximal 300 Datensätze. Um zusätzliche Datensätze abzurufen, verwenden Sie das NextPageToken, das vom Aufruf zurückgegeben wird.

```http
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Erstellen und aktualisieren

Benannte Konten mithilfe des standardmäßigen Lead-Datenbankmusters erstellen und aktualisieren. Übergeben Sie Datensätze im Eingabemitglied des JSON-Bodys einer POST-Anfrage. Sie können bis zu 300 Datensätze einbeziehen.

Die Mitglieder der Anfrage sind:

- `input`: Das einzige erforderliche Mitglied.
- `action`: Ein optionaler Member, der createOnly, updateOnly oder createOrUpdate akzeptiert. Der Standardwert ist createOrUpdate.
- `dedupeBy`: Ein optionales Element, das nur verfügbar ist, wenn die Aktion „updateOnly“ ist. Sie akzeptiert deduplizierte Felder bzw. idField, die den Feldern name bzw. marketoGUID entsprechen.

```http
POST /rest/v1/namedaccounts.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Felder

Das benannte Kontoobjekt enthält Felder, die durch Attribute wie Anzeigename, API-Name und Datentyp definiert sind. Zusammen werden diese Attribute als Metadaten bezeichnet.

Die folgenden Endpunkte geben Abfragefelder für das Unternehmensobjekt an. Der API-Benutzer muss über eine Rolle mit der Berechtigung Standardfeld für Lese- und Schreibschemafelder, der Berechtigung Benutzerdefiniertes Feld für Lese- und Schreibschemafelder oder beidem verfügen.

### Abfragefelder

Fragen Sie ein benanntes Kontofeld nach API-Namen ab oder rufen Sie alle Unternehmensfelder ab.

#### Nach Name

Der Endpunkt [Benanntes Kontofeld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) ruft Metadaten für ein Feld des benannten Kontoobjekts ab. Der erforderliche Pfadparameter fieldApiName gibt den API-Namen des Felds an.

Die Antwort ähnelt der Antwort von Describe Named Account, enthält jedoch zusätzliche Metadaten. Beispielsweise gibt das Attribut isCustom an, ob es sich um ein benutzerdefiniertes Feld handelt.

```http
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Durchsuchen

Der Endpunkt [Benannte Kontofelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) ruft Metadaten für alle Felder des benannten Kontoobjekts ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Verwenden Sie den Abfrageparameter „batchSize“, um diese Anzahl zu reduzieren.

Wenn das Attribut moreResult den Wert true hat, sind weitere Ergebnisse verfügbar. Rufen Sie den Endpunkt mit dem zurückgegebenen nextPageToken weiter auf, bis moreResult false ist.

```http
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Domain Name",
            "name": "domainName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Industry",
            "name": "industry",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Löschen

Löschen Sie benannte Konten, indem Sie eine POST-Anfrage mit einem JSON-Hauptteil senden. Die Anfrage enthält ein erforderliches Eingabeelement und ein optionales deleteBy-Element.

Der deleteBy-Member akzeptiert „dedupeFields“ oder „idField“, die dem Namen bzw. der marketoGUID entsprechen. Wenn nicht festgelegt, werden standardmäßig deduplizierte Felder verwendet. Das Eingabeglied akzeptiert bis zu 300 Datensätze. Jeder Datensatz enthält entweder Name oder marketoGUID, je nach der Einstellung von deleteBy.

```http
POST /rest/v1/namedaccounts/delete.json
```

```text
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
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
         "status":"deleted"
      },
      {
         "seq":1,
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Zeitüberschreitungen

- Die maximale Wartezeit für benannte Kontoendpunkte beträgt 30 Sekunden, sofern nicht anders angegeben.
- Die maximale Wartezeit für die Synchronisierung benannter Konten beträgt 120 Sekunden.
- Die maximale Wartezeit für das Löschen benannter Konten beträgt 60 Sekunden.
