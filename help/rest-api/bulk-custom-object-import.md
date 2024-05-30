---
title: "Massen-Import benutzerdefinierter Objekte"
feature: Custom Objects
description: "Batch-Import von benutzerdefinierten Objekten."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---


# Massen-Importieren benutzerdefinierter Objekte

[Endpunktverweis zum Importieren benutzerdefinierter Massenobjekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Wenn Sie viele benutzerdefinierte Objektdatensätze importieren müssen, empfiehlt es sich, diese asynchron mithilfe der Bulk API zu importieren. Dies geschieht durch Importieren einer reduzierten Datei, die durch Kommas, Tabulatoren oder Semikolons getrennte Datensätze enthält. Die Datei kann eine beliebige Anzahl von Datensätzen enthalten, sofern ihre Größe kleiner als 10 MB ist (ansonsten wird ein HTTP-Status-Code 413 zurückgegeben). Der Inhalt der Datei hängt von Ihrer benutzerdefinierten Objektdefinition ab. Die erste Zeile enthält immer eine Kopfzeile, die die Felder auflistet, denen Werte jeder Zeile zugeordnet werden sollen. Alle Feldnamen in der Kopfzeile müssen mit einem API-Namen übereinstimmen (wie unten beschrieben). Die verbleibenden Zeilen enthalten die zu importierenden Daten, einen Datensatz pro Zeile. Der Datensatzvorgang ist nur &quot;insert or update&quot;.

## Verarbeitungsbeschränkungen

Es ist Ihnen erlaubt, innerhalb von Beschränkungen mehr als eine Massen-Importanfrage zu senden. Jede Anforderung wird als Auftrag zu einer zu verarbeitenden FIFO-Warteschlange hinzugefügt. Es werden maximal zwei Aufträge gleichzeitig verarbeitet. Maximal zehn Aufträge sind jederzeit in der Warteschlange zulässig (einschließlich der beiden derzeit verarbeiteten Aufträge). Wenn Sie die maximale Anzahl von zehn Aufträgen überschreiten, wird der Fehler &quot;1016, Zu viele Importe&quot;zurückgegeben.

## Beispiel für ein benutzerdefiniertes Objekt

Bevor Sie die Bulk-API verwenden, müssen Sie zunächst die Marketo Admin-Benutzeroberfläche verwenden, um [Erstellen eines benutzerdefinierten Objekts](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Angenommen, wir haben ein benutzerdefiniertes Objekt &quot;Auto&quot;mit den Feldern &quot;Farbe&quot;, &quot;Make&quot;, &quot;Model&quot;und &quot;VIN&quot;erstellt. Nachfolgend finden Sie die Bildschirme der Admin-Benutzeroberfläche, in denen das benutzerdefinierte Objekt angezeigt wird. Sie können sehen, dass wir das VIN-Feld für die Deduplizierung verwendet haben. Die API-Namen werden hervorgehoben, da sie beim Aufrufen von Massen-API-bezogenen Endpunkten verwendet werden müssen.

![Benutzerdefiniertes Objekt einfügen](assets/bulk-insert-co-car-1.png)

Im Folgenden finden Sie die benutzerdefinierten Objektfelder, wie in der Admin-Benutzeroberfläche beschrieben.

![Benutzerdefinierte Objektfelder einfügen](assets/bulk-insert-co-car-fields.png)

### API-Namen

Sie können API-Namen programmgesteuert abrufen, indem Sie den benutzerdefinierten Objekt-API-Namen an die [Beschreiben eines benutzerdefinierten Objekts](#describe) -Endpunkt.

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Datei importieren

Nehmen wir nun an, Sie möchten drei benutzerdefinierte &quot;Auto&quot;-Objektdatensätze importieren. Bei Verwendung des CSV-Formats (Komma-Trennzeichen) könnte die Datei wie folgt aussehen:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

Zeile 1 ist die Kopfzeile und die Zeilen 2-4 sind die benutzerdefinierten Objektdatensätze.

## Erstellen eines Auftrags

Um die Massenimportanfrage durchzuführen, müssen Sie den API-Namen des benutzerdefinierten Objekts in den Pfad zum [Benutzerdefinierte Objekte importieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST) -Endpunkt. Sie müssen auch einen Parameter &quot;file&quot;einfügen, der auf den Namen Ihrer Importdatei verweist, sowie einen Parameter &quot;format&quot;, der angibt, wie Ihre Importdatei durch Trennzeichen (&quot;csv&quot;, &quot;tsv&quot;oder &quot;ssv&quot;) getrennt ist.

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

In diesem Beispiel haben wir das &quot;csv&quot;-Format angegeben und unsere Importdatei &quot;custom_object_import.csv&quot; genannt.

Beachten Sie, dass in der Antwort auf unseren -Aufruf hier keine Liste von Erfolgen oder Fehlern aufgeführt ist, wie Sie es beim erneuten Abrufen vom Endpunkt &quot;Benutzerdefinierte Objekte synchronisieren&quot;tun würden. Stattdessen erhalten Sie eine `batchId`. Dies liegt daran, dass der Aufruf asynchron ist und eine `status` von &quot;In die Warteschlange gestellt&quot;, &quot;Importieren&quot;oder &quot;Fehlgeschlagen&quot;. Sie sollten die batchId beibehalten, damit Sie den Status des Importvorgangs erhalten oder Fehler und/oder Warnungen nach Abschluss abrufen können. Die batchId bleibt sieben Tage lang gültig.

Eine einfache Möglichkeit, die Massenimportanforderung zu replizieren, besteht darin, curl über die Befehlszeile zu verwenden:

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Wobei die Importdatei &quot;custom_object_import.csv&quot;Folgendes enthält:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Abruf-Auftragsstatus

Nachdem der Importauftrag erstellt wurde, müssen Sie dessen Status abfragen. Es empfiehlt sich, den Importauftrag alle 5-30 Sekunden abzufragen. Geben Sie dazu den API-Namen des benutzerdefinierten Objekts und die `batchId` im Pfad zum [Abrufen des Status &quot;Benutzerdefiniertes Objekt importieren&quot;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) -Endpunkt.

```
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Diese Antwort zeigt einen abgeschlossenen Import, aber die `status` kann einer der folgenden sein: Abgeschlossen, In Warteschlange, Importieren, Fehlgeschlagen. Wenn der Auftrag abgeschlossen wurde, haben Sie eine Liste der verarbeiteten Zeilen, mit Fehlern und mit Warnungen. Das message -Attribut ist auch ein guter Ort, um nach weiteren Auftragsinformationen zu suchen.

## Fehler

Fehler werden durch die Variable `numOfRowsFailed` -Attribut in [Abrufen des Status &quot;Benutzerdefiniertes Objekt importieren&quot;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) Antwort. Wenn numOfRowsFailed größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Fehler an. Aufruf [Abrufen von Fehlern beim Importieren benutzerdefinierter Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) -Endpunkt, um eine Datei mit Fehlerdetails abzurufen. Auch hier müssen Sie den benutzerdefinierten Objekt-API-Namen übergeben und `batchId` im Pfad. Wenn keine Fehlerdatei vorhanden ist, wird ein HTTP-Statuscode 404 zurückgegeben.

Um mit dem Beispiel fortzufahren, können wir einen Fehler erzwingen, indem wir die Kopfzeile ändern und &quot;vin&quot;in &quot;vin&quot;ändern (indem ein Leerzeichen zwischen dem Komma und &quot;vin&quot;hinzugefügt wird).

```
color,make,model, vin
```

Wenn wir den Status erneut importieren und überprüfen, wird diese Antwort mit `numRowsFailed`3. Dies weist auf drei Fehler hin.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Jetzt machen wir den Endpunkt &quot;Import Custom Object Failure&quot;aufrufen, um zusätzliche Fehlerdetails zu erhalten:

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

Und wir können sehen, dass wir unser Deduplizierungsfeld vermissen `vin`.

## Warnungen

Warnhinweise werden durch die Variable `numOfRowsWithWarning` -Attribut in der Antwort &quot;Import Custom Object Status&quot;. Wenn numOfRowsWithWarning größer als null ist, zeigt dieser Wert die Anzahl der aufgetretenen Warnungen an. Aufruf [Warnungen beim Importieren benutzerdefinierter Objekte abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) -Endpunkt, um eine Datei mit Warndetails abzurufen. Auch hier müssen Sie den benutzerdefinierten Objekt-API-Namen übergeben und `batchId` im Pfad. Wenn keine Warndatei vorhanden ist, wird ein HTTP-Statuscode 404 zurückgegeben.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
