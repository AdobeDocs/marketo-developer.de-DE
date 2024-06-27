---
title: "Datenerfassung"
description: "Übersicht über die Datenerfassungs-API"
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 11%

---


# Datenaufnahme

Die Data Ingestion-API ist ein hochverfügbarer Dienst mit hohem Volumen und geringer Latenz, der dazu bestimmt ist, große Mengen von personenbezogenen Daten effizient und mit minimalen Verzögerungen zu erfassen. 

Daten werden durch Senden von Anforderungen erfasst, die asynchron ausgeführt werden. Der Anfragestatus kann abgerufen werden, indem Sie Ereignisse über das [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/)&#x200B;

Es stehen Schnittstellen für zwei Objekttypen zur Verfügung: Personen, benutzerdefinierte Objekte. Der Datensatzvorgang ist nur &quot;insert or update&quot;.

Die Data Ingestion-API befindet sich in der privaten Beta-Phase. Einladende Personen müssen über eine Berechtigung für [Marketo Engage-Leistungs-Tier-Paket](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Authentifizierung

Die Data Ingestion-API verwendet dieselbe OAuth 2.0-Authentifizierungsmethode wie die Marketo REST-API, um ein Zugriffstoken zu generieren. Das Zugriffstoken muss jedoch über die HTTP-Kopfzeile übergeben werden `X-Mkto-User-Token`. Sie können das Zugriffstoken nicht über einen Abfrageparameter übergeben.

Beispiel-Zugriffstoken über Kopfzeile:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Berechtigungen

Die Datenerfassung verwendet dasselbe Berechtigungsmodell wie die Marketo REST-API und erfordert keine zusätzlichen speziellen Berechtigungen, obwohl für jeden Endpunkt spezifische Berechtigungen erforderlich sind.

| Endpunkt | Berechtigung |
|---|---|
| Personen | Lead mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte | Objekt mit Lese-/Schreibzugriff |

## Kopfzeile

Die Datenerfassung nutzt die folgenden benutzerdefinierten HTTP-Header.

### Anfrage

| Schlüssel | Wert | Erforderlich | Beschreibung |
|---|---|---|---|
| X-Korrelation-Id | Beliebige Zeichenfolge (maximal 255 Zeichen). | Nein | Kann verwendet werden, um Anfragen über das System nachzuverfolgen. Siehe Marketo Observability Data Stream |
| X-Request-Source | Beliebige Zeichenfolge (maximal 50 Zeichen). | Nein | Kann verwendet werden, um die Anforderungsquelle über das System zu verfolgen. Siehe Marketo Observability Data Stream |

### Antwort

| Schlüssel | Wert | Erforderlich | Beschreibung |
|---|---|---|---|
| X-Request-Id | Eindeutige Anfrage-ID. | Ja | |

## Anfragen

Verwenden Sie die HTTP-POST-Methode, um Daten an den Server zu senden.

Die Datendarstellung ist als application/json im Anfragetext enthalten.

Der Domänenname lautet: `mkto-ingestion-api.adobe.io`

Der Pfad beginnt mit `/subscriptions/_MunchkinId_` where `_MunchkinId_` ist spezifisch für Ihre Marketo-Instanz. Sie finden Ihre Munchkin-ID in der Marketo Engage-Benutzeroberfläche unter **Admin** >**Mein Konto** > **Support-Info**. Der Rest des Pfads wird verwendet, um die Zielressource anzugeben.

Beispiel-URL für Personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Beispiel-URL für benutzerdefinierte Objekte:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Antworten

Alle Antworten geben eine eindeutige Anfrage-ID über die `X-Request-Id` -Kopfzeile.

Beispiel einer Anfrage-ID über die Kopfzeile:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Erfolg

Bei erfolgreichem Aufruf wird der Status 202 zurückgegeben. Es wird kein Antworttext zurückgegeben.

Beispiel einer Erfolgsantwort:

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Fehler

Wenn ein Aufruf einen Fehler erzeugt, wird der Status &quot;Nicht 202&quot;zusammen mit einem Antworttext mit zusätzlichen Fehlerdetails zurückgegeben. Der Antworttext lautet application/json und enthält ein einzelnes Objekt mit Elementen `error_code` und `message`.

Nachstehend finden Sie wiederverwendete Fehlercodes von Adobe Developer Gateway.

| HTTP-Statuscode | error_code | Nachricht |
|--- |--- |--- |
| 401 | 401013 | Authentifizierungs-Token ist ungültig |
| 403 | 403010 | OAuth-Token fehlt |
| 404 | 404040 | Ressource nicht gefunden |
| 429 | 429001 | Service-Nutzungslimit erreicht |

Im Folgenden finden Sie Fehlercodes, die für die Datenerfassungs-API eindeutig sind und aus drei Segmenten bestehen. Die ersten drei Stellen sind der Status (wird von Adobe IO Gateway zurückgegeben), gefolgt von einer Null &quot;0&quot;, gefolgt von drei Ziffern.

| HTTP-Statuscode | error_code | Nachricht |
|--- |--- |--- |
| 400 | 4000801 | Fehlerhafte Anfrage |
| 400 | 4000802 | Ungültige Daten |
| 403 | 4030801 | Nicht autorisiert |
| 429 | 4290801 | Tägliche Quote erreicht |
| 500 | 5000801 | Interner Serverfehler |

Beispiel einer Fehlerantwort:

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Weitere Zustellversuche

Wenn ein vorübergehender Fehler erkannt wird, wiederholt der Dienst den Vorgang dreimal. Der erste Versuch erfolgt nach einer Wartezeit von 5 Minuten, der zweite nach 30 weiteren Minuten und schließlich der dritte nach 30 weiteren Minuten. Wiederholungen treten aus verschiedenen Gründen auf, insbesondere wenn ein abhängiger Dienst eine Zeitüberschreitung aufweist oder vorübergehend nicht verfügbar ist.

## Endpunkte

Aufnahmeendpunkte sind für Personen und benutzerdefinierte Objekte verfügbar.

### Personen

Endpunkt, der zum Aktualisieren von Personendatensätzen verwendet wird.

| Methode |
|---|
| POST |

| Pfad |
|---|
| `/subscriptions/{munchkinId}/persons` |

| HeadersKey | Wert |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Anfrageinhalt

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
|---|---|---|---|---|
| Priorität | Zeichenfolge | Nein | Priorität der Anforderung: normal high | normal |
| partitionName | Zeichenfolge | Nein | Name der Personenpartition | Standard |
| dedupeFields | Objekt | Nein | Attribute, die dedupliziert werden sollen. Ein oder zwei Attributnamen sind zulässig. In einem UND-Vorgang werden zwei Attribute verwendet. Wenn zum Beispiel beide `email` und `firstName` festgelegt sind, werden sie beide verwendet, um eine Person zu suchen, die den UND-Vorgang verwendet. Folgende Attribute werden unterstützt:`idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` attributes (&quot;string&quot;- und &quot;integer&quot;-Typ) | E-Mail |
| Personen | Array of Object | Ja | Liste der Attributname-Wert-Paare für die Person | - |

| Berechtigung |
|---|
| Lead mit Lese-/Schreibzugriff |

#### Personenbeispiel

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

Endpunkt, der zum Aktualisieren benutzerdefinierter Objektdatensätze verwendet wird.

| Methode |
|---|
| POST |

| Pfad |
|---|
| `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

Kopfzeile

| Schlüssel | Wert |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Anfrageinhalt

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
|---|---|---|---|---|
| Priorität | Zeichenfolge | Nein | Priorität der Anforderung: normal high | normal |
| dedupeBy | Zeichenfolge | Nein | Attribute zum Deduplizieren auf:dedupeFieldsmarketoGUID | dedupeFields |
| customObjects | Array of Object | Ja | Liste der Attributname-Wert-Paare für das Objekt. | - |

| Berechtigung |
|---|
| Objekt mit Lese-/Schreibzugriff |

#### Person nicht vorhanden

Wenn in der Anfrage ein Feld für die Verknüpfung mit einer Person angegeben ist und diese Person nicht vorhanden ist, werden mehrere Zustellversuche unternommen. Wenn diese Person während des Wiederholungsfensters (65 Minuten) hinzugefügt wird, ist die Aktualisierung erfolgreich. Wenn das Link-Feld beispielsweise `email` auf &quot;Person&quot;und &quot;Person existiert nicht&quot;, werden weitere Zustellversuche unternommen.

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

Im Folgenden finden Sie eine Liste der Verwendung von Limits:

- Maximale Anforderungsgröße: 1 MB
- Maximale Objekte pro Anforderung pro Objekttyp: 1.000
- Maximale Anforderungen pro Sekunde pro Client-ID: 5.000
- Maximale Objekte pro Tag: 10.000.000

## Data Ingestion-API vs. REST-API

Im Folgenden finden Sie eine Liste der Unterschiede zwischen der Data Ingestion-API und anderen Marketo REST-APIs:

- Dies ist keine vollständige CRUD-Oberfläche, es unterstützt nur &quot;upsert&quot;
- Um sich zu authentifizieren, müssen Sie das Zugriffstoken mithilfe der `X-Mkto-User-Token` header
- Der URL-Domänenname lautet `mkto-ingestion-api.adobe.io`
- Der URL-Pfad beginnt mit `/subscriptions/_MunchkinId_`
- Es gibt keine Abfrageparameter
- Bei erfolgreichem Aufruf wird der Status 202 zurückgegeben und der Antworttext ist leer
- Wenn der Aufruf fehlschlägt, wird der Status &quot;Nicht 202&quot;zurückgegeben und der Antworttext enthält `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- Die Anfrage-ID wird über zurückgegeben. `X-Request-Id` header
