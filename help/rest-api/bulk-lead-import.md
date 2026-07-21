---
title: Massenimport von Leads
feature: REST API
description: Erstellen und überwachen Sie asynchrone Massenimporte von Leads in Marketo mit CSV TSV oder SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
TQID: https://experienceleague.adobe.com/UamXYWis5J1ERqnp5lAnfUf3pFcgfSOLfKRXRB-Yg4I
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
source-wordcount: 623
ht-degree: 1%

---

# Massenimport von Leads

[Referenz zum Massenimport von Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads)

Verwenden Sie die [Bulk-API](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) um eine große Anzahl von Lead-Datensätzen asynchron zu importieren. Stellen Sie die Datensätze in einer flachen Datei mit Komma, Tabulatoren oder Semikolons bereit, die kleiner als 10 MB ist.

Der Massenimport von Leads unterstützt nur den Datensatzvorgang „Einfügen oder aktualisieren“.

## Verarbeitungsbeschränkungen

Jede Massenimportanfrage wird als Auftrag zu einer FIFO-Warteschlange (First-In, First-Out) hinzugefügt. Es gelten die folgenden Grenzwerte:

- Es können maximal zwei Aufträge gleichzeitig verarbeitet werden.
- Es können maximal 10 Aufträge in der Warteschlange sein, einschließlich der beiden verarbeiteten Aufträge.

Wenn Sie das Maximum von 10 Aufträgen überschreiten, gibt die API einen `1016, Too many imports` Fehler zurück.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, die die REST-API-Felder auflistet, denen die Werte in jeder Zeile zugeordnet sind. Eine typische Datei folgt diesem Muster:

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Verwenden Sie `externalCompanyId`, um einen Lead-Datensatz mit einem Firmendatensatz zu verknüpfen. Verwenden Sie `externalSalesPersonId`, um einen Lead-Datensatz mit einem Verkaufspersonendatensatz zu verknüpfen.

Senden Sie die Anfrage mit dem `multipart/form-data` Inhaltstyp. Verwenden Sie eine vorhandene Bibliotheksimplementierung, um die mehrteilige Anfrage zu erstellen.

## Erstellen von Aufträgen

Um einen Massenimportvorgang zu erstellen, setzen Sie den Inhaltstyp auf `multipart/form-data` und schließen Sie die folgenden Parameter ein:

- `file`: Der Inhalt der Importdatei.
- `format`: Das Dateiformat. Gültige Werte sind `csv`, `tsv` und `ssv`.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
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

Dieser Endpunkt verwendet [multipart/form-data als Inhaltstyp](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Verwenden Sie eine HTTP-Support-Bibliothek für Ihre bevorzugte Sprache, um die Anfrage korrekt zu erstellen. Im folgenden Beispiel wird cURL über die Befehlszeile verwendet:

```bash
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

In diesem Beispiel enthält die `lead_data.csv` Importdatei die folgenden Daten:

```text
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Sie können auch die folgenden optionalen Parameter einbeziehen:

- `lookupField`: Wählt das Deduplizierungsfeld aus und `email` standardmäßig aus. Geben Sie an, `id` ein „nur aktualisieren“-Vorgang ausgeführt werden soll.
- `listId`: Wählt eine statische Liste aus. Importierte Leads werden zusätzlich zu den vom Import erstellten oder aktualisierten Datensätzen Mitglieder dieser Liste.
- `partitionName`: Wählt die Partition aus, in die importiert werden soll. Weitere Informationen finden Sie im Abschnitt Arbeitsbereiche und Partitionen .

Da die API asynchron ist, enthält die Antwort `batchId` und `status` Felder anstelle von einzelnen Erfolgen und Fehlern. Der Status kann `Queued`, `Importing` oder `Failed` sein.

Behalten Sie die `batchId` bei, um den Auftragsstatus zu überprüfen und Fehler oder Warnungen nach Abschluss abzurufen. Die `batchId` bleibt sieben Tage gültig.

## Status des Abrufauftrags

Verwenden Sie die API Lead-Status abrufen , um den Auftrag je nach Latenzanforderungen und API-Aufrufbeschränkungen alle 5 bis 30 Sekunden abzufragen.

```http
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

Diese Antwort zeigt einen abgeschlossenen Import an. Der Status kann einer der folgenden Werte sein:

- Abgeschlossen
- In Warteschl. versch
- Wird importiert
- Fehlgeschlagen

Wenn der Auftrag abgeschlossen ist, listet die Antwort die Anzahl der verarbeiteten Zeilen auf, die fehlgeschlagen sind und mit Warnungen verarbeitet wurden. Der `message` kann auch eine Fehlermeldung bereitstellen, wenn der Status `Failed` ist.

## Fehler

Das Attribut `numOfRowsFailed` in der Antwort Lead-Status abrufen gibt die Anzahl der fehlgeschlagenen Zeilen an. Ein Wert größer als null bedeutet, dass Fehler aufgetreten sind.

Um die fehlgeschlagenen Datensätze und ihre Ursachen abzurufen, fordern Sie die Fehlerdatei an:

```http
GET /bulk/v1/leads/batch/{id}/failures.json
```

Die API gibt eine Datei zurück, die jede fehlgeschlagene Zeile identifiziert und erklärt, warum der Datensatz fehlgeschlagen ist. Die Datei verwendet das Format, das durch den Parameter &quot;`format`&quot; bei der Auftragserstellung angegeben wird. Ein zusätzliches Feld in jedem Datensatz beschreibt den Fehler.

## Warnungen

Das Attribut `numOfRowsWithWarning` in der Antwort Lead-Status abrufen gibt die Anzahl der Zeilen mit Warnungen an. Ein Wert größer als null bedeutet, dass Warnungen aufgetreten sind.

Um die betroffenen Datensätze und ihre Ursachen abzurufen, fordern Sie die Warndatei an:

```http
GET /bulk/v1/leads/batch/{id}/warnings.json
```

Die API gibt eine Datei zurück, die jede Zeile mit einer Warnung identifiziert und erläutert, warum die Warnung aufgetreten ist. Die Datei verwendet das Format, das durch den Parameter &quot;`format`&quot; bei der Auftragserstellung angegeben wird. Ein zusätzliches Feld in jedem Datensatz beschreibt die Warnung.
