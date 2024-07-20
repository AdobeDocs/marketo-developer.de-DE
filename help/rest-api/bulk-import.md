---
title: Massenimport
feature: REST API
description: Batch-Import von Personendaten.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 2%

---

# Massenimport

Marketo bietet Schnittstellen zum Einfügen großer Mengen von personenbezogenen Daten, die als Massenimport bezeichnet werden. Derzeit sind Schnittstellen für drei Objekttypen verfügbar:

- Leads (Personen)
- Benutzerdefinierte Objekte
- Programm-Mitglieder

Der Massenimport erfolgt durch Erstellen eines Auftrags und anschließendes Warten auf den Abschluss des Vorgangs beim Lesen einer Datei. Diese Aufträge werden asynchron ausgeführt und können abgerufen werden, um den Status des Imports abzurufen. Dateien werden mit HTTP multipart/form-data per RFC 2399 hochgeladen.

Massen-API-Endpunkte erhalten wie andere Endpunkte nicht das Präfix &quot;/rest&quot;.

## Authentifizierung

Die Bulk-Import-APIs verwenden dieselbe OAuth 2.0-Authentifizierungsmethode wie andere Marketo REST-APIs.  Dazu muss ein gültiges Zugriffstoken entweder als Abfragezeichenfolgenparameter `access_token={_AccessToken_}` oder als HTTP-Header `Authorization: Bearer {_AccessToken_}` eingebettet werden.

## Beschränkungen

- Max. gleichzeitige Importvorgänge: 2
- Max. in die Warteschlange gestellte Importaufträge (einschließlich aktueller Importaufträge): 10
- Maximale Größe der Importdatei: 10 MB

## Berechtigungen

Der Massenimport verwendet dasselbe Berechtigungsmodell wie die Marketo REST-API und erfordert keine zusätzlichen speziellen Berechtigungen, um verwendet werden zu können. Für jeden Satz von Endpunkten sind jedoch spezifische Berechtigungen erforderlich.

## Vorgänge für Datensätze

Der Massenimport ist ein Vorgang zum Einfügen oder Aktualisieren von Datensätzen. Wenn ein übereinstimmender Datensatz in der Datenbank gefunden wird, wird er aktualisiert. Andernfalls wird ein neuer Datensatz erstellt. Die Antwort auf den Massenimport gibt nicht an, ob ein bestimmter Datensatz aktualisiert oder eingefügt wurde.

## Erstellen eines Auftrags

Die Bulk-Import-APIs von Marketo verwenden das Konzept eines Auftrags zum Ausführen des Datenimports. Im Folgenden wird die Erstellung eines einfachen Lead-Importvorgangs mit dem Endpunkt [Leads importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) beschrieben.  Beachten Sie, dass dieser Endpunkt [multipart/form-data als Inhaltstyp](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html) verwendet. Dies kann schwierig zu erreichen sein. Daher empfiehlt es sich, eine HTTP-Support-Bibliothek für Ihre Sprache zu verwenden.  Wenn Sie nur Ihre Füße nass machen, empfehlen wir Ihnen, [curl](https://curl.se/) zu verwenden.

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

Mit dieser Anfrage wird ein Auftrag erstellt, der die in der CSV-Datei enthaltenen Werte namens &quot;Leads.csv&quot;mit den Spaltenüberschriften &quot;Vorname&quot;, &quot;Nachname&quot;, &quot;E-Mail&quot;, &quot;Unternehmen&quot;importiert.

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

Wenn wir den Auftrag senden, wird eine batchId zurückgegeben, mit der wir dann seinen Status überprüfen können.

### Allgemeine Parameter

Jeder Endpunkt zur Auftragserstellung gibt einige allgemeine Parameter zum Konfigurieren des Dateiformats, der Feldnamen und des Filters eines Massenextraktionsauftrags frei.  Jeder Teiltyp eines Extraktionsauftrags kann zusätzliche Parameter aufweisen:

| Parameter | Datentyp | Hinweise |
|---|---|---|
| format | Zeichenfolge | Bestimmt das Dateiformat der importierten Daten mit Optionen für kommagetrennte Werte, tabulatorgetrennte Werte und durch Semikolon getrennte Werte. Akzeptiert eines von: CSV, SSV, TSV. Das Format ist standardmäßig CSV. |
| Datei | Zeichenfolge | Daten werden durch mehrteilige Formulardaten in der Datei angegeben. |


## Abruf-Auftragsstatus

Die Bestimmung des Status des Auftrags ist einfach mithilfe des Endpunkts [Import-Lead-Status abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) .

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

Das innere `status` -Element gibt den Fortschritt des Auftrags an und kann einer der folgenden Werte sein: In Warteschlange gestellt, Importiert, Abgeschlossen, Fehlgeschlagen. In diesem Fall ist unsere Arbeit abgeschlossen, sodass wir mit der Abstimmung aufhören können.

## Fehler

Fehler werden durch das Attribut `numOfRowsFailed` in der Antwort &quot;Import Lead Status&quot;angezeigt. Wenn `numOfRowsFailed` größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Fehler an.

Um die Datensätze und Ursachen für fehlgeschlagene Zeilen abzurufen, müssen Sie die Fehlerdatei mit dem Endpunkt [Import-Lead-Fehler abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET) abrufen.

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Die Datei gibt an, welche Zeilen fehlgeschlagen sind, sowie eine Meldung, die angibt, warum der Datensatz fehlgeschlagen ist.
