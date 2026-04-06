---
title: Datenaufnahme
feature: REST API, Dynamic Content
description: Verwenden Sie die Marketo-Datenaufnahme-API für die Aufnahme von Personen, benutzerdefinierten Objekten, Unternehmen und Programmmitgliedern mit hohem Volumen und geringer Latenz.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 6dc068f92d5b0c94035ca484fd1508dfe87bbd76
workflow-type: tm+mt
source-wordcount: '1789'
ht-degree: 17%

---

# Datenaufnahme-API

Die Datenerfassungs-API ist ein Service mit hoher Datenmenge und geringer Latenz, der mit hoher Verfügbarkeit entwickelt wurde, um die Aufnahme großer Mengen von Personen- und personenbezogenen Daten effizient und mit minimalen Verzögerungen zu handhaben.

Daten werden durch Senden von Anfragen aufgenommen, die asynchron ausgeführt werden. Der Anfragestatus kann abgerufen werden, indem Ereignisse aus dem [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup) abonniert werden.

Schnittstellen werden für vier Objekttypen angeboten: Personen, benutzerdefinierte Objekte, Unternehmen und Programmmitglieder. Der Datensatzvorgang ist nur „Einfügen oder Aktualisieren“, mit Ausnahme von Programmmitgliedern, die auch das Löschen unterstützen.

>[!NOTE]
>
>Für den Zugriff auf die Datenaufnahme-API ist eine Berechtigung für das Paket [Marketo Engage-](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835) erforderlich.

## Authentifizierung

Die Datenaufnahme-API verwendet dieselbe OAuth 2.0-Authentifizierungsmethode wie die Marketo REST-API zum Generieren eines Zugriffstokens, das Zugriffstoken muss jedoch über die HTTP-Kopfzeile `X-Mkto-User-Token` übergeben werden. Das Zugriffstoken kann nicht über einen Abfrageparameter übergeben werden.

Beispiel eines Zugriffs-Tokens über den Header:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Berechtigungen

Die Datenaufnahme verwendet dasselbe Berechtigungsmodell wie die Marketo REST-API und erfordert keine zusätzlichen speziellen Berechtigungen zur Verwendung, obwohl für jeden Endpunkt spezifische Berechtigungen erforderlich sind.

| Endpunkt | Berechtigung |
| --- | --- |
| Personen | Lead mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte | Objekt mit Lese-/Schreibzugriff |
| Firmen | Unternehmen mit Lese-/Schreibzugriff |
| Programm-Mitglieder | Lead mit Lese-/Schreibzugriff |

## Unterstützte Objekttypen

