---
title: Datenaufnahme
description: Übersicht über die Datenaufnahme-API
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 13%

---


# Datenaufnahme

Die Datenaufnahme-API ist ein Service mit hoher Datenmenge und geringer Latenz, der hochverfügbar ist und der die Aufnahme großer Mengen von Personen- und personenbezogenen Daten effizient und mit minimalen Verzögerungen verarbeitet. 

Daten werden durch Senden von Anfragen aufgenommen, die asynchron ausgeführt werden. Der Anfragestatus kann abgerufen werden, indem Ereignisse aus dem [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/) abonniert werden&#x200B;

Schnittstellen werden für zwei Objekttypen angeboten: Personen, Benutzerdefinierte Objekte. Der Datensatzvorgang ist nur „Einfügen oder Aktualisieren“.

Die Datenaufnahme-API befindet sich in der privaten Beta-Phase. Eingeladene Benutzer müssen über eine Berechtigung für das [Marketo Engage-Leistungspaketpaket verfügen](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Authentifizierung

Die Datenaufnahme-API verwendet dieselbe OAuth 2.0-Authentifizierungsmethode wie die Marketo REST-API zum Generieren eines Zugriffstokens, das Zugriffstoken muss jedoch über die HTTP-Kopfzeile `X-Mkto-User-Token` übergeben werden. Das Zugriffstoken kann nicht über einen Abfrageparameter übergeben werden.

Beispiel eines Zugriffs-Tokens über den Header:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Berechtigungen

Die Datenaufnahme verwendet dasselbe Berechtigungsmodell wie die Marketo REST-API und erfordert keine zusätzlichen speziellen Berechtigungen zur Verwendung, obwohl für jeden Endpunkt spezifische Berechtigungen erforderlich sind.

| Endpunkt | Berechtigung |
|---|---|
| Personen | Lead mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte | Objekt mit Lese-/Schreibzugriff |

## Kopfzeilen

Die Datenaufnahme nutzt die folgenden benutzerdefinierten HTTP-Kopfzeilen.

### Anfrage

| Schlüssel | Wert | Erforderlich | Beschreibung |
|---|---|---|---|
| x-correlation-id | Beliebige Zeichenfolge (maximale Länge 255 Zeichen). | Nein | Kann verwendet werden, um Anfragen über das System nachzuverfolgen. Siehe Marketo Observability Data Stream |
| x-request-Source | Beliebige Zeichenfolge (maximale Länge 50 Zeichen). | Nein | Kann verwendet werden, um die Anfragequelle über das System nachzuverfolgen. Siehe Marketo Observability Data Stream |

### Antwort

| Schlüssel | Wert | Erforderlich | Beschreibung |
|---|---|---|---|
| x-request-id | Eindeutige Anfrage-ID. | Ja | |

## Anfragen

Verwenden Sie die HTTP-POST-Methode, um Daten an den Server zu senden.

Die Datendarstellung ist im Anfragetext als application/json enthalten.

Der Domain-Name lautet: `mkto-ingestion-api.adobe.io`

Der Pfad beginnt mit `/subscriptions/_MunchkinId_`, wobei `_MunchkinId_` spezifisch für Ihre Marketo-Instanz ist. Ihre Munchkin-ID finden Sie in der Marketo Engage-Benutzeroberfläche unter **Admin** > **Mein** > **Support-Informationen**. Der Rest des Pfads wird verwendet, um die gewünschte Ressource anzugeben.

Beispiel-URL für Personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Beispiel-URL für benutzerdefinierte Objekte:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Antworten

Alle Antworten geben über den `X-Request-Id`-Header eine eindeutige Anfrage-ID zurück.

Beispiel einer Anfrage-ID über den Header:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Erfolgreich

Wenn ein Aufruf erfolgreich war, wird der Status 202 zurückgegeben. Es wird kein Antworttext zurückgegeben.

Beispiel für eine erfolgreiche Antwort:

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Fehler

Wenn ein Aufruf einen Fehler erzeugt, wird ein Nicht-202-Status zusammen mit einem Antworttext mit zusätzlichen Fehlerdetails zurückgegeben. Der Antworttext ist application/json und enthält ein einzelnes Objekt mit den Membern `error_code` und `message`.

Im Folgenden finden Sie wiederverwendete Fehler-Codes vom Adobe Developer-Gateway.

| HTTP-Status-Code | error_code | Nachricht |
|--- |--- |--- |
| 401 | 401013 | OAuth-Token ist ungültig |
| 403 | 403010 | OAuth-Token fehlt |
| 404 | 404040 | Ressource nicht gefunden |
| 429 | 429001 | Service-Nutzungsbeschränkung erreicht |

Im Folgenden finden Sie Fehler-Codes, die eindeutig für die Datenaufnahme-API sind und aus drei Segmenten bestehen. Die ersten drei Ziffern geben den Status an (zurückgegeben vom Adobe IO Gateway), gefolgt von einer Null „0“, gefolgt von drei Ziffern.

| HTTP-Status-Code | error_code | Nachricht |
|--- |--- |--- |
| 400 | 4000801 | Ungültige Anfrage |
| 400 | 4000802 | Ungültige Daten |
| 403 | 4030801 | Nicht autorisiert |
| 429 | 4290801 | Tägliches Kontingent erreicht |
| 500 | 5000801 | Interner Server-Fehler |

Beispiel einer Fehlerantwort:

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Weitere Zustellversuche

Wenn ein vorübergehender Fehler erkannt wird, versucht der Service den Vorgang dreimal erneut. Der erste erneute Versuch findet nach einer 5-minütigen Wartezeit statt, der zweite nach 30 weiteren Minuten und schließlich der dritte nach 30 weiteren Minuten. Weitere Zustellversuche erfolgen aus verschiedenen Gründen, in erster Linie, wenn ein abhängiger Service eine Zeitüberschreitung aufweist oder vorübergehend nicht verfügbar ist.

## Endpunkte

Aufnahme-Endpunkte sind für Personen und benutzerdefinierte Objekte verfügbar.

### Personen

Endpunkt, der zur Aktualisierung von Personendatensätzen verwendet wird.

| Methode |
|---|
| POST |

| Path |
|---|
| `/subscriptions/{munchkinId}/persons` |

| HeadersKey | Wert |
|---|---|
| Inhaltstyp | application/json |
| x-moto-user-token | {accessToken} |

Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
|---|---|---|---|---|
| Priorität | Zeichenfolge | Nein | Priorität der Anfrage:normalHoch | Normal |
| partitionName | Zeichenfolge | Nein | Name der Personenpartition | Standard |
| deduplizierte Felder | Objekt | Nein | Zu deduplizierende Attribute. Ein oder zwei Attributnamen sind zulässig. In einem AND-Vorgang werden zwei Attribute verwendet. Wenn beispielsweise sowohl `email` als auch `firstName` angegeben sind, können beide zum Suchen einer Person mithilfe des AND-Vorgangs verwendet werden. Unterstützte Attribute sind: `idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` (nur vom Typ „String“ und „Integer„) | E-Mail |
| Personen | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut der Person | – |

| Berechtigung |
|---|
| Lead mit Lese-/Schreibzugriff |

#### Beispiel für Personen

```
POST /subscriptions/{munchkinId}/persons
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

### Benutzerdefinierte Objekte

Endpunkt zum Upsertieren benutzerdefinierter Objektdatensätze.

| Methode |
|---|
| POST |

| Path |
|---|
| `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

Kopfzeilen

| Schlüssel | Wert |
|---|---|
| Inhaltstyp | application/json |
| x-moto-user-token | {accessToken} |

Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
|---|---|---|---|---|
| Priorität | Zeichenfolge | Nein | Priorität der Anfrage:normalHoch | Normal |
| deduplizieren nach | Zeichenfolge | Nein | Zu deduplizierende Attribute für: dedupeFieldsMarketoGUID | deduplizierte Felder |
| customObjects | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut des Objekts. | – |

| Berechtigung |
|---|
| Objekt mit Lese-/Schreibzugriff |

#### Person nicht anwesend

Wenn in der Anfrage ein Verknüpfungsfeld zu einer Person angegeben ist und diese Person nicht vorhanden ist, werden mehrere weitere Zustellversuche unternommen. Wenn diese Person im Wiederholungsfenster (65 Minuten) hinzugefügt wird, ist die Aktualisierung erfolgreich. Wenn beispielsweise das Verknüpfungsfeld auf Person `email` ist und Person nicht vorhanden ist, werden weitere Zustellversuche unternommen.

#### Beispiel für benutzerdefinierte Objekte

```
POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

## Beschränkungen

Im Folgenden finden Sie eine Liste der Leitplanken für die Verwendung:

- Maximale Größe der Anfrage: 1 MB
- Maximale Anzahl von Objekten pro Anfrage pro Objekttyp: 1.000
- Maximale Anzahl von Anfragen pro Sekunde und Client-ID: 5.000
- Maximale Anzahl an Objekten pro Tag: 10.000.000

## Datenaufnahme-API und REST-API im Vergleich

Im Folgenden finden Sie eine Liste der Unterschiede zwischen der Datenaufnahme-API und anderen Marketo-REST-APIs:

- Dies ist keine vollständige CRUD-Schnittstelle, sie unterstützt nur „upsert“.
- Zur Authentifizierung müssen Sie das Zugriffstoken mithilfe der `X-Mkto-User-Token`-Kopfzeile übergeben
- Der URL-Domain-Name lautet `mkto-ingestion-api.adobe.io`
- Der URL-Pfad beginnt mit `/subscriptions/_MunchkinId_`
- Es gibt keine Abfrageparameter
- Wenn der Aufruf erfolgreich ist, wird der Status 202 zurückgegeben und der Antworttext ist leer
- Wenn der Aufruf fehlschlägt, wird ein Nicht-202-Status zurückgegeben und der Antworttext enthält `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- Die Anfrage-ID wird über `X-Request-Id` -Header zurückgegeben
