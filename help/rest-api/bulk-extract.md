---
title: Massenextrakt
feature: REST API
description: Erfahren Sie, wie Sie mit der Marketo Bulk Extract-REST-API Leads, Aktivitäten, Programmmitglieder und benutzerdefinierte Objekte mit OAuth, Auftragswarteschlangen und täglichen 500-MB-Beschränkungen exportieren können.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
TQID: https://experienceleague.adobe.com/ECSchsjqp8fyxXbUGl5DgXHUkXuN0sIUc3yJfVaIe1E
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 1%

---

# Massenextrakt

Marketo Bulk Extract bietet Schnittstellen zum Abrufen großer Mengen an Personen- und personenbezogenen Daten. Schnittstellen sind derzeit für vier Objekttypen verfügbar:

- Leads (Personen)
- Aktivitäten
- Programm-Mitglieder
- Benutzerdefinierte Objekte

So führen Sie einen Massenextrakt durch:

1. Erstellen Sie einen Auftrag und definieren Sie die abzurufenden Daten.
1. Den Auftrag in die Warteschlange stellen.
1. Warten Sie, bis der Auftrag das Schreiben der Datei abgeschlossen hat.
1. Rufen Sie die Datei über HTTP ab.

Massenextraktionsaufträge werden asynchron ausgeführt. Abfrage des Vorgangs zum Abrufen des Exportstatus.

`Note:` Bulk-API-Endpunkte haben nicht das Präfix &quot;/rest“ wie andere Endpunkte.

## Authentifizierung

Die Massenextraktions-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo-REST-APIs. Senden Sie ein gültiges Zugriffstoken im `Authorization: Bearer {_AccessToken_}` HTTP-Header.

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 31. August 2026 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

## Beschränkungen

- Maximale Anzahl gleichzeitiger Exportvorgänge: 2
- Maximale Anzahl an Exportvorgängen in der Warteschlange, einschließlich gerade exportierender Aufträge: 10
- Aufbewahrungszeitraum für Dateien: sieben Tage
- Standardmäßige tägliche Exportzuweisung: 500 MB. Die Zuordnung wird täglich um 0:00 Uhr CST zurückgesetzt. Erhöhungen können erworben werden.
- Maximale Zeitspanne für den Datumsbereichsfilter (`createdAt` oder `updatedAt`): 31 Tage

Massenfilter für die Lead-Extraktion für aktualisierte Daten und Smart-Listen sind für einige Abonnementtypen nicht verfügbar. Wenn diese Filter nicht verfügbar sind, gibt der Endpunkt Exportvorgang erstellen den Fehler „1035, Nicht unterstützter Filtertyp für Zielabonnement“ zurück. Wenden Sie sich an den Marketo-Support, um diese Funktion für Ihr Abonnement zu aktivieren.

### Warteschlange

Die APIs für die Massenextraktion verwenden eine Auftragswarteschlange, die von Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten gemeinsam genutzt wird. Rufen Sie zunächst den Endpunkt Export-Lead/Aktivität/Programmteilnehmer-Vorgang erstellen auf, um einen Extraktionsvorgang zu erstellen. Rufen Sie dann den entsprechenden Endpunkt für den Vorgang „Lead-/Aktivitäts-/Programmmitglied-Export in die Warteschlange stellen“ auf, um den Vorgang in die Warteschlange zu stellen. Der Vorgang beginnt, wenn Rechenressourcen verfügbar werden.

Die Warteschlange kann maximal 10 Aufträge enthalten. Wenn Sie versuchen, einen Auftrag in die Warteschlange einzureihen, wenn die Warteschlange voll ist, gibt der Endpunkt „Exportauftrag in die Warteschlange einreihen“ den Fehler „1029, Zu viele Aufträge in der Warteschlange“ zurück. Maximal zwei Aufträge können den Status „Verarbeitung läuft“ haben und gleichzeitig ausgeführt werden.

### Dateigröße

Die APIs für die Massenextraktion basieren auf der Festplattengröße der Daten, die ein Massenextraktionsauftrag abruft. Um die Dateigröße in Byte zu ermitteln, lesen Sie das Attribut `fileSize` in der Statusantwort Abgeschlossen für einen Exportvorgang.

