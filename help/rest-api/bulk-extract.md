---
title: Massenextraktion
feature: REST API
description: Stapelvorgänge zum Extrahieren von Marketo-Daten.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 1%

---

# Massenextraktion

Marketo bietet Schnittstellen zum Abrufen großer Mengen von personenbezogenen und personenbezogenen Daten, die als Massenextraktion bezeichnet werden. Derzeit sind Schnittstellen für drei Objekttypen verfügbar:

- Leads (Personen)
- Aktivitäten
- Programm-Mitglieder
- Benutzerdefinierte Objekte

Die Massenextraktion wird ausgeführt, indem ein Auftrag erstellt wird, der Datensatz zum Abrufen definiert wird, der Auftrag in die Warteschlange gestellt wird, darauf gewartet wird, dass der Auftrag das Schreiben einer Datei abgeschlossen hat, und dann die Datei über HTTP abgerufen wird. Diese Aufträge werden asynchron ausgeführt und können abgerufen werden, um den Status des Exports abzurufen.

`Note:` Massen-API-Endpunkte haben nicht wie andere Endpunkte das Präfix &quot;/rest&quot;.

## Authentifizierung

Die Massenextraktions-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo REST-APIs. Dazu muss ein gültiges Zugriffstoken entweder als Abfragezeichenfolgenparameter `access_token={_AccessToken_}` oder als HTTP-Header `Authorization: Bearer {_AccessToken_}` eingebettet werden.

## Beschränkungen

- Max. gleichzeitige Exportvorgänge: 2
- Max. In die Warteschlange gestellte Exportaufträge (einschließlich der derzeit exportierten Aufträge): 10
- Aufbewahrungsfrist für Dateien: sieben Tage
- Standardmäßige tägliche Exportzuordnung: 500 MB (wird täglich um 12:00 Uhr CST zurückgesetzt). Für den Kauf verfügbare Steigerungen.
- Max. Besuchszeit für Datumsbereichsfilter (createdAt oder updatedAt): 31 Tage

Massenextraktionsfilter für UpdatedAt und Smart List sind bei einigen Abonnementtypen nicht verfügbar. Wenn nicht verfügbar, gibt ein Aufruf an den Endpunkt &quot;Lead-Vorgang erstellen&quot;den Fehler &quot;1035, Nicht unterstützter Filtertyp für Zielabonnement&quot;zurück. Kunden können sich an den Marketo-Support wenden, damit diese Funktion in ihrem Abonnement aktiviert wird.

### Warteschlange

Die Bulk-Extract-APIs verwenden eine Auftragswarteschlange (freigegeben zwischen Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten). Extraktionsaufträge müssen zuerst erstellt und dann durch Aufruf von &quot;Export Lead/Aktivität/Programmteilnehmer-Job erstellen&quot;in die Warteschlange gestellt und dann durch Aufruf der Endpunkte Export-Lead/Aktivität/Programmteilnehmer-Job in die Warteschlange gestellt werden. Nach der Verstellung in der Warteschlange werden die Aufträge aus der Warteschlange abgerufen und gestartet, sobald Rechenressourcen verfügbar werden.

Die maximale Anzahl von Aufträgen in der Warteschlange beträgt 10. Wenn Sie versuchen, einen Auftrag in die Warteschlange einzureihen, wenn die Warteschlange voll ist, gibt der Endpunkt &quot;Vorgang für die Warteschlange einschließen&quot;den Fehler &quot;1029, Zu viele Aufträge in der Warteschlange&quot;zurück. Es können maximal zwei Aufträge gleichzeitig ausgeführt werden (Status ist &quot;Verarbeitung&quot;).

### Dateigröße

Die Massenextraktions-APIs werden basierend auf der Größe auf der Festplatte der Daten gemessen, die von einem Massenextraktionsauftrag abgerufen werden. Die explizite Größe in Byte für einen Auftrag kann durch Lesen des Attributs `fileSize` aus der abgeschlossenen Statusantwort eines Exportauftrags bestimmt werden.

