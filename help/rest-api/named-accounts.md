---
title: Benannte Konten
feature: REST API
description: Marketo-REST-Handbuch für CRUD zu spezifischen Konten für ABM mit Beschreibung, Abfrage, Aktualisierungsbeispielen, durchsuchbaren Feldern, Deduplizierungsregeln und keiner Lead-Verknüpfung.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# Benannte Konten

[Endpunkt-Referenz für benannte Konten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo bietet eine Reihe von APIs zur Durchführung von CRUD-Vorgängen für benannte Konten zur Verwendung mit Marketo ABM. Diese APIs folgen dem Standard-Schnittstellenmuster für Lead-Datenbank-APIs und bieten Optionen zum Beschreiben, Erstellen/Aktualisieren, Löschen und Abfragen.

Derzeit sind die einzigen ABM-bezogenen Funktionen, die über die APIs von Marketo verfügbar sind, die CRUD-Vorgänge für benannte Konten. Leads können nicht über APIs mit benannten Konten verknüpft werden.

## beschreiben

Bei der Beschreibung benannter Konten werden Metadaten zurückgegeben, die sich auf die Verwendung benannter Konten über die APIs von Marketo beziehen, einschließlich einer Liste der gültigen durchsuchbaren Felder bei der Abfrage und einer Liste aller für die API-Nutzung verfügbaren Felder. Die `idField` eines benannten Kontos ist immer `marketoGUID`, und die einzige verfügbare `dedupeField` und der Schlüssel für die Erstellung ist das `name` Feld des -Objekts.

```
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

Die Abfrage nach benannten Konten basiert auf der Verwendung eines filterType und einem Satz von bis zu 300 kommagetrennten filterValues. `filterType` kann ein beliebiges einzelnes Feld sein, das im `searchableFields` des Ergebnisses „Beschreiben“ für benannte Konten zurückgegeben wird, während „filterValues“ eine beliebige gültige Eingabe für den Datentyp des Felds sein kann. Um einen bestimmten Satz von Feldern aus zurückzugeben, muss ein -Feldparameter übergeben werden, wobei der Wert eine kommagetrennte Liste von Feldern ist, die in der Antwort zurückgegeben werden sollen. Wie bei anderen Abfrageoptionen beträgt die maximale Anzahl von Datensätzen für eine einzelne Abfrageseite 300, und zusätzliche Datensätze im Satz müssen mit der Verwendung des NextPageToken angefordert werden, das vom Aufruf zurückgegeben wird.

```
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

Das Erstellen und Aktualisieren benannter Konten folgt dem standardmäßigen Lead-Datenbankmuster. Datensätze müssen in einer POST-Anfrage im Eingabemitglied eines JSON-Textkörpers übergeben werden. `input` ist das einzige erforderliche Element, wobei `action` und `dedupeBy` optionale Elemente sind. Es können bis zu 300 Datensätze eingegeben werden. Aktion kann entweder createOnly, updateOnly oder createOrUpdate sein. Wenn nicht anders angegeben, wird für die Aktion standardmäßig „createOrUpdate“ festgelegt. dedupeBy darf nur angegeben werden, wenn actionOnly ist, und akzeptiert nur eines der dedupeFields oder idField, die den Feldern name bzw. marketoGUID entsprechen.

```
POST /rest/v1/namedaccounts.json
```

```
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

Das benannte Kontoobjekt enthält einen Satz von Feldern. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind Anzeigename, API-Name und Datentyp. Diese Attribute werden zusammen als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder im Unternehmensobjekt abfragen. Diese APIs erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der Berechtigungen „Schema-Standardfeld lesen/schreiben“ oder „Schema-benutzerdefiniertes Feld lesen/schreiben“ verfügt.

### Abfragefelder

Die Abfrage benannter Kontofelder ist einfach. Sie können ein einzelnes benanntes Kontofeld nach API-Namen abfragen oder den Satz aller Unternehmensfelder abfragen.

#### Nach Name

Der Endpunkt [Benanntes Kontofeld nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im benannten Kontoobjekt ab. Der erforderliche fieldApiName-Pfadparameter gibt den API-Namen des Felds an. Die Antwort ähnelt dem benannten Konto-Endpunkt Describe , enthält jedoch zusätzliche Metadaten wie das Attribut isCustom, das angibt, ob das Feld ein benutzerdefiniertes Feld ist.

```
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

Der Endpunkt [Benannte Kontofelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) ruft Metadaten für alle Felder des benannten Kontoobjekts ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den Abfrageparameter „batchSize“ verwenden, um diese Anzahl zu reduzieren. Wenn das Attribut moreResult wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind. Das von dieser API zurückgegebene nextPageToken sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

```
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

Löschungen werden über eine JSON-POST-Anfrage durchgeführt und weisen ein erforderliches Eingabemittel und ein optionales deleteBy-Element auf. deleteBy kann eines der folgenden sein: „dedupeFields“ oder „idField“, die dem Namen bzw. der marketoGUID entsprechen, und wird standardmäßig auf „dedupeFields“ festgelegt, wenn nicht festgelegt. Das Eingabeelement akzeptiert ein Array von bis zu 300 Datensätzen, die je ein Element enthalten, entweder name oder marketoGUID, je nach der Einstellung von deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
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

- Die maximale Wartezeit für benannte Konto-Endpunkte beträgt 30 Sekunden, es sei denn, dies wird weiter unten angegeben
   - Spezifische Konten synchronisieren: 120s
   - Spezifische Konten löschen: 60er
