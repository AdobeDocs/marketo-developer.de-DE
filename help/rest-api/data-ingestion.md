---
title: Datenaufnahme
feature: REST API, Dynamic Content
description: Verwenden Sie die Marketo-Datenaufnahme-API für Upserts von Personen und benutzerdefinierten Objekten mit hohem Volumen und geringer Latenz mit OAuth-Header-Authentifizierung, asynchronen Statusereignissen und erneuten Zustellversuchen.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '978'
ht-degree: 12%

---

# Datenaufnahme-API

Die Datenerfassungs-API ist ein Service mit hoher Datenmenge und geringer Latenz, der mit hoher Verfügbarkeit entwickelt wurde, um die Aufnahme großer Mengen von Personen- und personenbezogenen Daten effizient und mit minimalen Verzögerungen zu handhaben.

Daten werden durch Senden von Anfragen aufgenommen, die asynchron ausgeführt werden. Der Anfragestatus kann abgerufen werden, indem Ereignisse aus dem [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup) abonniert werden.

Schnittstellen werden für zwei Objekttypen angeboten: Personen, Benutzerdefinierte Objekte. Der Datensatzvorgang ist nur „Einfügen oder Aktualisieren“.

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
|-|-|
| Personen | Lead mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte | Objekt mit Lese-/Schreibzugriff |

## Kopfzeilen

Die Datenaufnahme nutzt die folgenden benutzerdefinierten HTTP-Kopfzeilen.

### Anfrage

| Schlüssel | Wert | Erforderlich | Beschreibung |
| - | - | - | - |
| x-correlation-id | Beliebige Zeichenfolge (maximale Länge 255 Zeichen). | Nein | Kann verwendet werden, um Anfragen über das System nachzuverfolgen.  Siehe Marketo Observability Data Stream |
| x-request-Source | Beliebige Zeichenfolge (maximale Länge 50 Zeichen). | Nein | Kann verwendet werden, um die Anfragequelle im System zu verfolgen.  Siehe Marketo Observability Data Stream |

### Antwort

| Schlüssel | Wert | Erforderlich |
| - | - | - |
| x-request-id | Eindeutige Anfrage-ID. | Ja |

## Anfragen

Verwenden Sie die HTTP-POST-Methode, um Daten an den Server zu senden.

Die Datendarstellung ist im Anfragetext als application/json enthalten.

Der Domain-Name lautet: `mkto-ingestion-api.adobe.io`

Der Pfad beginnt mit `/subscriptions/MunchkinId`, wobei MunchkinId spezifisch für Ihre Marketo-Instanz ist. Ihre Munchkin-ID finden Sie in der Marketo Engage-Benutzeroberfläche unter **Admin** > **Mein** > **Support-Informationen**.  Der Rest des Pfads wird verwendet, um die gewünschte Ressource anzugeben.

Beispiel-URL für Personen:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Beispiel-URL für benutzerdefinierte Objekte:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

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

Wenn ein Aufruf einen Fehler erzeugt, wird ein Nicht-202-Status zusammen mit einem Antworttext mit zusätzlichen Fehlerdetails zurückgegeben.  Der Antworttext ist application/json und enthält ein einzelnes Objekt mit Membern und Fehlercode sowie Nachricht.

Im Folgenden finden Sie wiederverwendete Fehler-Codes vom Adobe Developer-Gateway.

| HTTP-Status-Code | error_code | Nachricht |
| - | - | - |
| 401 | 401013 | OAuth-Token ist ungültig |
| 403 | 403010 | OAuth-Token fehlt |
| 404 | 404040 | Ressource nicht gefunden |
| 429 | 429001 | Service-Nutzungsbeschränkung erreicht |

Im Folgenden finden Sie Fehlercodes, die für die Datenaufnahme-API eindeutig sind und aus drei Segmenten bestehen.  Die ersten drei Ziffern geben den Status an (von Adobe Developer Gateway zurückgegeben), gefolgt von einer Null „0“, gefolgt von drei Ziffern.

| HTTP-Status-Code | error_code | Nachricht |
| - | - | - |
| 400 | 4000801 | Ungültige Anfrage |
| 400 | 4000802 | Ungültige Daten |
| 403 | 4030801 | Nicht autorisiert |
| 429 | 4290801 | Tägliches Kontingent erreicht |
| 500 | 5000801 | Interner Server-Fehler |

## Weitere Zustellversuche

