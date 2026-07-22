---
title: Massenimport von Programmmitgliedern
feature: REST API
description: Erfahren Sie, wie Sie Programmmitglieder per Marketo-REST-API mithilfe von CSV-TSV- oder SSV-Dateien unter 10 MB massenhaft importieren können, Warteschlangenbeschränkungen, erforderliche Parameter und den Status des Abrufauftrags.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
TQID: https://experienceleague.adobe.com/T1PAzLN1mnp38kJ0jwh6kPv6r1Uvxc7-o9zeTHetIV0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 771
ht-degree: 0%

---

# Massenimport von Programmmitgliedern

[Referenz zum Massenimport-Endpunkt des Programmmitglieds](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members)

Verwenden Sie die [Bulk-API](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members) um eine große Anzahl von Programmteilnehmer-Datensätzen asynchron zu importieren. Stellen Sie die Datensätze in einer flachen Datei mit Komma, Tabulatoren oder Semikolons bereit, die kleiner als 10 MB ist.

Der Massenimport von Programmmitgliedern unterstützt nur den Datensatzvorgang „Einfügen oder aktualisieren“.

## Verarbeitungsbeschränkungen

Jede Massenimportanfrage wird als Auftrag zu einer FIFO-Warteschlange (First-In, First-Out) hinzugefügt. Es gelten die folgenden Grenzwerte:

- Es können maximal zwei Aufträge gleichzeitig verarbeitet werden.
- Es können maximal 10 Aufträge in der Warteschlange sein, einschließlich der beiden verarbeiteten Aufträge.

Wenn Sie das Maximum von 10 Aufträgen überschreiten, gibt die API einen `1016, Too many imports` Fehler zurück.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, die die REST-API-Feldnamen auflistet, denen die Werte in den einzelnen Zeilen zugeordnet sind. Rufen Sie diese Namen mithilfe der Endpunkte [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) und [Programmmitglied beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeProgramMemberUsingGET) ab.

Datensätze können Lead-Felder, benutzerdefinierte Lead-Felder und benutzerdefinierte Programmmitgliedsfelder enthalten.

Eine typische Datei folgt diesem Muster:

```text
email,firstName,lastName
test@example.com,John,Doe
```

Senden Sie die Anfrage mit dem `multipart/form-data` Inhaltstyp. Verwenden Sie eine vorhandene Bibliotheksimplementierung, um die mehrteilige Anfrage zu erstellen.

## Erstellen von Aufträgen

Der Endpunkt [Programmmitglieder importieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) liest Programmmitglieder-Datensätze aus einer Datei und fügt sie einem Programm mit dem angegebenen Status hinzu. Datensätze können Lead-Felder und benutzerdefinierte Programmmitgliedsfelder enthalten.

Jeder Datensatz muss das E-Mail-Feld enthalten, das für die Deduplizierung verwendet wird.

Der `programId` Pfadparameter gibt das Programm an, dem die Member hinzugefügt werden.

Die Anfrage erfordert drei Abfrageparameter:

- `format`: Das Format der Importdatei (`CSV`, `TSV` oder `SSV`).
- `programMemberStatus`: Der den importierten Mitgliedern zugewiesene Programmstatus.
- `file`: Der Name der Datei, die die Einträge der Programmmitglieder enthält.

```http
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```text
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```text
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Da der Endpunkt asynchron ist, enthält die Antwort `batchId`- und `status`. Der Status kann `Queued`, `Importing` oder `Failed` sein.

Behalten Sie die `batchId` bei, um den Importstatus zu überprüfen und Fehler oder Warnungen nach Abschluss abzurufen. Die `batchId` bleibt sieben Tage gültig.

Mit der folgenden Befehlszeilen-cURL-Anfrage wird der Beispielvorgang gesendet:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

In diesem Beispiel enthält die `Lead-House-Lannister.csv` Importdatei die folgenden Daten:

```text
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Status des Abrufauftrags

Nachdem Sie den Importauftrag erstellt haben, fragen Sie ihn alle 5-30 Sekunden ab. Übergeben Sie den `batchId` Pfadparameter an den Endpunkt [Abrufen des Status von &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET)-Abonnenten des Importprogramms“.

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

Diese Antwort zeigt einen abgeschlossenen Import an. Der Status kann `Complete`, `Queued`, `Importing` oder `Failed` sein.

Wenn der Auftrag abgeschlossen ist, listet die Antwort die Anzahl der verarbeiteten Zeilen auf, die fehlgeschlagen sind und mit Warnungen verarbeitet wurden. Der `message` kann auch eine Fehlermeldung bereitstellen, wenn der Status `Failed` ist.

## Fehler

Das Attribut `numOfRowsFailed` in der Antwort [Abrufen des Status des &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET)-Mitglieds des Importprogramms“ gibt die Anzahl der fehlgeschlagenen Zeilen an. Ein Wert größer als null bedeutet, dass Fehler aufgetreten sind.

Übergeben Sie den `batchId` Pfadparameter an den Endpunkt Abrufen von fehlgeschlagenen Datensätzen zu Importierprogrammmitgliederfehlern , um die Datensätze und ihre Ursachen abzurufen.

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Der Endpunkt gibt eine Datei zurück, die jede fehlgeschlagene Zeile identifiziert und erläutert, warum der Datensatz fehlgeschlagen ist. Die Datei verwendet das Format, das durch den Parameter &quot;`format`&quot; bei der Auftragserstellung angegeben wird. Ein zusätzliches Feld in jedem Datensatz beschreibt den Fehler.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen Lead-Bewertung:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Der Auftragsstatus gibt `numOfRowsFailed` als 1 zurück und zeigt an, dass ein Fehler aufgetreten ist:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

Abrufen der Fehlerdatei für weitere Informationen:

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Warnungen

Das Attribut `numOfRowsWithWarning` in der Antwort [Abrufen des Status des &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET)-Mitglieds des Importprogramms“ gibt die Anzahl der Zeilen mit Warnungen an. Ein Wert größer als null bedeutet, dass Warnungen aufgetreten sind.

Übergeben Sie den `batchId` Pfadparameter an den Endpunkt [Warnungen zum Abruf der Programmteilnehmer](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET), um die betroffenen Datensätze und ihre Ursachen abzurufen.

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Der Endpunkt gibt eine Datei zurück, die jede Zeile mit einer Warnung identifiziert und erläutert, warum die Warnung aufgetreten ist. Die Datei verwendet das Format, das durch den Parameter &quot;`format`&quot; bei der Auftragserstellung angegeben wird. Ein zusätzliches Feld in jedem Datensatz beschreibt die Warnung.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen E-Mail-Adresse:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Der Auftragsstatus gibt `numOfRowsWithWarning` als 1 zurück und zeigt an, dass eine Warnung aufgetreten ist:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

Rufen Sie die Warndatei für weitere Informationen ab:

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