Das tägliche Kontingent beträgt 500 MB und wird zwischen Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten geteilt. Wenn Sie das Kontingent überschreiten, können Sie keinen anderen Auftrag erstellen oder in die Warteschlange aufnehmen, bis das Kontingent um Mitternacht ([) zurückgesetzt &#x200B;](https://en.wikipedia.org/wiki/Central_Time_Zone). Bis zum Zurücksetzen gibt die API den Fehler „1029, Export Daily Kontingent exceeded“ zurück. Abgesehen vom täglichen Kontingent gibt es keine maximale Dateigröße.

Nachdem ein Auftrag in die Warteschlange gestellt oder verarbeitet wurde, wird er bis zum Ende ausgeführt, es sei denn, ein Fehler tritt auf oder Sie brechen den Auftrag ab. Wenn ein Auftrag fehlschlägt, müssen Sie ihn neu erstellen.

Die API schreibt die vollständige Datei nur, wenn der Auftrag den Status „Abgeschlossen“ erreicht. Es werden keine Teildateien geschrieben. Um die Datei zu überprüfen, berechnen Sie ihren SHA-256-Hash und vergleichen Sie ihn mit der Prüfsumme, die der Auftragsstatus-Endpunkt zurückgibt.

Um den gesamten für den aktuellen Tag verwendeten Speicherplatz zu ermitteln, rufen Sie einen Endpunkt für Vorgänge des Typs Lead/Aktivität/Programmteilnehmer abrufen. Diese Endpunkte geben alle Aufträge der letzten sieben Tage zurück.

Filtern Sie die Liste nach Aufträgen, die am aktuellen Tag abgeschlossen wurden, indem Sie die Attribute `status` und `finishedAt` verwenden. Fügen Sie dann die Dateigrößen für diese Aufträge hinzu. Sie können eine Datei nicht löschen, um Festplattenspeicher freizugeben.

## Berechtigungen

Die Massenextraktion verwendet dasselbe Berechtigungsmodell wie die Marketo-REST-API. Es sind keine zusätzlichen speziellen Berechtigungen erforderlich, aber jeder Satz von Endpunkten erfordert spezifische Berechtigungen.

Nur der API-Benutzer, der einen Massenextraktionsauftrag erstellt hat, kann darauf zugreifen, seinen Status abfragen oder seine Dateiinhalte abrufen.

Massenextraktionsendpunkte kennen Marketo-Arbeitsbereiche nicht. Extraktionsanfragen enthalten Daten aus allen Arbeitsbereichen, unabhängig davon, wie Sie die API für Ihren benutzerdefinierten Service „Nur Benutzer“ definieren.

## Erstellen von Aufträgen

Die Massenextraktions-APIs von Marketo verwenden Aufträge zum Initiieren und Ausführen von Datenextraktionen. Die folgende Anfrage erstellt einen Lead-Exportvorgang:

```http
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

Diese Anfrage erstellt einen Auftrag, der alle Leads exportiert, die zwischen dem 1. Januar 2023 und dem 31. Januar 2023 erstellt wurden. Die CSV-Datei enthält Werte aus den Feldern „firstName“ und „lastName“ und verwendet die Spaltenüberschriften „first name“ und „lastName“.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Die Antwort gibt die Auftrags-ID im `exportId` zurück. Verwenden Sie diese Auftrags-ID, um den Auftrag in die Warteschlange einzureihen oder abzubrechen, seinen Status zu überprüfen oder die abgeschlossene Datei abzurufen.

### Allgemeine Parameter

Jeder Auftragserstellungsendpunkt verfügt über gemeinsame Parameter zum Konfigurieren des Dateiformats, der Feldnamen und des Filters. Jeder Subtyp des Extraktionsvorgangs kann auch zusätzliche Parameter aufweisen:

| Parameter | Datentyp | Hinweise |
| --- | --- | --- |
| Format | String | Bestimmt das Dateiformat der extrahierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und Semikolon-getrennte Werte. Akzeptiert eine der folgenden Optionen: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| columnHeaderNames | Objekt | Ermöglicht das Festlegen der Namen von Spaltenüberschriften in der zurückgegebenen Datei. Jeder Memberschlüssel ist der Name der umzubenennenden Spaltenüberschrift und der Wert ist der neue Name der Spaltenüberschrift. Beispiel: „columnHeaderNames“: { „firstName“: „First Name“, „lastName“: „Last Name“ }, |
| filter | Objekt | Auf den Extraktionsvorgang angewendeter Filter. Die Typen und Optionen variieren je nach Vorgangstyp. |

## Abrufen von Aufträgen

Verwenden Sie den Endpunkt Exportaufträge abrufen für den entsprechenden Objekttyp, um aktuelle Aufträge abzurufen. Jeder Endpunkt für Abrufen von Exportvorgängen unterstützt diese Parameter:

- `status` filtert Aufträge nach Exportstatus. Gültige Werte sind Erstellt, In die Warteschlange gestellt, Verarbeitet, Abgebrochen, Abgeschlossen und Fehlgeschlagen.
- `batchSize` begrenzt die Anzahl der zurückgegebenen Aufträge. Der Standard- und Höchstwert ist 300.
- `nextPageToken` von Seiten durch große Ergebnismengen.

Die folgende Anfrage ruft Lead-Exportaufträge mit dem Status „Abgeschlossen“ oder „Fehlgeschlagen“ ab:

```http
GET /bulk/v1/leads/export.json?status=Completed,Failed
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

Das Ergebnis-Array enthält die Statusantwort für jeden Auftrag, der in den letzten sieben Tagen für diesen Objekttyp erstellt wurde. Die Antwort enthält nur Aufträge, deren Inhaber der API-Benutzer ist, der den Aufruf ausführt.

## Starten eines Vorgangs

Nachdem Sie einen Auftrag erstellt haben, verwenden Sie dessen Auftrags-ID, um ihn in die Warteschlange einzureihen und zu starten:

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Die Anfrage startet den Auftrag und gibt eine Statusantwort zurück. Da Exporte asynchron ausgeführt werden, fragen Sie den Auftragsstatus ab, um festzustellen, wann der Export abgeschlossen ist.

## Status des Abrufauftrags

Fragen Sie den Status-Endpunkt ab, um den Fortschritt eines Auftrags zu ermitteln. Nur der API-Benutzer, der einen Auftrag erstellt hat, kann dessen Status abfragen.

Ein Auftragsstatus wird nicht häufiger als einmal alle 60 Sekunden aktualisiert. Führen Sie keine häufigeren Befragungen durch. In den meisten Fällen ist eine Abfrage alle 5 Minuten ausreichend. Daten von jedem erfolgreichen Export werden 10 Tage lang gespeichert.

```http
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

Das innere `status` gibt den Fortschritt des Auftrags an. Sein Wert kann erstellt, in die Warteschlange gestellt, verarbeitet, abgebrochen, abgeschlossen oder fehlgeschlagen sein.

In diesem Beispiel ist der Vorgang abgeschlossen, sodass Sie den Abruf stoppen und die Datei abrufen können. Bei einem abgeschlossenen Auftrag gibt das `fileSize`-Element die Gesamtdateilänge in Byte an, und das `fileChecksum`-Element enthält den SHA-256-Hash der Datei. Der Auftragsstatus ist 30 Tage lang verfügbar, nachdem der Auftrag den Status Abgeschlossen oder Fehlgeschlagen erreicht hat.

## Daten abrufen

Nachdem der Auftrag abgeschlossen ist, rufen Sie die exportierte Datei ab:

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

Die Antwort enthält die Datei in dem Format, das für den Auftrag konfiguriert wurde. Wenn der Auftrag unvollständig ist oder die Anfrage eine ungültige Auftrags-ID enthält, gibt der Dateiendpunkt den Status „404 Nicht gefunden“ und eine Klartext-Fehlermeldung zurück. Diese Antwort unterscheidet sich von den meisten anderen Marketo REST-Endpunktantworten.

Um das teilweise und wiederholbare Abrufen zu unterstützen, unterstützt der Dateiendpunkt den optionalen HTTP-`Range`-Header mit dem `bytes`, wie in [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233) definiert. Wenn Sie die -Kopfzeile nicht festlegen, gibt der Endpunkt die gesamte Datei zurück.

Um die ersten 10.000 Byte einer Datei abzurufen, übergeben Sie die folgende Kopfzeile in der GET-Anfrage. Der Bereich beginnt bei Byte 0:

```text
Range: bytes=0-9999
```

Für eine partielle Datei gibt der Endpunkt den Status-Code 206 sowie die Kopfzeilen Accept-Ranges, Content-Length und Content-Range zurück:

```text
Accept-Ranges: bytes
Content-Length: 10000
Content-Range: bytes 0-9999/123424
```

### Teilweiser Abruf und Wiederaufnahme

Verwenden Sie den `Range`-Header, um einen Teil einer Datei abzurufen oder einen Abruf fortzusetzen. Der Dateibereich beginnt bei Byte 0 und endet bei dem Wert `fileSize` minus 1. Der Endpunkt Exportdatei abrufen zeigt auch die Dateilänge als Nenner im `Content-Range`-Antwortheader an.

Wenn ein Abruf teilweise fehlschlägt, können Sie ihn fortsetzen. Wenn Sie beispielsweise versuchen, eine 1.000-Byte-Datei abzurufen, aber nur die ersten 725 Byte erhalten, rufen Sie den Endpunkt erneut auf und übergeben Sie einen neuen Bereich:

```text
Range: bytes=725-999
```

Diese Anfrage gibt die verbleibenden 275 Byte der Datei zurück.

#### Prüfung der Dateiintegrität

Wenn `status` „Abgeschlossen“ ist, geben die Auftragsstatus-Endpunkte eine Prüfsumme im `fileChecksum` Attribut zurück. Die Prüfsumme ist der SHA-256-Hash der exportierten Datei. Vergleichen Sie sie mit dem SHA-256-Hash der abgerufenen Datei, um zu überprüfen, ob die Datei vollständig ist.

Die folgende Antwort enthält eine Prüfsumme:

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

Im folgenden Beispiel wird das Befehlszeilendienstprogramm sha256sum verwendet, um den SHA-256-Hash einer abgerufenen Datei mit dem Namen „bulk_lead_export.csv“ zu erstellen:

```bash
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert oder nicht mehr erforderlich ist, brechen Sie ihn ab:

```http
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Der Antwortstatus gibt an, dass der Vorgang abgebrochen wurde.
