---
title: Massenimport
feature: REST API
description: Massenimport von Marketo zum Laden von Leads, benutzerdefinierten Objekten und Programmmitgliedern über mehrteilige Uploads, Erstellen asynchroner Aufträge, Abfragestatus und die Verarbeitung von Fehlern.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
TQID: https://experienceleague.adobe.com/lr9dyX-fY-oJ2LM5P0zE1m24HtFYKQYYbxMkVe--PkE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 538
ht-degree: 3%

---

# Massenimport

Der Massenimport bietet Schnittstellen zum Einfügen großer Mengen von Personen- und personenbezogenen Daten. Sie können drei Objekttypen importieren:

- Leads (Personen)
- Benutzerdefinierte Objekte
- Programm-Mitglieder

Um einen Massenimport durchzuführen, erstellen Sie einen Auftrag, der eine hochgeladene Datei liest. Der Auftrag wird asynchron ausgeführt. Fragen Sie ihn daher ab, um den Importstatus abzurufen.

Hochladen von Dateien mit HTTP-`multipart/form-data` gemäß RFC 2399.

Im Gegensatz zu anderen Endpunkten sind Bulk-API-Endpunkte nicht mit dem Präfix `/rest` versehen.

## Authentifizierung

Die Massenimport-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo-REST-APIs. Senden Sie ein gültiges Zugriffstoken im `Authorization: Bearer {_AccessToken_}` HTTP-Header.

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 30. Juni 2025 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

## Beschränkungen

- Maximale Anzahl gleichzeitiger Importvorgänge: 2
- Maximale Anzahl an Importvorgängen in der Warteschlange, einschließlich aktuell importierender Vorgänge: 10
- Maximale Größe der Importdatei: 10 MB

## Berechtigungen

Der Massenimport verwendet dasselbe Berechtigungsmodell wie die Marketo-REST-API. Es sind keine zusätzlichen Berechtigungen erforderlich, aber für jeden Endpunktsatz sind spezifische Berechtigungen erforderlich.

## Datensatzvorgänge

Der Massenimport ist ein Datensatzvorgang vom Typ „Einfügen oder Aktualisieren“. Wenn die Datenbank einen übereinstimmenden Datensatz enthält, wird er durch den Vorgang aktualisiert. Andernfalls wird ein Datensatz erstellt.

Die Massenimportantwort gibt nicht an, ob ein einzelner Datensatz aktualisiert oder eingefügt wurde.

## Erstellen von Aufträgen

Erstellen Sie einen Lead-Importvorgang durch Aufruf des Endpunkts [Leads importieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Dieser Endpunkt verwendet [multipart/form-data als Inhaltstyp](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html).

Verwenden Sie eine HTTP-Support-Bibliothek für Ihre bevorzugte Sprache, um die mehrteilige Anfrage zu erstellen. Sie können auch [curl](https://curl.se/) verwenden, um zu beginnen.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Diese Anfrage erstellt einen Auftrag, der Werte aus der CSV-Datei mit dem Namen `leads.csv` importiert.

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

Die Antwort gibt ein -`batchId` zurück. Verwenden Sie diesen Wert, um den Auftragsstatus zu überprüfen.

### Allgemeine Parameter

Jeder Vorgangserstellungsendpunkt verwendet die gleichen Parameter zur Konfiguration der Importdatei. Ein Importuntertyp kann auch zusätzliche Parameter unterstützen.

| Parameter | Datentyp | Hinweise |
| --- | --- | --- |
| Format | String | Bestimmt das Dateiformat der importierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und Semikolon-getrennte Werte. Akzeptiert eine der folgenden Optionen: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| Datei | String | Daten werden durch mehrteilige Formulardaten in der Datei angegeben. |

## Status des Abrufauftrags

Übergeben Sie die `batchId` an den Endpunkt [Lead-Status abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) um den Auftragsstatus abzurufen.

```http
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

Das `status` gibt den Fortschritt des Vorgangs an. Sein Wert kann `Queued`, `Importing`, `Complete` oder `Failed` sein.

In diesem Beispiel ist der Vorgang abgeschlossen, sodass das Abrufen beendet werden kann.

## Fehler

Das Attribut `numOfRowsFailed` in der Antwort Lead-Status abrufen gibt die Anzahl der fehlgeschlagenen Zeilen an. Ein Wert größer als null bedeutet, dass Fehler aufgetreten sind.

Um die fehlgeschlagenen Datensätze und ihre Ursachen abzurufen, verwenden Sie den Endpunkt [Abrufen von Lead-](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET)).

```http
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Die Fehlerdatei identifiziert jede fehlgeschlagene Zeile und erklärt, warum der Datensatz fehlgeschlagen ist.
