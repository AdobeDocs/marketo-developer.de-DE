---
title: Massenimport von Programmmitgliedern
feature: REST API
description: Erfahren Sie, wie Sie Programmmitglieder per Marketo-REST-API mithilfe von CSV-TSV- oder SSV-Dateien unter 10 MB massenhaft importieren können, Warteschlangenbeschränkungen, erforderliche Parameter und den Status des Abrufauftrags.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Massenimport von Programmmitgliedern

[Referenz zum Massenprogramm-Member-Importendpunkt](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Bei großen Mengen an Programmmitgliedsdatensätzen können Programmmitglieder asynchron mit der [Bulk-API“ importiert &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Auf diese Weise können Sie eine Liste von Datensätzen mithilfe einer flachen Datei mit den Trennzeichen (Komma, Tabulator oder Semikolon) in Marketo importieren. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, sofern die Datei weniger als 10 MB groß ist. Der Datensatzvorgang ist nur „Einfügen oder Aktualisieren“.

## Verarbeitungsbeschränkungen

Es ist zulässig, mehr als eine Massenimportanfrage zu senden, mit Einschränkungen. Jede Anfrage wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Es sind maximal zehn Aufträge gleichzeitig in der Warteschlange zulässig (einschließlich der zwei derzeit verarbeiteten). Wenn Sie den Maximalwert von zehn Aufträgen überschreiten, wird der Fehler „1016, Zu viele Importe“ zurückgegeben.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, die die entsprechenden REST-API-Namen als Felder auflistet, denen die Werte jeder Zeile zugeordnet werden sollen. REST-API-Namen können mit den Endpunkten [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) und/oder [Programmmitglied beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET) abgerufen werden. Datensätze können Lead-Felder, benutzerdefinierte Lead-Felder und benutzerdefinierte Programmmitgliedsfelder enthalten.

Eine typische Datei würde diesem grundlegenden Muster folgen:

```
email,firstName,lastName
test@example.com,John,Doe
```

Der Aufruf selbst erfolgt über den Inhaltstyp `multipart/form-data`.

Dieser Anfragetyp kann schwierig zu implementieren sein. Daher wird dringend empfohlen, eine vorhandene Bibliotheksimplementierung zu verwenden.

## Erstellen von Aufträgen

Der Endpunkt [Programmmitglieder importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) liest eine Datei mit Programmmitgliedsdatensätzen und fügt sie einem Programm mit einem bestimmten Status hinzu. Die Datensätze können sowohl Lead-Felder als auch benutzerdefinierte Felder für Programmmitglieder enthalten. Alle Datensätze müssen das E-Mail-Feld enthalten, das für Deduplizierungszwecke verwendet wird.

Der `programId` Pfadparameter gibt das Programm an, dem die Member hinzugefügt werden.

Es gibt drei erforderliche Abfrageparameter. Der Parameter `format` gibt das Format der Importdatei (CSV, TSV oder SSV) an, der Parameter `programMemberStatus` gibt den Programmstatus für die Mitglieder an, die dem Programm hinzugefügt werden sollen, und der Parameter `file` enthält den Namen der Importdatei, die die Einträge der Programmmitglieder enthält.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
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

Beachten Sie in der Antwort auf unseren Aufruf, dass es ein `batchId` und ein `status` Feld für den Datensatz im Ergebnis-Array gibt. Da dieser Endpunkt asynchron ist, kann er den Status „In Warteschlange“, „Importieren“ oder „Fehlgeschlagen“ zurückgeben. Sie müssen die `batchId` beibehalten, um den Status des Importvorgangs abzurufen und Fehler und/oder Warnungen nach Abschluss des Vorgangs abzurufen. Die `batchId` bleibt sieben Tage gültig.

Im obigen Beispiel besteht eine einfache Möglichkeit, den Endpunkt aufzurufen, darin, cURL über die Befehlszeile zu verwenden:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Die Importdatei „Lead-House-Lannister.csv“ enthält Folgendes:

```
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

Nachdem der Importvorgang erstellt wurde, müssen Sie seinen Status abfragen. Es empfiehlt sich, den Importauftrag alle 5 bis 30 Sekunden abzufragen. Übergeben Sie dazu den `batchId`-Pfadparameter an den Endpunkt [Abrufen des Mitgliedsstatus des Importprogramms](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```
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

Diese Antwort zeigt einen abgeschlossenen Import an. Der Status kann einer der folgenden sein: Abgeschlossen, In Warteschlange, Importieren, Fehlgeschlagen.

Wenn der Vorgang abgeschlossen ist, wird eine Liste der verarbeiteten Zeilen, der fehlgeschlagenen Zeilen oder der Warnungen angezeigt. Der Nachrichtenparameter kann auch die Fehlermeldung ausgeben, wenn der Status Fehlgeschlagen ist.

## Fehler

Fehler werden durch das Attribut `numOfRowsFailed` in der Antwort [Abrufen des Mitgliedsstatus &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) Importprogramms“ angezeigt. Wenn numOfRowsFailed größer als null ist, gibt dieser Wert die Anzahl der aufgetretenen Fehler an.

Verwenden Sie den Endpunkt Abrufen von Fehlern bei Programmmitgliedern des Importprogramms , um Datensätze und Ursachen fehlgeschlagener Zeilen abzurufen, indem Sie den `batchId` Pfadparameter übergeben.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Der Endpunkt antwortet mit einer Datei, die angibt, welche Zeilen fehlgeschlagen sind, sowie mit einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem Format, das `format` Parameter bei der Auftragserstellung angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Beschreibung des Fehlers angehängt.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen Lead-Bewertung:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie, dass `numOfRowsFailed` 1 ist, was anzeigt, dass ein Fehler aufgetreten ist:

```
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

Rufen Sie dann die Fehlerdatei ab, um weitere Details zum Fehler zu erhalten:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Warnungen

Warnungen werden durch das Attribut `numOfRowsWithWarning` in der Antwort [Abrufen des Mitgliedsstatus &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) Importprogramms“ angezeigt. Wenn `numOfRowsWithWarning` größer als null ist, gibt dieser Wert die Anzahl der aufgetretenen Warnungen an.

Verwenden Sie den Endpunkt [Abrufen von Warnhinweisen zu &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET)Abonnenten des Importprogramms“, um Datensätze und Ursachen von Warnzeilen abzurufen, indem Sie den `batchId` Pfadparameter übergeben.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Der Endpunkt antwortet mit einer -Datei, die angibt, welche Zeilen Warnungen erzeugt haben, zusammen mit einer Meldung, die angibt, warum der Datensatz eine Warnung erzeugt hat. Das Format der Datei entspricht dem Format, das `format` Parameter bei der Auftragserstellung angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Beschreibung der Warnung angehängt.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen E-Mail-Adresse:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie, dass `numOfRowsWithWarning` 1 ist, was anzeigt, dass eine Warnung aufgetreten ist:

```
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

Anschließend rufen Sie die Warndatei ab, um weitere Details zur Warnung zu erhalten:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
