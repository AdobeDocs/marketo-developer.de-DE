---
title: "Spezifische Konten"
feature: REST API
description: "Bearbeiten Sie benannte Konten über die API."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 0%

---


# Genannte Konten

[Endpunktverweis zu benannten Konten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo bietet eine Reihe von APIs zum Ausführen von CRUD-Vorgängen für benannte Konten zur Verwendung mit Marketo ABM. Diese APIs entsprechen dem Standardmuster der Benutzeroberfläche für Lead-Datenbank-APIs und bieten Optionen zum Beschreiben, Erstellen/Aktualisieren, Löschen und Abfragen.

Derzeit sind die einzigen ABM-bezogenen Funktionen, die über die APIs von Marketo verfügbar sind, CRUD-Vorgänge für benannte Konten. Leads können nicht über APIs mit benannten Konten verknüpft werden.

## Beschreibung

Die Beschreibung benannter Konten gibt Metadaten zurück, die mit der Verwendung benannter Konten über die Marketo-APIs zusammenhängen, einschließlich einer Liste gültiger durchsuchbarer Felder bei der Abfrage und einer Liste aller für die API-Nutzung verfügbaren Felder. Die `idField` von einem benannten Konto immer `marketoGUID`und der einzigen verfügbaren `dedupeField`, und der Schlüssel für die Erstellung ist die `name` -Feld des Objekts.

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

### Anfrage

Die Abfrage nach benannten Konten basiert auf der Verwendung eines filterType und einer Anzahl von bis zu 300 kommagetrennten filterValues. `filterType` kann jedes einzelne Feld sein, das in der `searchableFields` Mitglied des beschreibenden Ergebnisses für benannte Konten, während filterValues eine beliebige gültige Eingabe für den Datentyp des Felds sein kann. Um einen bestimmten Feldsatz zurückzugeben, muss ein Feldparameter übergeben werden, wobei der Wert eine kommagetrennte Liste von Feldern ist, die in der Antwort zurückgegeben werden sollen. Wie bei anderen Abfrageoptionen beträgt die maximale Datensatzanzahl für eine einzelne Abfrageseite 300, und zusätzliche Datensätze im Satz müssen mit der Verwendung des vom Aufruf zurückgegebenen nextPageToken angefordert werden.

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

### Erstellen und Aktualisieren

Das Erstellen und Aktualisieren benannter Konten folgt dem standardmäßigen Muster der Lead-Datenbank. Datensätze müssen in einer POST-Anfrage an das Eingabemitglied eines JSON-Hauptteils übergeben werden. `input` ist das einzige erforderliche Mitglied mit `action` und `dedupeBy` als optionale Mitglieder. Bis zu 300 Datensätze können in die Eingabe aufgenommen werden. Die Aktion kann zu createOnly, updateOnly oder createOrUpdate gehören. Wenn nicht angegeben, wird standardmäßig createOrUpdate verwendet. dedupeBy kann nur angegeben werden, wenn die Aktion updateOnly lautet, und akzeptiert nur eines von dedupeFields bzw. idField, die dem name- bzw. marketoGUID-Feld entsprechen.

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

Das benannte Kontoobjekt enthält eine Reihe von Feldern. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind der Anzeigename, der API-Name und der dataType. Diese Attribute werden kollektiv als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder für das Unternehmensobjekt abfragen. Für diese APIs muss der Eigentümer-API-Benutzer über eine oder beide der Berechtigungen &quot;Schema Standard-Feld lesen/schreiben&quot;oder &quot;Benutzerdefiniertes Feld lesen und schreiben&quot;verfügen.

### Abfragefelder

Die Abfrage von benannten Kontofeldern ist unkompliziert. Sie können ein einzelnes benanntes Kontofeld nach API-Name abfragen oder die Gruppe aller Unternehmensfelder abfragen.

#### Nach Name

Die [Feld für benanntes Konto nach Name abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) -Endpunkt ruft Metadaten für ein einzelnes Feld im benannten Kontoobjekt ab. Der erforderliche fieldApiName -Pfadparameter gibt den API-Namen des Felds an. Die Antwort entspricht dem Endpunkt &quot;Benanntes Konto beschreiben&quot;, enthält jedoch zusätzliche Metadaten wie das Attribut isCustom , das angibt, ob es sich bei dem Feld um ein benutzerdefiniertes Feld handelt.

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

Die [Spezifische Kontofelder abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) -Endpunkt ruft Metadaten für alle Felder im benannten Kontoobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den Abfrageparameter batchSize verwenden, um diese Zahl zu reduzieren. Wenn das Attribut moreResult &quot;true&quot;ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Das von dieser API zurückgegebene nextPageToken sollte für die nächste Iteration dieses Aufrufs immer wiederverwendet werden.

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

Löschungen werden über eine JSON-POST-Anfrage durchgeführt und verfügen über ein erforderliches Eingabemitglied und ein optionales deleteBy-Mitglied. deleteBy kann einer der Werte &quot;dedupeFields&quot;bzw. &quot;idField&quot;sein, die jeweils name bzw. marketoGUID entsprechen, und dedupeFields wird standardmäßig verwendet, wenn nicht festgelegt. Das Eingabeelement akzeptiert ein Array mit bis zu 300 Datensätzen, das je nach Einstellung von deleteBy jeweils ein Mitglied enthält, entweder name oder marketoGUID.

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

## Timeouts

- Für benannte Kontoendpunkte gilt eine Zeitüberschreitung von 30 Sekunden, es sei denn, dies ist unten angegeben.
   - Spezifische Konten synchronisieren: 120s 
   - Benannte Konten löschen: 60 s
