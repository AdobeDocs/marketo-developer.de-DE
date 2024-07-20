---
title: Massenextraktion benutzerdefinierter Objekte
feature: REST API, Custom Objects
description: Stapelverarbeitung benutzerdefinierter Marketo-Objekte.
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 1%

---

# Massenextraktion benutzerdefinierter Objekte

[Referenz: Endpunkt der Massenextraktion benutzerdefinierter Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

Der Satz mit benutzerdefinierten REST-APIs zur Massenextraktion von benutzerdefinierten Objekten bietet eine programmatische Schnittstelle zum Abrufen großer Mengen benutzerdefinierter Objektdatensätze aus Marketo. Dies ist die empfohlene Schnittstelle für Anwendungsfälle, die einen kontinuierlichen Datenaustausch zwischen Marketo und einem oder mehreren externen Systemen erfordern, zum ETL-, Daten-Warehouse- und Archivierungszwecken.

Diese API unterstützt den Export benutzerdefinierter Marketo-Objektdatensätze der ersten Ebene, die direkt mit einem Lead verknüpft sind. Übergeben Sie den Namen des benutzerdefinierten Objekts und eine Liste von Leads, mit denen das Objekt verknüpft ist. Für jeden Lead in der Liste werden die verknüpften benutzerdefinierten Objektdatensätze, die mit dem angegebenen benutzerdefinierten Objektnamen übereinstimmen, als Zeilen in die Exportdatei geschrieben. Benutzerdefinierte Objektdaten sind auf der Registerkarte [Benutzerdefiniertes Objekt auf der Detailseite des Leads in der Marketo-Benutzeroberfläche](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects) sichtbar.

## Berechtigungen

Für die APIs zur Massenextraktion benutzerdefinierter Objekte ist es erforderlich, dass der API-Benutzer über eine oder beide der Berechtigungen &quot;Schreibgeschütztes benutzerdefiniertes Objekt&quot;oder &quot;Benutzerdefiniertes Objekt lesen und schreiben&quot;verfügt.

## Filter

Die benutzerdefinierte Objektextraktion unterstützt mehrere Filteroptionen, mit denen eine Liste von Leads angegeben wird, die mit dem benutzerdefinierten Objekt verknüpft sind. Wenn ein Lead in der Liste mit benutzerdefinierten Objektdatensätzen verknüpft ist, die mit einem bestimmten benutzerdefinierten Objektnamen übereinstimmen, werden die Datensätze in die Exportdatei geschrieben. Pro Exportauftrag kann nur ein Filtertyp angegeben werden.

| Filtertyp | Datentyp | Hinweise |
|---|---|---|
| `updatedAt` | Datumsbereich | Akzeptiert ein JSON-Objekt mit den Elementen `startAt` und `endAt` &amp;nbsp.;`startAt` akzeptiert einen Datum, der das Wasserzeichen mit niedrigem Wasserzeichen darstellt, und `endAt` akzeptiert einen Datum, der das Wasserzeichen mit hohem Wasserzeichen darstellt. Der Zeitraum muss 31 Tage oder weniger betragen. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die innerhalb des Datumsbereichs aktualisiert wurden. Die Datenzeiten sollten im ISO-8601-Format vorliegen, ohne Millisekunden. |
| `staticListName` | Zeichenfolge | Akzeptiert den Namen einer statischen Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zum Zeitpunkt der Verarbeitung des Auftrags Mitglieder der statischen Liste sind. Rufen Sie mit dem Endpunkt &quot;Listen abrufen&quot;statische Listennamen ab. |
| `staticListId` | Ganze Zahl | Akzeptiert die ID einer statischen Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zum Zeitpunkt der Verarbeitung des Auftrags Mitglieder der statischen Liste sind. Rufen Sie statische Listen-IDs mithilfe des Endpunkts &quot;Listen abrufen&quot;ab. |
| `smartListName`* | Zeichenfolge | Akzeptiert den Namen einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zu dem Zeitpunkt, zu dem der Auftrag verarbeitet wird, Mitglieder der Smart-Listen sind. Rufen Sie mit dem Endpunkt Smart-Listen abrufen Smart-Namen ab. |
| `smartListId`* | Ganze Zahl | Akzeptiert die ID einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zu dem Zeitpunkt, zu dem der Auftrag verarbeitet wird, Mitglieder der Smart-Listen sind. Abrufen von Smart-List-IDs mithilfe des Endpunkts &quot;Smart-Listen abrufen&quot;. |

Für einige Abonnements ist der Filtertyp nicht verfügbar. Wenn Sie für Ihr Abonnement nicht verfügbar sind, erhalten Sie beim Aufruf des Endpunkts &quot;Lead-Exportauftrag erstellen&quot;eine Fehlermeldung (&quot;1035, Nicht unterstützter Filtertyp für Zielabonnement&quot;). Kunden können sich an den Marketo-Support wenden, damit diese Funktion in ihrem Abonnement aktiviert wird.

## Optionen

Der Endpunkt [Benutzerdefinierten Objektexport erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) bietet mehrere Formatierungsoptionen. Diese Optionen bieten Benutzern folgende Möglichkeiten:

- Geben Sie die in die exportierte Datei einzuschließenden Felder an
- Spaltenüberschriften dieser Felder umbenennen
- Format der exportierten Datei angeben

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| `fields` | Array[String] | Ja | Array von Zeichenfolgen, die den Wert des benutzerdefinierten Objektattributnamens enthalten, der vom Endpunkt &quot;Describe Custom Object&quot;zurückgegeben wird. Die aufgelisteten Felder sind in der exportierten Datei enthalten. |
| `columnHeaderNames` | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenüberschriftsnamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportauftrag enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| `format` | Zeichenfolge | Nein | Akzeptiert eines von: CSV, TSV, SSV. Die exportierte Datei wird als Datei mit kommagetrennten Werten, tabulatorgetrennten Werten oder durch Leerzeichen getrennten Werten gerendert, sofern festgelegt. Wenn nicht festgelegt, wird standardmäßig CSV verwendet. |


## Erstellen eines Auftrags

Die Parameter für den Auftrag werden vor dem Starten des Exports mithilfe des Endpunkts [Benutzerdefinierten Objektexport erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) definiert.

Der erforderliche Pfadparameter &quot;`apiName`&quot;ist der benutzerdefinierte Objektname, der vom Endpunkt &quot;[Describe Custom Object](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1)&quot;zurückgegeben wird. Gibt an, welches benutzerdefinierte Marketo-Objekt exportiert werden soll. Benutzerdefinierte CRM-Objekte sind nicht zulässig. Der erforderliche Parameter `filter` enthält die Liste der Leads, die mit dem benutzerdefinierten Objekt verknüpft sind. Dies kann auf eine statische Liste oder eine Smart-Liste verweisen. Der erforderliche Parameter `fields` enthält die API-Namen der benutzerdefinierten Objektattribute, die in die Exportdatei aufgenommen werden sollen. Optional können wir die `format` der Datei und die `columnHeaderNames` definieren.

Nehmen wir als Beispiel an, dass wir ein benutzerdefiniertes Objekt namens &quot;Auto&quot;mit den folgenden Feldern erstellt haben: Farbe, Marke, Modell, VIN. Das Linkfeld ist die Lead-ID und das Deduplizierungsfeld ist VIN.

Eigene Objektdefinition

![Benutzerdefiniertes Objekt](assets/custom-object-car.png)


Benutzerdefinierte Objektfelder

![Benutzerdefinierte Objektfelder](assets/custom-object-car-fields.png)

Wir können [Benutzerdefiniertes Objekt beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) aufrufen, um die benutzerdefinierten Objektattribute, die im Attribut `fields` in der Antwort angezeigt werden, programmatisch zu überprüfen.

```
GET /rest/v1/customobjects/car_c/describe.json
```

```json
{
    "requestId": "148ef#1793e00f64f",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2021-05-05T16:14:41Z",
            "updatedAt": "2021-05-05T16:14:42Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vIN"
            ],
            "searchableFields": [
                [
                    "vIN"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "Id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vIN",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Erstellen Sie mehrere benutzerdefinierte Objektdatensätze und verknüpfen Sie jeden mit einem anderen Lead, indem Sie den Endpunkt [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) verwenden. Ein Lead kann mit vielen benutzerdefinierten Objektdatensätzen verknüpft werden. Dies wird als eine &quot;Eins-zu-viele&quot;-Beziehung bezeichnet.

```
POST /rest/v1/customobjects/car_c.json
```

```json
{
   "action":"createOrUpdate",
   "input":[
       {
           "leadId": 11,
           "color": "Pearl White",
           "make": "Tesla",
           "model": "Model S",
           "vIN": "5YJSA1E41FF156789"
       },
       {
           "leadId": 12,
           "color": "Midnight Silver Metallic",
           "make": "Tesla",
           "model": "Model X",
           "vIN": "LRWXB2B41FF198765"
       },
       {
           "leadId": 13,
           "color": "Fusion Red",
           "make": "Tesla",
           "model": "Roadster",
           "vIN": "SFGRC3C41FF154321"
       }
    ]
}
```

```json
{
    "requestId": "50d9#1793e066088",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "d911eaa1-fd0b-4a99-9b71-c6a7233c782c",
            "status": "created"
        },
        {
            "seq": 1,
            "marketoGUID": "20d04ffb-51f0-4336-924c-c783b9bb4215",
            "status": "created"
        },
        {
            "seq": 2,
            "marketoGUID": "e7da4331-8e7a-473b-85c8-047638eb6c7f",
            "status": "created"
        }
    ],
    "success": true
}
```

Jede der drei oben genannten Leads gehört zu einer statischen Liste mit dem Namen &quot;Autoverkäufer&quot;, deren `id` 1081 ist. Dies wird unten durch Aufruf des Endpunkts [Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1) angezeigt.

```
GET /rest/v1/lists/1081/leads.json
```

```json
{
    "requestId": "d023#1793e1e982b",
    "result": [
        {
            "id": 11,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Hanna.Crawford@pookmail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 12,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Bertha.Fulton@trashymail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 13,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Faith.England@dodgit.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        }
    ],
    "success": true
}
```

Erstellen wir nun einen Exportauftrag, um diese Datensätze abzurufen. Mithilfe des Endpunkts [Benutzerdefinierten Objektauftrag exportieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) geben wir benutzerdefinierte Objektattribute im Parameter `fields` und eine statische Listen-ID im Parameter `filter` an.

```
POST /bulk/v1/customobjects/car_c/export/create.json
```

```json
{
    "fields": [
        "leadId",
        "color",
        "make",
        "model",
        "vIN"
    ],
    "filter": {
        "staticListId": 1081
    }
}
```

```json
{
    "requestId": "8d2f#1793e289e87",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2021-05-05T20:12:01Z"
        }
    ],
    "success": true
}
```

Dadurch wird in der Antwort ein Status zurückgegeben, der angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, wurde jedoch noch nicht gestartet. Dazu muss der Endpunkt [Export Custom Object Job exportieren in die Warteschlange ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) mit dem Wert `apiName` und der Endpunkt `exportId` aus der Erstellungsstatusantwort aufgerufen werden.

```
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/enqueue.json
```

```json
{
    "requestId": "cfaf#1793e2a0762",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z"
        }
    ],
    "success": true
}
```

Dies antwortet mit dem anfänglichen Wert `status` von &quot;In Warteschlange&quot;und dem Wert &quot;Verarbeitung&quot;, wenn ein verfügbarer Export-Slot vorhanden ist.

## Abruf-Auftragsstatus

Der Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags seinen Status abfragen, um seinen Fortschritt zu bestimmen. Umfrage mit dem Endpunkt [Export Custom Object Job Status abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET) . Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine kürzere Abrufhäufigkeit nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Das Statusfeld kann mit einer der folgenden Optionen reagieren: &quot;Erstellt&quot;, &quot;In Warteschlange&quot;, &quot;Verarbeitung&quot;, &quot;Abgebrochen&quot;, &quot;Abgeschlossen&quot;oder &quot;Fehlgeschlagen&quot;.

```
GET /bulk/v1/customobjects/{apiName}/export/{exportId}/status.json
```

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z"
        }
    ],
    "success": true
}
```

