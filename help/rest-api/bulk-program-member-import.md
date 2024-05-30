---
title: Importieren von Mitgliedern des Bulk-Programms
feature: REST API
description: "Batch-Import von Mitgliedsdaten."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---


# Importieren von Mitgliedern des Bulk-Programms

[Referenz zum Importzeitpunkt eines Massenprogramms für Mitglieder](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Bei großen Mengen von Programmmitgliedern können Programmmitglieder asynchron mit der [Bulk-API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Auf diese Weise können Sie eine Datensatzliste mit einer flachen Datei mit den Trennzeichen (Komma, Tabulator oder Semikolon) in Marketo importieren. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, solange die Datei insgesamt weniger als 10 MB groß ist. Der Datensatzvorgang ist nur &quot;insert or update&quot;.

## Verarbeitungsbeschränkungen

Sie können mehr als eine Massen-Importanfrage mit Einschränkungen senden. Jede Anforderung wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Maximal zehn Aufträge sind jederzeit in der Warteschlange zulässig (einschließlich der beiden derzeit verarbeiteten Aufträge). Wenn Sie die maximale Anzahl von zehn Aufträgen überschreiten, wird der Fehler &quot;1016, Zu viele Importe&quot;zurückgegeben.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, in der die entsprechenden REST-API-Namen als Felder aufgeführt werden, in denen die Werte jeder Zeile zugeordnet werden. REST-API-Namen können mithilfe von abgerufen werden. [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) und/oder [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET) -Endpunkte. Datensätze können Lead-Felder, benutzerdefinierte Lead-Felder und benutzerdefinierte Felder für Programmmitglieder enthalten.

Eine typische Datei würde diesem grundlegenden Muster folgen:

```
email,firstName,lastName
test@example.com,John,Doe
```

Der Aufruf selbst erfolgt über die `multipart/form-data` Inhaltstyp.

Dieser Anfragetyp kann schwierig zu implementieren sein. Daher wird dringend empfohlen, eine vorhandene Bibliotheksimplementierung zu verwenden.

## Erstellen eines Auftrags

Die [Programmteilnehmer importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) endpoint liest eine Datei mit den Datensätzen der Programmmitglieder und fügt sie zu einem Programm mit einem bestimmten Status hinzu. Die Datensätze können sowohl Lead-Felder als auch benutzerdefinierte Felder für Programmmitglieder enthalten. Alle Datensätze müssen das E-Mail-Feld enthalten, das zur Deduplizierung verwendet wird.

Die `programId` path Parameter gibt das Programm an, dem die Mitglieder hinzugefügt werden.

Es gibt drei erforderliche Abfrageparameter. Die `format` Parameter gibt das Importdateiformat (CSV, TSV oder SSV) an, die `programMemberStatus` -Parameter gibt den Programmstatus für die Mitglieder an, die dem Programm hinzugefügt werden, und die `file` enthält den Namen der Importdatei, die Datensätze zu Programmmitgliedern enthält.

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

Beachten Sie in der Antwort auf unseren Aufruf, dass eine `batchId` und `status` -Feld für den Datensatz im Ergebnisarray. Da dieser Endpunkt asynchron ist, kann er den Status &quot;In Warteschlange&quot;, &quot;Importieren&quot;oder &quot;Fehlgeschlagen&quot;zurückgeben. Sie müssen die `batchId` um den Status des Importvorgangs zu erhalten und Fehler und/oder Warnungen nach Abschluss abzurufen. Die `batchId` für sieben Tage gültig.

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

Nachdem der Importauftrag erstellt wurde, müssen Sie dessen Status abfragen. Es empfiehlt sich, den Importauftrag alle 5-30 Sekunden abzufragen. Gehen Sie dazu wie folgt vor: `batchId` Pfadparameter zum [Abrufen des Status eines Programmteilnehmers beim Import](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) -Endpunkt.

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

Fehler werden durch die Variable `numOfRowsFailed` -Attribut in [Abrufen des Status eines Programmteilnehmers beim Import](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) Antwort. Wenn numOfRowsFailed größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Fehler an.

Verwenden Sie die [Abrufen von Fehlern bei Programmteilnehmern](http://TODO) Endpunkt zum Abrufen von Datensätzen und Ursachen für fehlgeschlagene Zeilen durch Übergeben der `batchId` Pfadparameter.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Der Endpunkt antwortet mit einer Datei, die angibt, welche Zeilen fehlgeschlagen sind, sowie einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem in `format` -Parameter bei der Auftragserstellung. An jeden Datensatz wird ein zusätzliches Feld mit einer Fehlerbeschreibung angehängt.

Angenommen, Sie importieren die folgende Datei mit einem ungültigen Lead-Ergebnis:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie `numOfRowsFailed` ist 1, der angibt, dass ein Fehler aufgetreten ist:

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

Warnhinweise werden durch die Variable `numOfRowsWithWarning` -Attribut in [Abrufen des Status eines Programmteilnehmers beim Import](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) Antwort. Wenn `numOfRowsWithWarning` größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Warnungen an.

Verwenden Sie die [Warnungen von Programmmitgliedern erhalten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) Endpunkt zum Abrufen von Datensätzen und Ursachen für Warnzeilen durch Übergabe der `batchId` Pfadparameter.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Der Endpunkt antwortet mit einer Datei, die angibt, welche Zeilen Warnungen erzeugt haben, sowie einer Meldung, die angibt, warum der Datensatz eine Warnung erzeugt hat. Das Format der Datei entspricht dem in `format` -Parameter bei der Auftragserstellung. Jedem Datensatz wird ein zusätzliches Feld mit einer Beschreibung der Warnung angehängt.

Angenommen, Sie importieren die folgende Datei mit einer ungültigen E-Mail-Adresse:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Wenn Sie den Auftragsstatus überprüfen, sehen Sie `numOfRowsWithWarning` ist 1, der angibt, dass eine Warnung aufgetreten ist:

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