| Objekttyp | Unterstützte Vorgänge |
| --- | --- |
| Personen | Upsert (einfügen oder aktualisieren) |
| Benutzerdefinierte Objekte | Upsert (einfügen oder aktualisieren) |
| Firmen | Synchronisieren (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Programm-Mitglieder | Synchronisieren (Upsert-Status), Löschen (aus Programm entfernen) |

## Kopfzeilen

Die Datenaufnahme nutzt die folgenden benutzerdefinierten HTTP-Kopfzeilen.

### Anfrage

| Schlüssel | Wert | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Beliebige Zeichenfolge (maximale Länge 255 Zeichen). | Nein | Kann verwendet werden, um Anfragen über das System nachzuverfolgen. Siehe Marketo Observability Data Stream |
| `X-Request-Source` | Beliebige Zeichenfolge (maximale Länge 50 Zeichen). | Nein | Kann verwendet werden, um die Anfragequelle im System zu verfolgen. Siehe Marketo Observability Data Stream |

### Antwort

| Schlüssel | Wert | Erforderlich |
| --- | --- | --- |
| `X-Request-Id` | Eindeutige Anfrage-ID. | Ja |

## Anfragen

Verwenden Sie die HTTP-POST-Methode, um Daten an den Server zu senden.

Die Datendarstellung ist im Anfragetext als application/json enthalten.

Der Domain-Name lautet: `mkto-ingestion-api.adobe.io`

Der Pfad beginnt mit `/subscriptions/MunchkinId`, wobei MunchkinId spezifisch für Ihre Marketo-Instanz ist. Ihre Munchkin-ID finden Sie in der Marketo Engage-Benutzeroberfläche unter **Admin** > **Mein** > **Support-Informationen**.  Der Rest des Pfads wird verwendet, um die gewünschte Ressource anzugeben.

Beispiel-URL für Personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Beispiel-URL für benutzerdefinierte Objekte:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

Beispiel-URL für Unternehmen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Beispiel-URL für Programmmitglieder:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

### Antworten

Alle Antworten geben über den `X-Request-Id`-Header eine eindeutige Anfrage-ID zurück.

Beispiel einer Anfrage-ID über den Header:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Erfolgreich

Wenn ein Aufruf erfolgreich war, wird der Status 202 zurückgegeben.  Es wird kein Antworttext zurückgegeben.

Beispiel für eine erfolgreiche Antwort:

```
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Fehler

Wenn ein Aufruf einen Fehler erzeugt, wird ein Nicht-202-Status zusammen mit einem Antworttext mit zusätzlichen Fehlerdetails zurückgegeben. Der Antworttext ist `application/json` und enthält ein einzelnes -Objekt mit `error_code` und `message`.

Im Folgenden finden Sie wiederverwendete Fehler-Codes vom Adobe Developer-Gateway.

| HTTP-Status-Code | error_code | Nachricht |
| --- | --- | --- |
| 401 | 401013 | OAuth-Token ist ungültig |
| 403 | 403010 | OAuth-Token fehlt |
| 404 | 404040 | Ressource nicht gefunden |
| 429 | 429001 | Service-Nutzungsbeschränkung erreicht |

Im Folgenden finden Sie Fehlercodes, die für die Datenaufnahme-API eindeutig sind und aus drei Segmenten bestehen.  Die ersten drei Ziffern geben den Status an (von Adobe Developer Gateway zurückgegeben), gefolgt von einer Null „0“, gefolgt von drei Ziffern.

| HTTP-Status-Code | error_code | Nachricht |
| --- | --- | --- |
| 400 | 4000801 | Ungültige Anfrage |
| 400 | 4000802 | Ungültige Daten |
| 403 | 4030801 | Nicht autorisiert |
| 429 | 4290801 | Tägliches Kontingent erreicht |
| 500 | 5000801 | Interner Server-Fehler |

## Weitere Zustellversuche

Wenn ein vorübergehender Fehler erkannt wird, versucht der Service den Vorgang erneut. Weitere Zustellversuche erfolgen aus verschiedenen Gründen, in erster Linie, wenn ein abhängiger Service eine Zeitüberschreitung aufweist oder vorübergehend nicht verfügbar ist.

Wiederholungsintervalle:

* Anfänglicher Vorgang und erster erneuter Versuch : 5 Minuten
* &#x200B;1. und 2. : 15 Min
* &#x200B;2. und 3. : 20 Min
* &#x200B;3. und 4. : 20 Min
* &#x200B;4. und 5. : 2 Stunden
* Nach 5. Wiederholung -> 3 Stunden

## Endpunkte

Aufnahme-Endpunkte sind für Personen, benutzerdefinierte Objekte, Unternehmen und Programmmitglieder verfügbar.

### Personen

Endpunkt, der zur Aktualisierung von Personendatensätzen verwendet wird.

| Methode | Pfad |
| --- | --- |
| POST | /subscriptions/{munchkinId}/persons |

#### Kopfzeilen

| Schlüssel | Wert |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| --- | --- | --- | --- | --- |
| `priority` | Zeichenfolge | Nein | Priorität der Anfrage: normal oder hoch | Normal |
| `partitionName` | Zeichenfolge | Nein | Name der Personenpartition | Standard |
| `dedupeFields` | Objekt | Nein | Zu deduplizierende Attribute. Ein oder zwei Attributnamen sind zulässig. <br/> In einem AND-Vorgang werden zwei Attribute verwendet. Wenn beispielsweise sowohl `email` als auch `firstName` angegeben sind, werden sie beide verwendet, um eine Person mithilfe des AND-Vorgangs nachzuschlagen. <br/>Unterstützte Attribute sind: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, benutzerdefinierte Attribute (nur vom Typ „string“ und „integer„) `email` |  |
| `persons` | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut der Person | – |

Erforderliche Berechtigungen sind `Read-Write Lead`.

### Beispiel für Personen

#### Anfrage

`POST /subscriptions/{munchkinId}/persons`

#### Kopfzeilen

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Textkörper

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Benutzerdefinierte Objekte

Endpunkt zum Upsertieren benutzerdefinierter Objektdatensätze.

| Methode | Pfad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Kopfzeilen

| Schlüssel | Wert |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| --- | --- | --- | --- | --- |
| `priority` | Zeichenfolge | Nein | Priorität der Anfrage: normal, hoch | Normal |
| `dedupeBy` | Zeichenfolge | Nein | Zu deduplizierende Attribute für: dedupeFields, marketoGUID | deduplizierte Felder |
| `customObjects` | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut des Objekts. | – |

Erforderliche Berechtigungen sind `Read-Write Custom Object`.

Wenn in der Anfrage ein Verknüpfungsfeld zu einer Person angegeben ist und diese Person nicht vorhanden ist, werden mehrere weitere Zustellversuche unternommen. Wenn diese Person im Wiederholungsfenster (65 Minuten) hinzugefügt wird, ist die Aktualisierung erfolgreich. Wenn beispielsweise das Verknüpfungsfeld auf Person `email` ist und Person nicht vorhanden ist, werden weitere Zustellversuche unternommen.

### Beispiel für benutzerdefinierte Objekte

#### Anfrage

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`

#### Kopfzeilen

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Textkörper

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Firmen

Endpunkt zum Synchronisieren von Firmendatensätzen. Unterstützt das Erstellen, Aktualisieren und Aktualisieren von Vorgängen mit Deduplizierung nach externer Unternehmens-ID oder interner Marketo-ID.

| Methode | Pfad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### Kopfzeilen

| Schlüssel | Wert | Erforderlich |
| --- | --- | --- |
| `Content-Type` | application/json | Ja |
| `X-Mkto-User-Token` | {accessToken} | Ja |
| `X-Correlation-Id` | Beliebige Zeichenfolge (maximale Länge 255 Zeichen) | Nein |
| `X-Request-Source` | Beliebige Zeichenfolge (maximale Länge 50 Zeichen) | Nein |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| --- | --- | --- | --- | --- |
| `action` | Zeichenfolge | Nein | Synchronisierungsaktion: `createOnly`, `updateOnly` oder `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | Zeichenfolge | Nein | Feld für Deduplizierung: `dedupeFields` oder `idField` (ohne Unterscheidung zwischen Groß- und Kleinschreibung). Für `createOnly` und `createOrUpdate` ist nur `dedupeFields` zulässig. Beides ist `updateOnly` zulässig. | `dedupeFields` |
| `input` | Array von Objekten | Ja | Liste der Name-Wert-Paare für Firmenattribute. Akzeptiert JSON-`input` oder -`companies`. | – |

Jedes Unternehmensobjekt im `input`-Array unterstützt die folgenden Felder:

| Schlüssel | Datentyp | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| `externalCompanyId` | Zeichenfolge | bedingt | Externe Unternehmenskennung. Erforderlich, wenn `dedupeBy` `dedupeFields` ist. Nicht zulässig, wenn `dedupeBy` `idField` ist. |
| `id` | Lang | bedingt | Interne Marketo-Unternehmens-ID. Erforderlich, wenn `dedupeBy` `idField` und `action` `updateOnly` ist. Nicht zulässig, wenn `dedupeBy` `dedupeFields` ist. |
| `company` | Zeichenfolge | Nein | Firmenname. |
| (beliebiges Feld) | Beliebig | Nein | Zusätzliche standardmäßige oder benutzerdefinierte Unternehmensfelder gemäß Definition in [Firmen beschreiben](companies.md). Bei Feldnamen wird nicht zwischen Groß- und Kleinschreibung unterschieden. |

Erforderliche Berechtigungen sind `Read-Write Company`.

### Beispiel für Unternehmen

#### Anfrage

`POST /subscriptions/{munchkinId}/companies`

#### Kopfzeilen

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Textkörper

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Beispiel für eine Aktualisierung nach ID durch Unternehmen

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Validierungsregeln für Unternehmen

| Regel | Detail |
| --- | --- |
| Aktion | Muss eines der folgenden sein: `createOnly`, `updateOnly`, `createOrUpdate`. Von Schreibweise abhängig. |
| deduplizieren nach | Muss `dedupeFields` oder `idField` sein (ohne Unterscheidung zwischen Groß- und Kleinschreibung). Die Standardeinstellung ist `dedupeFields`. |
| deduplizierenNach + Aktion | `createOnly` und `createOrUpdate` nur `dedupeFields`. `updateOnly` erlaubt sowohl `dedupeFields` als auch `idField`. |
| Wenn `dedupeBy=dedupeFields` | Jedes Unternehmen muss über `externalCompanyId` verfügen. Feld `id` darf nicht vorhanden sein. |
| Wenn `dedupeBy=idField` | Jedes Unternehmen muss über `id` verfügen. Feld `externalCompanyId` darf nicht vorhanden sein. |
| `input` / `companies` | Darf nicht null oder leer sein. |
| Max. Objekte pro Anfrage | 1,000 |

### Programmmitglieder (synchronisieren)

Endpunkt, der zum Synchronisieren des Programmmitgliedsstatus, Hinzufügen von Leads zu Programmen oder Aktualisieren ihres Programmstatus verwendet wird.

| Methode | Pfad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### Kopfzeilen

| Schlüssel | Wert | Erforderlich |
| --- | --- | --- |
| Inhaltstyp | application/json | Ja |
| x-moto-user-token | {accessToken} | Ja |
| x-correlation-id | Beliebige Zeichenfolge (maximale Länge 255 Zeichen) | Nein |
| x-request-Source | Beliebige Zeichenfolge (maximale Länge 50 Zeichen) | Nein |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| --- | --- | --- | --- | --- |
| Programme | Array von Objekten | Ja | Liste der Programmvorgänge. Jeder gibt ein Programm, einen Zielstatus und die Leads zur Synchronisierung an. | – |

Jedes Objekt im `programs`-Array enthält:

| Schlüssel | Datentyp | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| programId | Lang | Ja | Die Marketo-Programm-ID. Muss eine positive Ganzzahl sein. |
| status | Zeichenfolge | Ja | Der festzulegende Status des Programmmitglieds, z. B. `"Member"` oder `"Influenced"`. Akzeptiert JSON-`statusName` oder -`status`. Der Wert darf nicht `"Not in Program"` sein. Verwenden Sie stattdessen den Endpunkt „delete“. |
| Mitglieder | Array von Objekten | Ja | Liste der Lead-Verweise, die dem Programm hinzugefügt oder aktualisiert werden sollen. Akzeptiert JSON-`input` oder -`members`. |

Jedes Objekt im `members`-Array enthält:

| Schlüssel | Datentyp | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| leadId | Lang | Ja | Die Marketo Lead-ID. |
| (beliebiges Feld) | Beliebig | Nein | Zusätzliche benutzerdefinierte Programmteilnehmerfelder. Bei Feldnamen wird nicht zwischen Groß- und Kleinschreibung unterschieden. |

Erforderliche Berechtigungen sind `Read-Write Lead`.

### Beispiel für die Synchronisierung von Programmmitgliedern

#### Anfrage

`POST /subscriptions/{munchkinId}/programmembers`

#### Kopfzeilen

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Textkörper

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Programmmitglieder synchronisieren Validierungsregeln

| Regel | Detail |
| --- | --- |
| Programme | Darf nicht null oder leer sein. |
| programId | Erforderlich. Muss eine positive Ganzzahl sein. |
| status | Erforderlich. Darf nicht leer sein. Darf nicht `"Not in Program"` sein (ignoriert Groß- und Kleinschreibung). Verwenden Sie stattdessen den Endpunkt „delete“. |
| Mitglieder | Darf nicht null oder leer sein. |
| leadId | Erforderlich für jedes Element im Eingabe-Array. |
| Max. Leads pro Anfrage | Insgesamt 1.000 Mitglieder in allen Programmen. |

### Programmmitglieder (Löschen)

Endpunkt zum Entfernen von Leads aus Programmen. Dadurch wird der Mitgliedschaftsstatus des Leads auf `"Not in Program"` gesetzt und das Mitglied aus diesem Programm entfernt.

>[!NOTE]
>
>Dieser Endpunkt verwendet POST anstelle von DELETE, da die Anfrage einen JSON-Hauptteil mit strukturierten Daten erfordert.

| Methode | Pfad |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### Kopfzeilen

| Schlüssel | Wert | Erforderlich |
| --- | --- | --- |
| Inhaltstyp | application/json | Ja |
| x-moto-user-token | {accessToken} | Ja |
| x-correlation-id | Beliebige Zeichenfolge (maximale Länge 255 Zeichen) | Nein |
| x-request-Source | Beliebige Zeichenfolge (maximale Länge 50 Zeichen) | Nein |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| --- | --- | --- | --- | --- |
| Programme | Array von Objekten | Ja | Liste der Vorgänge zum Löschen von Programmen. Jeder gibt ein Programm und die zu entfernenden Leads an. | – |

Jedes Objekt im `programs`-Array enthält:

| Schlüssel | Datentyp | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| programId | Lang | Ja | Die Marketo-Programm-ID. Muss eine positive Ganzzahl sein. |
| Mitglieder | Array von Objekten | Ja | Liste der Lead-Referenzen, die aus dem Programm entfernt werden sollen. Akzeptiert JSON-`input` oder -`members`. |

Jedes Objekt im `members`-Array enthält:

| Schlüssel | Datentyp | Erforderlich | Beschreibung |
| --- | --- | --- | --- |
| leadId | Lang | Ja | Die Marketo Lead-ID. |

Erforderliche Berechtigungen sind `Read-Write Lead`.

### Beispiel zum Löschen von Programmmitgliedern

#### Anfrage

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### Kopfzeilen

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Textkörper

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### Programmmitglieder löschen Validierungsregeln

| Regel | Detail |
| --- | --- |
| Programme | Darf nicht null oder leer sein. |
| programId | Erforderlich. Muss eine positive Ganzzahl sein. |
| Mitglieder | Darf nicht null oder leer sein. |
| leadId | Erforderlich für jedes Element im Eingabe-Array. |
| Max. Leads pro Anfrage | Insgesamt 1.000 Mitglieder in allen Programmen. |

## Beschränkungen

Im Folgenden finden Sie eine aktualisierte Liste von Leitplanken:

* Maximale Größe der Anfrage: 1 MB
* Maximale Anzahl von Objekten pro Anfrage pro Objekttyp: 1.000
* Maximale Anzahl von Anfragen pro Sekunde und Client-ID: 5.000
* Maximale Anzahl an Objekten pro Tag: 10.000.000

Diese Beschränkungen gelten einheitlich für Personen, benutzerdefinierte Objekte, Unternehmen und Programmmitglieder. Für Programmteilnehmer ist „Objekte pro Anfrage“ die Gesamtzahl der Lead-Referenzen in allen Programmen in einer einzigen Anfrage.

## Datenaufnahme-API und REST-API im Vergleich

Im Folgenden finden Sie eine Liste der Unterschiede zwischen der Datenaufnahme-API und anderen Marketo-REST-APIs:

* Zur Authentifizierung müssen Sie das Zugriffstoken mithilfe der `X-Mkto-User-Token`-Kopfzeile übergeben
* Der URL-Domain-Name lautet `mkto-ingestion-api.adobe.io`
* Der URL-Pfad beginnt mit `/subscriptions/MunchkinId`
* Es gibt keine Abfrageparameter
* Wenn der Aufruf erfolgreich ist, wird der Status 202 zurückgegeben und der Antworttext ist leer
* Wenn ein Aufruf fehlschlägt, wird ein Nicht-202-Status zurückgegeben und der Antworttext enthält `{ "error_code" : "Error Code", "message" : "Message" }`
* Die Anfrage-ID wird über `X-Request-Id` -Header zurückgegeben
