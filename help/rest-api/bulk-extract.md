---
title: Massenextrakt
feature: REST API
description: Batch-Vorgänge für die Extraktion von Marketo-Daten.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1682'
ht-degree: 1%

---

# Massenextrakt

Marketo bietet Schnittstellen zum Abrufen großer Mengen von Personen- und personenbezogenen Daten, die als Massenextraktion bezeichnet werden. Derzeit werden Schnittstellen für drei Objekttypen angeboten:

- Leads (Personen)
- Aktivitäten
- Programm-Mitglieder
- Benutzerdefinierte Objekte

Die Massenextraktion erfolgt durch Erstellen eines Auftrags, Definieren des abzurufenden Satzes von Daten, Einreihen des Auftrags in die Warteschlange, Warten darauf, dass der Auftrag zum Abschließen des Schreibens einer Datei abgeschlossen ist, und anschließendes Abrufen der Datei über HTTP. Diese Aufträge werden asynchron ausgeführt und können abgerufen werden, um den Status des Exports abzurufen.

`Note:` Bulk-API-Endpunkte haben nicht das Präfix &quot;/rest“ wie andere Endpunkte.

## Authentifizierung

Die Massenextraktions-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo-REST-APIs. Dazu muss ein gültiges Zugriffstoken als HTTP-Header-`Authorization: Bearer {_AccessToken_}` gesendet werden.

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 30. Juni 2025 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

## Beschränkungen

- Max. gleichzeitige Exportvorgänge: 2
- Max. in die Warteschlange gestellte Exportaufträge (einschließlich der aktuell exportierten Aufträge): 10
- Aufbewahrungszeitraum für Dateien: sieben Tage
- Standardmäßige tägliche Exportzuweisung: 500 MB (wird täglich mit 12 % CST :00AM). Erhöhungen können erworben werden.
- Maximale Zeitspanne für den Datumsbereichsfilter (createdAt oder updatedAt): 31 Tage

Massenfilter für die Lead-Extraktion für aktualisierte Daten und Smart-Listen sind für einige Abonnementtypen nicht verfügbar. Wenn nicht verfügbar, gibt ein Aufruf des Endpunkts „Export-Lead-Auftrag erstellen“ den Fehler „1035, Nicht unterstützter Filtertyp für Zielabonnement“ zurück. Kunden können sich an den Marketo-Support wenden, um diese Funktion in ihrem Abonnement aktivieren zu lassen.

### Warteschlange

Die APIs für die Massenextraktion verwenden eine Auftragswarteschlange (die von Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten gemeinsam genutzt wird). Extraktionsaufträge müssen zuerst erstellt und dann in die Warteschlange gestellt werden, indem die Endpunkte „Export-Lead/Aktivität/Programmteilnehmer-Auftrag erstellen“ und „Export-Lead/Aktivität/Programmteilnehmer-Auftrag in die Warteschlange stellen“ aufgerufen werden. Nach der Aktivierung werden die Aufträge aus der Warteschlange gezogen und gestartet, sobald Rechenressourcen verfügbar werden.

Die maximale Anzahl von Aufträgen in der Warteschlange ist 10. Wenn Sie versuchen, einen Auftrag in die Warteschlange einzureihen, wenn die Warteschlange voll ist, gibt der Endpunkt „Exportauftrag in die Warteschlange einreihen“ den Fehler „1029, Zu viele Aufträge in der Warteschlange“ zurück. Es können maximal zwei Aufträge gleichzeitig ausgeführt werden (Status ist „Verarbeitung läuft„).

### Dateigröße

Die APIs für die Massenextraktion basieren auf der Größe der Daten, die über einen Massenextraktionsauftrag abgerufen werden. Die explizite Größe in Byte für einen Vorgang kann durch Lesen des `fileSize` Attributs aus der Antwort „Abgeschlossener Status“ eines Exportvorgangs bestimmt werden.

