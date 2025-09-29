---
title: Massenimport
feature: REST API
description: Massenimport von Marketo zum Laden von Leads, benutzerdefinierten Objekten und Programmmitgliedern über mehrteilige Uploads, Erstellen asynchroner Aufträge, Abfragestatus und die Verarbeitung von Fehlern.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---

# Massenimport

Marketo bietet Schnittstellen zum Einfügen großer Mengen an personenbezogenen Daten, den so genannten Massenimport. Derzeit werden Schnittstellen für drei Objekttypen angeboten:

- Leads (Personen)
- Benutzerdefinierte Objekte
- Programm-Mitglieder

Der Massenimport wird ausgeführt, indem ein Auftrag erstellt wird und dann darauf gewartet wird, dass der Auftrag das Lesen einer Datei abschließt. Diese Aufträge werden asynchron ausgeführt und können abgerufen werden, um den Status des Imports abzurufen. Dateien werden mit HTTP Multipart/form-data per RFC 2399 hochgeladen.

Bulk-API-Endpunkte haben nicht das Präfix &quot;/rest“ wie andere Endpunkte.

## Authentifizierung

Die Massenimport-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo-REST-APIs.  Dies erfordert ein gültiges Zugriffstoken, das als HTTP-Header-`Authorization: Bearer {_AccessToken_}` gesendet wird.

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 30. Juni 2025 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

## Beschränkungen

- Max. gleichzeitige Importvorgänge: 2
- Max. Anzahl an Importvorgängen in der Warteschlange (einschließlich aktuell importierender Vorgänge): 10
- Maximale Größe der Importdatei: 10 MB

## Berechtigungen

Der Massenimport verwendet dasselbe Berechtigungsmodell wie die Marketo-REST-API und erfordert keine zusätzlichen speziellen Berechtigungen, um verwenden zu können, obwohl spezifische Berechtigungen für jeden Satz von Endpunkten erforderlich sind.

## Datensatzvorgänge

Der Massenimport ist ein Datensatzvorgang vom Typ „Einfügen oder Aktualisieren“. Wenn ein übereinstimmender Datensatz in der Datenbank gefunden wird, wird er aktualisiert. Andernfalls wird ein neuer Datensatz erstellt. Die Antwort zum Massenimport gibt nicht an, ob ein bestimmter Datensatz aktualisiert oder eingefügt wurde.

## Erstellen von Aufträgen

Die Massenimport-APIs von Marketo verwenden das Konzept eines Auftrags zum Ausführen des Datenimports. Im Folgenden wird das Erstellen eines einfachen Lead-Importvorgangs mit dem Endpunkt [Leads importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) beschrieben.  Beachten Sie, dass dieser Endpunkt [multipart/form-data als Inhaltstyp“ ](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Dies kann schwierig sein, sodass die Best Practice darin besteht, eine HTTP-Support-Bibliothek für die Sprache Ihrer Wahl zu verwenden.  Wenn Sie nur nasse Füße bekommen, empfehlen wir, dass Sie [cURL](https://curl.se/) verwenden.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Mit dieser Anfrage wird ein Vorgang erstellt, der in der CSV-Datei enthaltene Werte namens „lead.csv“ mit den Spaltenüberschriften „FirstName“, „LastName“, „Email“, „Company“ importiert.

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Wenn wir den Auftrag übermitteln, wird eine batchId zurückgegeben, mit der wir dann den Status überprüfen können.

### Allgemeine Parameter

Jeder Auftragserstellungsendpunkt verwendet einige allgemeine Parameter zum Konfigurieren des Dateiformats, der Feldnamen und des Filters eines Massenextraktionsauftrags.  Jeder Subtyp des Extraktionsvorgangs kann über zusätzliche Parameter verfügen:

| Parameter | Datentyp | Hinweise |
|---|---|---|
| Format | String | Bestimmt das Dateiformat der importierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und Semikolon-getrennte Werte. Akzeptiert eine der folgenden Optionen: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| Datei | String | Daten werden durch mehrteilige Formulardaten in der Datei angegeben. |

## Status des Abrufauftrags

Die Bestimmung des Status des Auftrags ist einfach mithilfe des Endpunkts [Lead-Status abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Das innere `status` gibt den Fortschritt des Auftrags an und kann einen der folgenden Werte aufweisen: „In Warteschlange“, „Importieren“, „Abgeschlossen“ oder „Fehlgeschlagen“. In diesem Fall ist unser Vorgang abgeschlossen, sodass wir den Abruf stoppen können.

## Fehler

Fehler werden durch das Attribut `numOfRowsFailed` in der Antwort zum Abrufen des Lead-Importstatus angezeigt. Wenn `numOfRowsFailed` größer als null ist, gibt dieser Wert die Anzahl der aufgetretenen Fehler an.

Um die Datensätze und Ursachen fehlgeschlagener Zeilen abzurufen, müssen Sie die Fehlerdatei mit dem Endpunkt [Abrufen von Lead-](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET)) abrufen.

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Die Datei gibt an, welche Zeilen fehlgeschlagen sind, zusammen mit einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist.
