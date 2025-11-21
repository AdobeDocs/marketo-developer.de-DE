---
title: Massenimport von Leads
feature: REST API
description: Erstellen und überwachen Sie asynchrone Massenimporte von Leads in Marketo mit CSV TSV oder SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: c1b9763835b25584f0c085274766b68ddf5c7ae2
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 1%

---

# Massenimport von Leads

[Referenz zum Massenimport-Endpunkt für Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Bei großen Mengen an Lead-Datensätzen können Leads asynchron mit der [Bulk-API“ importiert &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Auf diese Weise können Sie eine Liste von Datensätzen mithilfe einer flachen Datei mit den Trennzeichen (Komma, Tabulator oder Semikolon) in Marketo importieren. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, sofern die Datei weniger als 10 MB groß ist. Der Datensatzvorgang ist nur „Einfügen oder Aktualisieren“.

## Verarbeitungsbeschränkungen

Es ist zulässig, mehr als eine Massenimportanfrage zu senden, mit Einschränkungen. Jede Anfrage wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Es sind maximal 10 Aufträge gleichzeitig in der Warteschlange zulässig (einschließlich der beiden derzeit verarbeiteten). Wenn Sie den Maximalwert von zehn Aufträgen überschreiten, wird ein `1016, Too many imports` zurückgegeben.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, die die entsprechenden REST-API-Felder auflistet, denen die Werte jeder Zeile zugeordnet werden sollen. Eine typische Datei würde diesem grundlegenden Muster folgen:

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Das `externalCompanyId` Feld kann verwendet werden, um den Lead-Datensatz mit einem Firmendatensatz zu verknüpfen. Das `externalSalesPersonId` Feld kann verwendet werden, um den Lead-Datensatz mit einem Verkaufspersonendatensatz zu verknüpfen.

Der Aufruf selbst erfolgt über den Inhaltstyp `multipart/form-data`.

Dieser Anfragetyp kann schwierig zu implementieren sein. Daher wird dringend empfohlen, eine vorhandene Bibliotheksimplementierung zu verwenden.

## Erstellen von Aufträgen

Um eine Massenimportanfrage zu stellen, müssen Sie Ihre Kopfzeile für den Inhaltstyp auf `multipart/form-data` festlegen und mindestens einen `file` Parameter in Ihren Dateiinhalt und einen `format` mit dem Wert `csv`, `tsv` oder `ssv` einschließen, der Ihr Dateiformat bezeichnet.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

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

Dieser Endpunkt verwendet [multipart/form-data als Inhaltstyp](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Es empfiehlt sich, eine HTTP-Support-Bibliothek für die Sprache Ihrer Wahl zu verwenden, um eine korrekte Verwendung sicherzustellen. Das folgende Beispiel zeigt eine einfache Möglichkeit, dies mit cURL über die Befehlszeile zu tun:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Wenn die Importdatei `lead_data.csv` Folgendes enthält:

```
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Optional können Sie auch die Parameter `lookupField`, `listId` und `partitionName` in Ihre Anfrage einbeziehen. `lookupField` können Sie ein bestimmtes Feld auswählen, in dem eine Deduplizierung durchgeführt werden soll, genau wie Leads synchronisieren, und standardmäßig auf E-Mail setzen. Sie können `id` als `lookupField` angeben, um einen „nur aktualisieren“-Vorgang anzugeben. Mit `listId` können Sie eine statische Liste auswählen, in die Sie die Liste der Leads importieren möchten. Dadurch werden die Leads in der Liste zu Mitgliedern dieser statischen Liste, zusätzlich zu allen durch den Import verursachten Erstellungen oder Aktualisierungen. `partitionName` wählt eine bestimmte Partition zum Importieren aus. Weitere Informationen finden Sie im Abschnitt Arbeitsbereiche und Partitionen .

Beachten Sie in der Antwort auf unseren Aufruf, dass es keine Liste von Erfolgen oder Fehlern wie bei Synchronisierungs-Leads gibt, sondern eine batchId und ein Statusfeld für den Datensatz im Ergebnis-Array. Dies liegt daran, dass diese API asynchron ist und den Status „In Warteschlange“, „Importieren“ oder „Fehlgeschlagen“ zurückgeben kann. Sie müssen die batchId beibehalten, um den Status des Importvorgangs abzurufen und Fehler und/oder Warnungen nach Abschluss des Vorgangs abzurufen. Die batchId bleibt sieben Tage gültig.

## Status des Abrufauftrags

Es empfiehlt sich, den Auftrag je nach erforderlicher Latenz und den API-Aufrufbeschränkungen alle 5 bis 30 Sekunden abzufragen, um den Status des Importvorgangs anzuzeigen. Dies können Sie mit der API Lead-Status abrufen tun.

```
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

Diese Antwort zeigt einen abgeschlossenen Import an, aber der Status kann einer der folgenden sein:

- Abgeschlossen
- In Warteschl. versch
- Wird importiert
- Fehlgeschlagen

Wenn der Auftrag abgeschlossen ist, wird eine Liste der verarbeiteten Zeilen angezeigt, d. h. der Zeilen, die fehlgeschlagen sind, d. h. der Zeilen mit Warnhinweisen. Der Nachrichtenparameter kann auch die Fehlermeldung ausgeben, wenn der Status Fehlgeschlagen ist.

## Fehler

Fehler werden durch das Attribut `numOfRowsFailed` in der Antwort zum Abrufen des Lead-Importstatus angezeigt. Wenn `numOfRowsFailed` größer als null ist, gibt dieser Wert die Anzahl der aufgetretenen Fehler an.

Um die Datensätze und Ursachen fehlgeschlagener Zeilen abzurufen, müssen Sie die Fehlerdatei abrufen:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

Die API antwortet mit einer Datei, die angibt, welche Zeilen fehlgeschlagen sind, sowie mit einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem Format, das `format` Parameter bei der Auftragserstellung angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Beschreibung des Fehlers angehängt.

## Warnungen

Warnungen werden durch das Attribut `numOfRowsWithWarning` in einer Antwort zum Abrufen des Lead-Importstatus angezeigt. Wenn `numOfRowsWithWarning` größer als null ist, gibt dieser Wert die Anzahl der aufgetretenen Warnungen an.

Um die Datensätze und Ursachen von Warnzeilen abzurufen, rufen Sie die Warndatei ab:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

Die API antwortet mit einer Datei, die angibt, welche Zeilen Warnungen erzeugt haben, zusammen mit einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem Format, das `format` Parameter bei der Auftragserstellung angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Beschreibung der Warnung angehängt.