Das tägliche Kontingent beträgt maximal 500 MB pro Tag, die von Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten gemeinsam genutzt werden. Wenn das Kontingent überschritten wird, können Sie keinen anderen Auftrag erstellen oder in die Warteschlange stellen, bis das tägliche Kontingent um Mitternacht ([) zurückgesetzt ](https://en.wikipedia.org/wiki/Central_Time_Zone). Bis zu diesem Zeitpunkt wird der Fehler „1029, Export Daily Kontingent exceeded“ zurückgegeben. Abgesehen vom täglichen Kontingent gibt es keine maximale Dateigröße.

Sobald ein Auftrag in die Warteschlange gestellt wurde oder verarbeitet wird, wird er bis zum Ende ausgeführt (sofern kein Fehler vorliegt oder der Auftrag nicht abgebrochen wurde). Wenn ein Auftrag aus irgendeinem Grund fehlschlägt, müssen Sie ihn neu erstellen. Dateien werden nur dann vollständig geschrieben, wenn ein Auftrag den Status Abgeschlossen erreicht (partielle Dateien werden nie geschrieben). Sie können überprüfen, ob eine Datei vollständig geschrieben wurde, indem Sie ihren SHA-256-Hash berechnen und diesen mit der Prüfsumme vergleichen, die von Auftragsstatus-Endpunkten zurückgegeben wird.

Sie können die Gesamtmenge der für den aktuellen Tag verwendeten Festplatte ermitteln, indem Sie „Export-Lead/Aktivität/Programmteilnehmeraufträge abrufen“ aufrufen. Diese Endpunkte geben eine Liste aller Aufträge der letzten sieben Tage zurück. Sie können diese Liste nach den Aufträgen filtern, die am aktuellen Tag abgeschlossen wurden (mithilfe der Attribute `status` und `finishedAt`). Addieren Sie dann die Dateigrößen für diese Aufträge, um die insgesamt verwendete Menge zu generieren. Es gibt keine Möglichkeit, eine Datei zu löschen, um Speicherplatz freizugeben.

## Berechtigungen

Die Massenextraktion verwendet dasselbe Berechtigungsmodell wie die Marketo-REST-API und erfordert keine zusätzlichen speziellen Berechtigungen zur Verwendung, obwohl für jeden Endpunktsatz spezifische Berechtigungen erforderlich sind.

Massenextraktionsaufträge sind nur für den API-Benutzer zugänglich, der sie erstellt hat, einschließlich des Abrufs des Status und des Abrufs von Dateiinhalten.

Massenextraktionsendpunkte kennen Marketo-Arbeitsbereiche nicht. Extraktionsanfragen enthalten immer Daten über alle Arbeitsbereiche hinweg, unabhängig davon, wie Sie die API für Ihren benutzerdefinierten Service „Nur Benutzer“ definieren.

## Erstellen von Aufträgen

Die Massenextraktions-APIs von Marketo verwenden das Konzept eines Auftrags zum Initiieren und Ausführen der Datenextraktion. Betrachten wir das Erstellen eines einfachen Lead-Exportvorgangs.

```
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

Mit dieser einfachen Anfrage wird ein Auftrag erstellt, der die in den Feldern „firstName“ und „lastName“ enthaltenen Werte zurückgibt und die Spaltenüberschriften „First Name“ und „Last Name“ als CSV-Datei enthält, die jeden Lead enthält, der zwischen dem 1. Januar 2023 und dem 31. Januar 2023 erstellt wurde.

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

Wenn wir den Auftrag erstellen, wird eine Auftrags-ID im `exportId`-Attribut zurückgegeben. Anschließend können wir diese Auftrags-ID verwenden, um den Auftrag in eine Warteschlange einzureihen, abzubrechen, seinen Status zu überprüfen oder die abgeschlossene Datei abzurufen.

### Allgemeine Parameter

Jeder Auftragserstellungsendpunkt verwendet einige allgemeine Parameter zum Konfigurieren des Dateiformats, der Feldnamen und des Filters eines Massenextraktionsauftrags. Jeder Subtyp des Extraktionsvorgangs kann über zusätzliche Parameter verfügen:

| Parameter | Datentyp | Hinweise |
|---|---|---|
| Format | String | Bestimmt das Dateiformat der extrahierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und Semikolon-getrennte Werte. Akzeptiert eine der folgenden Optionen: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| columnHeaderNames | Objekt | Ermöglicht das Festlegen der Namen von Spaltenüberschriften in der zurückgegebenen Datei. Jeder Memberschlüssel ist der Name der umzubenennenden Spaltenüberschrift und der Wert ist der neue Name der Spaltenüberschrift. Beispiel: „columnHeaderNames“: { „firstName“: „First Name“, „lastName“: „Last Name“ }, |
| filter | Objekt | Auf den Extraktionsvorgang angewendeter Filter. Die Typen und Optionen variieren je nach Vorgangstyp. |

## Abrufen von Aufträgen

Manchmal müssen Sie möglicherweise Ihre aktuellen Aufträge abrufen. Dies ist mit den GET-Exportvorgängen für den entsprechenden Objekttyp einfach möglich. Jeder Endpunkt Abrufen von Exportvorgängen unterstützt ein `status` Filterfeld, eine  `batchSize`, um die Anzahl der zurückgegebenen Aufträge zu begrenzen, und `nextPageToken` für das Paging durch große Ergebnismengen. Der Statusfilter unterstützt jeden gültigen Status für einen Exportvorgang: Erstellt, In Warteschlange, Verarbeitung läuft, Abgebrochen, Abgeschlossen und Fehlgeschlagen. Die batchSize-Eigenschaft ist auf maximal 300 festgelegt. Rufen wir die Liste der Lead-Exportvorgänge ab:

```
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

Der Endpunkt antwortet mit `status` Antwort jedes Auftrags, der in den letzten sieben Tagen für diesen Objekttyp im Ergebnis-Array erstellt wurde. Die Antwort enthält nur Ergebnisse für Aufträge des API-Benutzers, der den Aufruf ausführt.

## Starten eines Vorgangs

Lassen Sie uns mit unserer Job-ID anfangen:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Dadurch wird die Ausführung des Auftrags gestartet und eine Statusantwort zurückgegeben. Da der Export immer asynchron erfolgt, müssen wir den Status des Auftrags abfragen, um festzustellen, ob er abgeschlossen wurde. Der Status für einen bestimmten Auftrag wird nicht häufiger als einmal alle 60 Sekunden aktualisiert, daher sollte der Status nie häufiger als dieser abgefragt werden. Beachten Sie jedoch, dass in den meisten Anwendungsfällen die Abfrage nie häufiger als einmal alle 5 Minuten erforderlich sein sollte. Daten von jedem erfolgreichen Export werden 10 Tage lang gespeichert.

## Status des Abrufauftrags

Die Bestimmung des Status des Auftrags ist einfach.

Der Status kann nur für Aufträge abgerufen werden, die von demselben API-Benutzer erstellt wurden, der sie erstellt hat.

```
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

Das innere `status` gibt den Fortschritt des Auftrags an und kann einen der folgenden Werte aufweisen: Erstellt, In Warteschlange, Verarbeitung läuft, Abgebrochen, Abgeschlossen, Fehlgeschlagen. In diesem Fall ist unser Vorgang abgeschlossen, sodass wir den Abruf stoppen und mit dem Abrufen der Datei fortfahren können. Nach Abschluss gibt das `fileSize`-Element die Gesamtlänge der Datei in Byte an, und das `fileChecksum`-Element enthält den SHA-256-Hash der Datei. Der Auftragsstatus ist 30 Tage lang verfügbar, nachdem der Status „Abgeschlossen“ oder „Fehlgeschlagen“ erreicht wurde.

## Daten abrufen

Wenn Ihr Auftrag abgeschlossen ist, können Sie die Datei einfach abrufen.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

Die Antwort enthält eine -Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der -Datei. Wenn ein Auftrag nicht abgeschlossen oder eine fehlerhafte Auftrags-ID übergeben wurde, antworten die Dateiendpunkte mit dem Status „404 Not Found“ und einer Klartext-Fehlermeldung als Payload, im Gegensatz zu den meisten anderen Marketo-REST-Endpunkten.

Um das teilweise und fortsetzungsfreundliche Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional die HTTP-Header-`Range` vom Typ `bytes` (gemäß [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Um die ersten 10.000 Byte einer Datei abzurufen, würden Sie die folgende Kopfzeile als Teil Ihrer GET-Anfrage an den -Endpunkt übergeben, beginnend mit Byte 0:

```
Range: bytes=0-9999
```

Beim Abrufen der partiellen Datei antwortet der Endpunkt mit Status-Code 206 und gibt die Kopfzeilen Accept-Ranges, Content-Length und Content-Range zurück:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Teilweiser Abruf und Wiederaufnahme

Dateien können teilweise abgerufen oder später mit dem `Range`-Header fortgesetzt werden. Der Bereich für eine Datei beginnt bei Byte 0 und endet bei dem Wert `fileSize` minus 1. Die Länge der Datei wird auch als Nenner im Wert des `Content-Range`-Antwort-Headers beim Aufruf eines GET-Exportdatei-Endpunkts angegeben. Schlägt ein Abruf teilweise fehl, kann er später fortgesetzt werden. Wenn Sie beispielsweise versuchen, eine Datei mit einer Länge von 1.000 Byte abzurufen, aber nur die ersten 725 Byte empfangen wurden, kann der Abruf an der Fehlerstelle erneut versucht werden, indem Sie den Endpunkt erneut aufrufen und einen neuen Bereich übergeben:

```
Range: bytes 724-999
```

Dadurch werden die verbleibenden 275 Byte der Datei zurückgegeben.

#### Prüfung der Dateiintegrität

Die Auftragsstatus-Endpunkte geben eine Prüfsumme im `fileChecksum`-Attribut zurück, wenn `status` „Abgeschlossen“ ist. Die Prüfsumme ist ein SHA-256-Hash der exportierten Datei. Sie können die Prüfsumme mit dem SHA-256-Hash der abgerufenen Datei vergleichen, um zu überprüfen, ob sie vollständig ist.

Im Folgenden finden Sie eine Beispielantwort mit der Prüfsumme:

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

Im Folgenden finden Sie ein Beispiel für die Erstellung des SHA-256-Hash einer abgerufenen Datei mit dem Namen „bulk_lead_export.csv“ mithilfe des Befehlszeilen-Dienstprogramms „sha256sum“:

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach abgebrochen werden:

```
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

Daraufhin wird ein Status angezeigt, der angibt, dass der Vorgang abgebrochen wurde.
