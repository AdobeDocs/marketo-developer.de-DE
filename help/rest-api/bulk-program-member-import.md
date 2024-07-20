---
title: Importieren von Mitgliedern des Bulk-Programms
feature: REST API
description: Batch-Import von Mitgliederdaten.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---

# Importieren von Mitgliedern des Bulk-Programms

[Referenz zum Importieren von Programmteilnehmern für Massenprogramme](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Bei großen Mengen von Programmmitgliedern können Programmmitglieder asynchron mit der [Bulk API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members) importiert werden. Auf diese Weise können Sie eine Datensatzliste mit einer flachen Datei mit den Trennzeichen (Komma, Tabulator oder Semikolon) in Marketo importieren. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, solange die Datei insgesamt weniger als 10 MB groß ist. Der Datensatzvorgang ist nur &quot;insert or update&quot;.

## Verarbeitungsbeschränkungen

Sie können mehr als eine Massen-Importanfrage mit Einschränkungen senden. Jede Anforderung wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Maximal zehn Aufträge sind jederzeit in der Warteschlange zulässig (einschließlich der beiden derzeit verarbeiteten Aufträge). Wenn Sie die maximale Anzahl von zehn Aufträgen überschreiten, wird der Fehler &quot;1016, Zu viele Importe&quot;zurückgegeben.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, in der die entsprechenden REST-API-Namen als Felder aufgeführt werden, in denen die Werte jeder Zeile zugeordnet werden. REST-API-Namen können mit den Endpunkten [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) und/oder [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET) abgerufen werden. Datensätze können Lead-Felder, benutzerdefinierte Lead-Felder und benutzerdefinierte Felder für Programmmitglieder enthalten.

Eine typische Datei würde diesem grundlegenden Muster folgen:

```
email,firstName,lastName
test@example.com,John,Doe
```

Der Aufruf selbst erfolgt über den Inhaltstyp `multipart/form-data` .

Dieser Anfragetyp kann schwierig zu implementieren sein. Daher wird dringend empfohlen, eine vorhandene Bibliotheksimplementierung zu verwenden.

## Erstellen eines Auftrags

Der Endpunkt [Programmmitglieder importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) liest eine Datei mit Programmmitgliedern-Datensätzen und fügt sie einem Programm mit einem bestimmten Status hinzu. Die Datensätze können sowohl Lead-Felder als auch benutzerdefinierte Felder für Programmmitglieder enthalten. Alle Datensätze müssen das E-Mail-Feld enthalten, das zur Deduplizierung verwendet wird.

Der Pfadparameter `programId` gibt das Programm an, dem die Mitglieder hinzugefügt werden.

Es gibt drei erforderliche Abfrageparameter. Der Parameter `format` gibt das Importdateiformat (CSV, TSV oder SSV), den Parameter `programMemberStatus` den Programmstatus für die Mitglieder an, die dem Programm hinzugefügt werden, und den Parameter `file` enthält den Namen der Importdatei, die die Datensätze der Programmmitglieder enthält.

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

Beachten Sie in der Antwort auf unseren Aufruf, dass es ein `batchId` - und ein `status` -Feld für den Datensatz im Ergebnis-Array gibt. Da dieser Endpunkt asynchron ist, kann er den Status &quot;In Warteschlange&quot;, &quot;Importieren&quot;oder &quot;Fehlgeschlagen&quot;zurückgeben. Sie müssen die `batchId` beibehalten, um den Status des Importvorgangs zu erhalten und Fehler und/oder Warnungen nach Abschluss abzurufen. Die `batchId` bleibt sieben Tage lang gültig.

Im obigen Beispiel besteht eine einfache Möglichkeit, den Endpunkt aufzurufen, in der Verwendung von cURL über die Befehlszeile:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Wenn die Importdatei &quot;Lead-House-Lannister.csv&quot;Folgendes enthält:

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

## Abruf-Auftragsstatus

Nachdem der Importauftrag erstellt wurde, müssen Sie dessen Status abfragen. Es empfiehlt sich, den Importauftrag alle 5-30 Sekunden abzufragen. Übergeben Sie dazu den Pfadparameter `batchId` an den Endpunkt [Status der Programmteilnehmer importieren ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) .

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

Diese Antwort zeigt einen abgeschlossenen Import. Der Status kann wie folgt lauten: Abgeschlossen, In Warteschlange, Importieren, Fehlgeschlagen.

Wenn der Auftrag abgeschlossen wurde, wird die Anzahl der verarbeiteten, fehlgeschlagenen oder mit Warnungen versehenen Zeilen aufgelistet. Der Nachrichtenparameter gibt möglicherweise auch die Fehlermeldung aus, wenn der Status Fehlgeschlagen lautet.

## Fehler

Fehler werden durch das Attribut `numOfRowsFailed` in der Antwort [Status der Programmteilnehmer importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) angezeigt. Wenn numOfRowsFailed größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Fehler an.

Verwenden Sie den Endpunkt [Import Program Member Failures abrufen](http://TODO) , um Datensätze und Ursachen für fehlgeschlagene Zeilen abzurufen, indem Sie den Pfadparameter `batchId` übergeben.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Der Endpunkt antwortet mit einer Datei, die angibt, welche Zeilen fehlgeschlagen sind, sowie einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem, das beim Erstellen des Auftrags im Parameter `format` angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Fehlerbeschreibung angehängt.

Angenommen, Sie importieren die folgende Datei mit einem ungültigen Lead-Ergebnis:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie, dass `numOfRowsFailed` 1 ist, was auf einen Fehler hinweist:

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

Rufen Sie dann die Datei mit den Fehlern ab, um weitere Details zum Fehler zu erhalten:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Warnungen

Warnungen werden durch das Attribut `numOfRowsWithWarning` in der Antwort [Status der Programmteilnehmer importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) angezeigt. Wenn `numOfRowsWithWarning` größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Warnungen an.

Verwenden Sie den Endpunkt [Warnungen von Programmmitgliedern abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) , um Datensätze und Ursachen für Warnzeilen abzurufen, indem Sie den Parameter `batchId` path übergeben.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Der Endpunkt antwortet mit einer Datei, die angibt, welche Zeilen Warnungen erzeugt haben, sowie einer Meldung, die angibt, warum der Datensatz eine Warnung erzeugt hat. Das Format der Datei entspricht dem, das beim Erstellen des Auftrags im Parameter `format` angegeben wurde. Jedem Datensatz wird ein zusätzliches Feld mit einer Beschreibung der Warnung angehängt.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen E-Mail-Adresse:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie &quot;`numOfRowsWithWarning` ist 1&quot;, was anzeigt, dass eine Warnung aufgetreten ist:

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

Anschließend rufen Sie die Warnungsdatei ab, um weitere Details zur Warnung zu erhalten:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