Wenn ein vorübergehender Fehler erkannt wird, versucht der Service den Vorgang dreimal erneut.  Der erste erneute Versuch findet nach einer Wartezeit von 5 Minuten statt, der zweite nach 30 weiteren Minuten und schließlich der dritte nach 30 weiteren Minuten.  Weitere Zustellversuche erfolgen aus verschiedenen Gründen, in erster Linie, wenn ein abhängiger Service eine Zeitüberschreitung aufweist oder vorübergehend nicht verfügbar ist.

## Endpunkte

Aufnahme-Endpunkte sind für Personen und benutzerdefinierte Objekte verfügbar.

### Personen

Endpunkt, der zur Aktualisierung von Personendatensätzen verwendet wird.

| Methode | Pfad |
| - | - |
| POST | /subscriptions/{munchkinId}/persons |

#### Kopfzeilen

| Schlüssel | Wert |
| - | - |
| Inhaltstyp | application/json |
| x-moto-user-token | {accessToken} |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| - | - | - | - | - |
| Priorität | Zeichenfolge | Nein | Priorität der Anfrage: normal oder hoch | Normal |
| partitionName | Zeichenfolge | Nein | Name der Personenpartition | Standard |
| deduplizierte Felder | Objekt | Nein | Zu deduplizierende Attribute. Ein oder zwei Attributnamen sind zulässig. <br/> In einem AND-Vorgang werden zwei Attribute verwendet. Wenn beispielsweise sowohl `email` als auch `firstName` angegeben sind, werden sie beide verwendet, um eine Person mithilfe des AND-Vorgangs nachzuschlagen. <br/>Unterstützte Attribute sind: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, benutzerdefinierte Attribute (nur vom Typ „string“ und „integer„) `email` |
| Personen | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut der Person | - |

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

#### Antwort

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Benutzerdefinierte Objekte

Endpunkt zum Upsertieren benutzerdefinierter Objektdatensätze

| Methode | Pfad |
| - | - |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Kopfzeilen

| Schlüssel | Wert |
| - | - |
| Inhaltstyp | application/json |
| x-moto-user-token | {accessToken} |

#### Anfragetext

| Schlüssel | Datentyp | Erforderlich | Wert | Standardwert |
| - |- | - | - | - |
| Priorität | Zeichenfolge | Nein | Priorität der Anfrage: normal, hoch | Normal |
| deduplizieren nach | Zeichenfolge | Nein | Zu deduplizierende Attribute für: dedupeFields, marketoGUID | deduplizierte Felder |
| customObjects | Array von Objekten | Ja | Liste der Name-Wert-Paare für das Attribut des Objekts. | – |

Erforderliche Berechtigungen sind `Read-Write Custom Object`.

Wenn in der Anfrage ein Verknüpfungsfeld zu einer Person angegeben ist und diese Person nicht vorhanden ist, werden mehrere weitere Zustellversuche unternommen. Wenn diese Person im Wiederholungsfenster (65 Minuten) hinzugefügt wird, ist die Aktualisierung erfolgreich. Wenn beispielsweise das Verknüpfungsfeld E-Mail an Person lautet und Person nicht vorhanden ist, werden weitere Zustellversuche unternommen.

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

## Beschränkungen

Im Folgenden finden Sie eine Liste zur Verwendung von Leitplanken:

* Maximale Größe der Anfrage: 1 MB
* Maximale Anzahl von Objekten pro Anfrage pro Objekttyp: 1.000
* Maximale Anzahl von Anfragen pro Sekunde und Client-ID: 5.000
* Maximale Anzahl an Objekten pro Tag: 10.000.000

## Datenaufnahme-API und REST-API im Vergleich

Im Folgenden finden Sie eine Liste der Unterschiede zwischen der Datenaufnahme-API und anderen Marketo-REST-APIs:

* Dies ist keine vollständige CRUD-Schnittstelle, sondern unterstützt nur upsert
* Zur Authentifizierung müssen Sie das Zugriffstoken mithilfe der `X-Mkto-User-Token`-Kopfzeile übergeben
* Der URL-Domain-Name lautet `mkto-ingestion-api.adobe.io`
* Der URL-Pfad beginnt mit `/subscriptions/MunchkinId`
* Es gibt keine Abfrageparameter
* Wenn der Aufruf erfolgreich ist, wird der Status 202 zurückgegeben und der Antworttext ist leer
* Wenn der Aufruf fehlschlägt, wird ein Nicht-202-Status zurückgegeben und der Antworttext enthält `{ "error_code" : "Error Code", "message" : "Message" }`
* Die Anfrage-ID wird über `X-Request-Id` -Header zurückgegeben