Der Statusendpunkt gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Sobald der Auftrag &quot;`status`&quot; in &quot;Abgeschlossen&quot;geändert wurde, kann er heruntergeladen werden.

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z",
            "finishedAt": "2021-05-05T20:14:28Z",
            "numberOfRecords": 3,
            "fileSize": 182,
            "fileChecksum": "sha256:fac0cabc2352229c12e18b2fde03d1f24178bc71e9e926f520ae8d61bbe98c01"
        }
    ],
    "success": true
}
```

## Abrufen Ihrer Daten

Um die Datei eines abgeschlossenen benutzerdefinierten Objektexports abzurufen, rufen Sie einfach den Endpunkt [Export Custom Object File ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) mit Ihrem `apiName` und Ihrem `exportId` auf.

Die Antwort enthält eine Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der Datei. Wenn ein angefordertes benutzerdefiniertes Objektattribut leer ist (keine Daten enthält), wird `null` in das entsprechende Feld in der Exportdatei eingefügt.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Um das teilweise und wiederverwendbare Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional den HTTP-Header-Bereich der Typ-Bytes. Wenn der Header nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Weitere Informationen zur Verwendung der Bereichskopfzeile finden Sie in Marketo [Massenextraktion](bulk-extract.md) .

## Abbruch eines Auftrags

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach über den Endpunkt [Benutzerdefinierten Objektauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST) abgebrochen werden. Dies antwortet mit einem &quot;`status`&quot;, der angibt, dass der Auftrag abgebrochen wurde.

```
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/cancel.json
```

```json
{
    "requestId": "e5f9#179391286a7",
    "result": [
        {
            "exportId": "4a8cdd80-0d16-4dd6-9923-6ec97e30e91b",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2021-05-04T20:24:33Z"
        }
    ],
    "success": true
}
```
