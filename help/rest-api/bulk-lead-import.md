---
title: "Bulk-Lead-Import"
feature: REST API
description: "Batch-Import von Lead-Daten."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---


# Import von Bulk-Lead

[Referenz zum Import von Massen-Lead-Endpunkten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Bei großen Mengen von Lead-Datensätzen können Leads asynchron mit der [Bulk-API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Auf diese Weise können Sie eine Datensatzliste mit einer flachen Datei mit den Trennzeichen (Komma, Tabulator oder Semikolon) in Marketo importieren. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, solange die Datei insgesamt weniger als 10 MB groß ist. Der Datensatzvorgang ist nur &quot;insert or update&quot;.

## Verarbeitungsbeschränkungen

Sie können mehr als eine Massen-Importanfrage mit Einschränkungen senden. Jede Anforderung wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Maximal zehn Aufträge sind jederzeit in der Warteschlange zulässig (einschließlich der beiden derzeit verarbeiteten Aufträge). Wenn Sie die maximale Anzahl von zehn Aufträgen überschreiten, wird der Fehler &quot;1016, Zu viele Importe&quot;zurückgegeben.

## Datei importieren

Die erste Zeile der Datei muss eine Kopfzeile sein, in der die entsprechenden REST-API-Felder aufgelistet werden, denen die Werte jeder Zeile zugeordnet werden. Eine typische Datei würde diesem grundlegenden Muster folgen:

```
email,firstName,lastName
test@example.com,John,Doe
```

Die `externalCompanyId` kann verwendet werden, um den Lead-Datensatz mit einem Firmendatensatz zu verknüpfen. Die `externalSalesPersonId` kann verwendet werden, um den Lead-Datensatz mit einem Datensatz der Vertriebsperson zu verknüpfen.

Der Aufruf selbst erfolgt über die `multipart/form-data` Inhaltstyp.

Dieser Anfragetyp kann schwierig zu implementieren sein. Daher wird dringend empfohlen, eine vorhandene Bibliotheksimplementierung zu verwenden.

## Erstellen eines Auftrags

Um eine Massenimport-Anfrage zu erstellen, müssen Sie Ihre Content-Type-Kopfzeile auf &quot;multipart/form-data&quot;setzen und mindestens einen Dateiparameter mit Ihrem Dateiinhalt sowie einen Formatparameter mit dem Wert &quot;csv&quot;, &quot;tsv&quot;oder &quot;ssv&quot;angeben, der Ihr Dateiformat angibt.

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

FirstName,LastName,Email,Company
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

Dieser Endpunkt verwendet [multipart/form-data als Inhaltstyp](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Dies kann schwierig zu erreichen sein. Daher empfiehlt es sich, eine HTTP-Support-Bibliothek für Ihre Sprache zu verwenden. Eine einfache Möglichkeit, dies über die Befehlszeile mit cURL zu tun, sieht wie folgt aus:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Wobei die Importdatei &quot;lead_data.csv&quot;Folgendes enthält:

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Sie können optional auch die `lookupField`, `listId`, und `partitionName` -Parameter in Ihrer -Anfrage. `lookupField` ermöglicht Ihnen, ein bestimmtes Feld auszuwählen, in dem die Duplizierung wie bei &quot;Leads synchronisieren&quot;erfolgen soll, und standardmäßig E-Mail zu verwenden. Sie können `id` as `lookupField` um einen &quot;Nur aktualisieren&quot;-Vorgang anzugeben. `listId` ermöglicht die Auswahl einer statischen Liste, in die die Liste der Leads importiert werden soll. Dadurch werden die Leads in der Liste zu Mitgliedern dieser statischen Liste, zusätzlich zu den durch den Import verursachten Kreationen oder Aktualisierungen. `partitionName` wählt eine bestimmte Partition aus, in die importiert werden soll. Weitere Informationen finden Sie im Abschnitt Arbeitsbereiche und Partitionen .

Beachten Sie in der Antwort auf unseren -Aufruf, dass es keine Liste von Erfolgen oder Fehlern wie bei &quot;SynchronisierungsLeads&quot;gibt, sondern eine batchId und ein Statusfeld für den Datensatz im Ergebnis-Array. Der Grund dafür ist, dass diese API asynchron ist und den Status &quot;In Warteschlange&quot;, &quot;Importieren&quot;oder &quot;Fehlgeschlagen&quot;zurückgeben kann. Sie müssen die batchId beibehalten, um den Status des Importvorgangs zu erhalten und Fehler und/oder Warnungen nach Abschluss abzurufen. Die batchId bleibt sieben Tage lang gültig.

## Abruf-Auftragsstatus

Je nach erforderlicher Latenz und API-Aufrufbeschränkungen empfiehlt es sich, den Auftrag alle 5-30 Sekunden abzufragen, um den Status des Importvorgangs zu sehen. Dies können Sie mit der API Import Lead Status abrufen.

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

Diese Antwort zeigt einen abgeschlossenen Import an, der Status kann jedoch einer der folgenden sein:

- Abgeschlossen
- In Warteschl. versch
- Wird importiert
- Fehlgeschlagen

Wenn der Auftrag abgeschlossen wurde, haben Sie eine Liste mit der Anzahl der verarbeiteten, fehlgeschlagenen Zeilen, für die Warnhinweise angezeigt wurden. Der Nachrichtenparameter gibt möglicherweise auch die Fehlermeldung aus, wenn der Status Fehlgeschlagen lautet.

## Fehler

Fehler werden durch das Attribut &quot;numOfRowsFailed&quot;in der Antwort &quot;Get Import Lead Status&quot;angezeigt. Wenn &quot;numOfRowsFailed&quot;größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Fehler an.

Um die Datensätze und Ursachen für fehlgeschlagene Zeilen abzurufen, müssen Sie die Fehlerdatei abrufen:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

Die API antwortet mit einer Datei, die angibt, welche Zeilen fehlgeschlagen sind, sowie einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem, das beim Erstellen des Auftrags im Parameter &quot;format&quot;angegeben wurde. An jeden Datensatz wird ein zusätzliches Feld mit einer Fehlerbeschreibung angehängt.

## Warnungen

Warnungen werden durch das Attribut &quot;numOfRowsWithWarning&quot;in der Antwort &quot;Import Lead Status&quot;(Lead-Status abrufen) angezeigt. Wenn &quot;numOfRowsWithWarning&quot;größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Warnungen an.

Rufen Sie die Warnungsdatei ab, um die Datensätze und Ursachen für Warnzeilen abzurufen:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

Die API antwortet mit einer Datei, die angibt, welche Zeilen Warnungen erzeugt haben, sowie einer Meldung, die angibt, warum der Datensatz fehlgeschlagen ist. Das Format der Datei entspricht dem, das beim Erstellen des Auftrags im Parameter &quot;format&quot;angegeben wurde. Jedem Datensatz wird ein zusätzliches Feld mit einer Beschreibung der Warnung angehängt.