Das tägliche Kontingent beträgt maximal 500 MB pro Tag, das von Leads, Aktivitäten, Programmmitgliedern und benutzerdefinierten Objekten gemeinsam genutzt wird. Wenn das Kontingent überschritten wird, können Sie keinen weiteren Auftrag erstellen oder in die Warteschlange einreihen, bis das tägliche Kontingent um Mitternacht zurückgesetzt wird [Central Time](https://en.wikipedia.org/wiki/Central_Time_Zone). Bis zu diesem Zeitpunkt wird der Fehler &quot;1029, Export der täglichen Kontingentsmenge überschritten&quot;zurückgegeben. Abgesehen vom täglichen Kontingent gibt es keine maximale Dateigröße.

Sobald ein Auftrag in die Warteschlange gestellt oder verarbeitet wurde, läuft er bis zum Abschluss (mit Ausnahme eines Fehlers oder eines Auftragsabbruchs). Wenn ein Auftrag aus irgendeinem Grund fehlschlägt, müssen Sie ihn neu erstellen. Dateien werden nur dann vollständig geschrieben, wenn ein Auftrag den Status &quot;Abgeschlossen&quot;erreicht (Teildateien werden nie geschrieben). Sie können überprüfen, ob eine Datei vollständig geschrieben wurde, indem Sie den SHA-256-Hash berechnen und diese mit der Prüfsumme vergleichen, die von Auftragsstatus-Endpunkten zurückgegeben wird.

Sie können die für den aktuellen Tag insgesamt verwendete Festplatte ermitteln, indem Sie &quot;Export Lead/Aktivität/Programmteilaufträge abrufen&quot;aufrufen. Diese Endpunkte geben eine Liste aller Aufträge aus den letzten sieben Tagen zurück. Sie können diese Liste nach nur den Aufträgen filtern, die am aktuellen Tag abgeschlossen wurden (mithilfe der Attribute `status` und `finishedAt`). Geben Sie dann die Dateigrößen für diese Aufträge an, um die insgesamt verwendete Menge zu erhalten. Es gibt keine Möglichkeit, eine Datei zu löschen, um Speicherplatz zurückzugewinnen.

## Berechtigungen

Die Massenextraktion verwendet dasselbe Berechtigungsmodell wie die Marketo REST-API und erfordert keine zusätzlichen speziellen Berechtigungen, obwohl für jeden Satz von Endpunkten spezifische Berechtigungen erforderlich sind.

Massenextraktionsaufträge sind nur für den API-Benutzer verfügbar, der sie erstellt hat, einschließlich der Statusabfrage und des Abrufs von Dateiinhalten.

Massenextraktions-Endpunkte sind nicht über Marketo-Arbeitsbereiche informiert. Extrahierungsanfragen enthalten immer Daten für alle Arbeitsbereiche, unabhängig davon, wie Sie den Nur-API-Benutzer für Ihren benutzerdefinierten Dienst definieren.

## Erstellen eines Auftrags

Die Massenextraktions-APIs von Marketo verwenden das Konzept eines Auftrags zum Initiieren und Ausführen der Datenextraktion. Werfen wir einen einfachen Lead-Export-Auftrag vor.

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

Mit dieser einfachen Anfrage wird ein Auftrag erstellt, der die in den Feldern &quot;firstName&quot;und &quot;lastName&quot;enthaltenen Werte zurückgibt, wobei die Spaltenüberschriften &quot;Vorname&quot;und &quot;Nachname&quot;als CSV-Datei vorliegen und jeden zwischen dem 1. Januar 2023 und dem 31. Januar 2023 erstellten Lead enthalten.

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

Wenn wir den Auftrag erstellen, wird eine Auftrags-ID im Attribut `exportId` zurückgegeben. Anschließend können wir diese Auftrags-ID verwenden, um den Auftrag anzureihen, abzubrechen, seinen Status zu überprüfen oder die abgeschlossene Datei abzurufen.

### Allgemeine Parameter

Jeder Endpunkt zur Auftragserstellung gibt einige allgemeine Parameter zum Konfigurieren des Dateiformats, der Feldnamen und des Filters eines Massenextraktionsauftrags frei. Jeder Teiltyp eines Extraktionsauftrags kann zusätzliche Parameter aufweisen:

| Parameter | Datentyp | Hinweise |
|---|---|---|
| format | Zeichenfolge | Bestimmt das Dateiformat der extrahierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und durch Semikolon getrennte Werte. Akzeptiert eines von: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| columnHeaderName | Objekt | Ermöglicht das Festlegen der Namen von Spaltenüberschriften in der zurückgegebenen Datei. Jeder Mitgliederschlüssel ist der Name der Spaltenüberschrift, die umbenannt werden soll, und der Wert ist der neue Name der Spaltenüberschrift. Beispiel: &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| Filter | Objekt | Auf den Extraktionsauftrag angewendeter Filter. Die Typen und Optionen variieren je nach Auftragstyp. |


## Abrufen von Aufträgen

Manchmal müssen Sie Ihre letzten Aufträge abrufen. Dies ist mit dem Befehl &quot;Get Export Jobs&quot;für den entsprechenden Objekttyp einfach zu erledigen. Jeder Get Export Jobs-Endpunkt unterstützt ein `status` -Filterfeld, ein  `batchSize` , um die Anzahl der zurückgegebenen Aufträge zu begrenzen, und `nextPageToken` für das Paging durch große Ergebnismengen. Der Statusfilter unterstützt jeden gültigen Status für einen Exportauftrag: &quot;Erstellt&quot;, &quot;In Warteschlange&quot;, &quot;Verarbeitung&quot;, &quot;Abgebrochen&quot;, &quot;Abgeschlossen&quot;und &quot;Fehlgeschlagen&quot;. Die batchSize hat eine maximale und die Standardeinstellung von 300. Rufen wir die Liste der Lead-Exportaufträge ab:

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

Der Endpunkt antwortet mit der Antwort `status` jedes Auftrags, der in den letzten sieben Tagen für diesen Objekttyp im Ergebnisarray erstellt wurde. Die Antwort enthält nur Ergebnisse für Aufträge, die dem API-Benutzer gehören, der den Aufruf durchführt.

## Auftrag starten

Beginnen wir mit der Auftrags-ID:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Dadurch wird die Ausführung des Auftrags gestartet und eine Statusantwort zurückgegeben. Da der Export immer asynchron erfolgt, müssen wir den Status des Auftrags abfragen, um festzustellen, ob er abgeschlossen wurde. Der Status eines bestimmten Auftrags wird nicht häufiger als einmal alle 60 Sekunden aktualisiert. Daher sollte der Status nie häufiger abgefragt werden. Beachten Sie jedoch, dass die meisten Anwendungsfälle niemals häufiger als einmal alle 5 Minuten eine Abfrage erfordern sollten. Die Daten jedes erfolgreichen Exports werden 10 Tage lang gespeichert.

## Abruf-Auftragsstatus

Die Bestimmung des Auftragsstatus ist einfach.

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

Das innere Element `status` gibt den Fortschritt des Auftrags an und kann einer der folgenden Werte sein: Erstellt, In Warteschlange, Verarbeitung, Abgebrochen, Abgeschlossen, Fehlgeschlagen. In diesem Fall ist unser Auftrag abgeschlossen, sodass wir die Abfrage stoppen und die Datei weiterhin abrufen können. Wenn der Vorgang abgeschlossen ist, gibt das Element `fileSize` die Gesamtlänge der Datei in Byte an und das Element `fileChecksum` enthält den SHA-256-Hash der Datei. Der Auftragsstatus ist 30 Tage lang verfügbar, nachdem der Status Abgeschlossen oder Fehlgeschlagen erreicht wurde.

## Abrufen Ihrer Daten

Nach Abschluss des Auftrags können Sie die Datei einfach abrufen.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

Die Antwort enthält eine Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der Datei. Wenn ein Auftrag nicht abgeschlossen oder eine ungültige Auftrags-ID übergeben wurde, antworten Dateiendpunkte mit dem Status &quot;404 Nicht gefunden&quot;und einer Fehlermeldung als Payload (im Gegensatz zu den meisten anderen Marketo REST-Endpunkten).

Um das teilweise und wiederverwendbare Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional den HTTP-Header `Range` des Typs `bytes` (per [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Wenn der Header nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Um die ersten 10.000 Byte einer GET abzurufen, übergeben Sie die folgende Kopfzeile als Teil Ihrer -Anfrage an den -Endpunkt, beginnend mit Byte 0:

```
Range: bytes=0-9999
```

Beim Abrufen der partiellen Datei antwortet der Endpunkt mit Status-Code 206 und gibt die Accept-range-, Content-Length- und Content-Range-Header zurück:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Teilabruf und -wiederaufnahme

Die Dateien können teilweise abgerufen oder später mit der Kopfzeile `Range` fortgesetzt werden. Der Bereich für eine Datei beginnt bei Byte 0 und endet beim Wert von `fileSize` minus 1. Die Länge der Datei wird auch als Nenner im Wert des Antwortheaders `Content-Range` beim Aufruf eines Endpunkts &quot;Get Export File&quot;berichtet. Wenn ein Abruf teilweise fehlschlägt, kann er später wieder aufgenommen werden. Wenn Sie z. B. versuchen, eine Datei mit einer Länge von 1000 Byte abzurufen, aber nur die ersten 725 Byte empfangen wurden, kann der Abruf ab dem Zeitpunkt des Fehlschlagens wiederholt werden, indem der Endpunkt erneut aufgerufen und ein neuer Bereich übergeben wird:

```
Range: bytes 724-999
```

Dadurch werden die verbleibenden 275 Byte der Datei zurückgegeben.

#### Überprüfung der Dateiintegrität

Die Auftragsstatus-Endpunkte geben eine Prüfsumme im Attribut `fileChecksum` zurück, wenn `status` &quot;Abgeschlossen&quot;ist. Die Prüfsumme ist ein SHA-256-Hash der exportierten Datei. Sie können die Prüfsumme mit dem SHA-256-Hash der abgerufenen Datei vergleichen, um sicherzustellen, dass sie vollständig ist.

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

Im Folgenden finden Sie ein Beispiel für die Erstellung des SHA-256-Hash einer abgerufenen Datei namens &quot;bulk_lead_export.csv&quot;mit dem Befehlszeilen-Dienstprogramm sha256sum :

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Abbruch eines Auftrags

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

Diese Antwort erhält einen Status, der angibt, dass der Auftrag abgebrochen wurde.
