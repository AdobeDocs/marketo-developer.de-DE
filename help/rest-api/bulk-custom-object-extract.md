---
title: Massenextraktion benutzerdefinierter Objekte
feature: REST API, Custom Objects
description: Stapelverarbeitung von benutzerdefinierten Marketo-Objekten.
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 1%

---

# Massenextraktion benutzerdefinierter Objekte

[Massenreferenz zum Extrahieren von benutzerdefinierten Objekten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

Die Gruppe der REST-APIs zum Extrahieren benutzerdefinierter Massenobjekte bietet eine programmgesteuerte Schnittstelle zum Abrufen großer Mengen benutzerdefinierter Objektdatensätze aus Marketo. Dies ist die empfohlene Oberfläche für Anwendungsfälle, in denen Daten aus Gründen der ETL, des Data Warehousing und der Archivierung kontinuierlich zwischen Marketo und einem oder mehreren externen Systemen ausgetauscht werden müssen.

Diese API unterstützt den Export von benutzerdefinierten Marketo-Objektdatensätzen der ersten Ebene, die direkt mit einem Lead verknüpft sind. Übergeben Sie den Namen des benutzerdefinierten Objekts und eine Liste der Leads, mit denen das Objekt verknüpft ist. Für jeden Lead in der Liste werden die verknüpften benutzerdefinierten Objektdatensätze, die mit dem angegebenen benutzerdefinierten Objektnamen übereinstimmen, als Zeilen in die Exportdatei geschrieben. Benutzerdefinierte Objektdaten werden auf der Registerkarte [Benutzerdefiniertes Objekt“ der Detailseite des Leads in der Marketo-Benutzeroberfläche angezeigt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Berechtigungen

Die APIs zum Extrahieren benutzerdefinierter Massenobjekte erfordern, dass der API-Benutzer über eine Rolle mit einer oder beiden der Berechtigungen „Schreibgeschütztes benutzerdefiniertes Objekt“ oder „Lese-/Schreibzugriff für benutzerdefiniertes Objekt“ verfügt.

## Filter

Benutzerdefiniertes Objekt - Extrahieren unterstützt mehrere Filteroptionen, mit denen eine Liste von Leads angegeben wird, die mit dem benutzerdefinierten Objekt verknüpft sind. Wenn ein Lead in der Liste mit benutzerdefinierten Objektdatensätzen verknüpft ist, die mit einem angegebenen benutzerdefinierten Objektnamen übereinstimmen, werden die Datensätze in die Exportdatei geschrieben. Pro Exportvorgang kann nur ein Filtertyp angegeben werden.

| Filtertyp | Datentyp | Hinweise |
|---|---|---|
| `updatedAt` | Datumsbereich | Akzeptiert ein JSON-Objekt mit den Membern `startAt` und `endAt` &amp;nbsp.;`startAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und `endAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die innerhalb des Datumsbereichs zugegriffen werden kann. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden. |
| `staticListName` | String | Akzeptiert den Namen einer statischen Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der statischen Liste sind, zu dem der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listennamen mithilfe des Endpunkts „Listen abrufen“ ab. |
| `staticListId` | Ganzzahl | Akzeptiert die ID einer statischen Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der statischen Liste sind, zu dem der Auftrag mit der Verarbeitung beginnt. Rufen Sie statische Listen-IDs mithilfe des Endpunkts „Listen abrufen“ ab. |
| `smartListName`* | String | Akzeptiert den Namen einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der Smart-Listen sind, zu dem der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listennamen mithilfe des Endpunkts „Smart-Listen abrufen“. |
| `smartListId`* | Ganzzahl | Akzeptiert die ID einer Smart-Liste. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zu dem Zeitpunkt Mitglieder der Smart-Listen sind, zu dem der Auftrag mit der Verarbeitung beginnt. Abrufen von Smart-Listen-IDs mit dem Endpunkt „Smart-Listen abrufen“. |

Filtertyp ist für einige Abonnements nicht verfügbar. Wenn für Ihr Abonnement nicht verfügbar ist, erhalten Sie eine Fehlermeldung beim Aufruf des Endpunkts „Exportleitungs-Auftrag erstellen“ („1035, Nicht unterstützter Filtertyp für Zielabonnement„). Kunden können sich an den Marketo-Support wenden, um diese Funktion in ihrem Abonnement aktivieren zu lassen.

## Optionen

Der Endpunkt [Benutzerdefinierten Objektauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) bietet mehrere Formatierungsoptionen. Diese Optionen bieten Benutzenden folgende Möglichkeiten:

- Geben Sie die Felder an, die in die exportierte Datei aufgenommen werden sollen
- Spaltenüberschriften dieser Felder umbenennen
- Geben Sie das Format der exportierten Datei an

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| `fields` | array[string] | Ja | Array von Zeichenfolgen, das den Wert des benutzerdefinierten Objektattributnamens enthält, der vom Endpunkt „Describe Custom Object“ zurückgegeben wird. Die aufgelisteten Felder sind in der exportierten Datei enthalten. |
| `columnHeaderNames` | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenkopfzeilennamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportvorgang enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| `format` | Zeichenfolge | Nein | Akzeptiert eine der folgenden Optionen: CSV, TSV, SSV. Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertedatei gerendert, sofern festgelegt. Die Standardeinstellung ist CSV, wenn nicht festgelegt. |


## Erstellen von Aufträgen

Die Parameter für den Auftrag werden vor dem Start des Exports mithilfe des Endpunkts [Benutzerdefinierten Objektauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) definiert.

Der erforderliche `apiName`-Pfadparameter ist der benutzerdefinierte Objektname, der vom Endpunkt [Benutzerdefiniertes Objekt beschreiben“ zurückgegeben ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1). Dies gibt an, welches benutzerdefinierte Marketo-Objekt exportiert werden soll. Benutzerdefinierte CRM-Objekte sind nicht zulässig. Der erforderliche `filter` enthält die Liste der Leads, die mit dem benutzerdefinierten Objekt verknüpft sind. Dies kann auf eine statische Liste oder eine Smart-Liste verweisen. Der erforderliche `fields`-Parameter enthält die API-Namen der benutzerdefinierten Objektattribute, die in die Exportdatei aufgenommen werden sollen. Optional können wir den `format` der Datei und die `columnHeaderNames` definieren.

Nehmen wir als Beispiel an, dass wir ein benutzerdefiniertes Objekt namens „Auto“ mit den folgenden Feldern erstellt haben: Farbe, Marke, Modell, FIN. Das Verknüpfungsfeld ist die Lead-ID und das Deduplizierungsfeld ist die FIN.

Benutzerdefinierte Objektdefinition

![Benutzerdefiniertes Objekt](assets/custom-object-car.png)


Benutzerdefinierte Objektfelder

![Benutzerdefinierte Objektfelder](assets/custom-object-car-fields.png)

Wir können [Benutzerdefiniertes Objekt beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) aufrufen, um die benutzerdefinierten Objektattribute, die im `fields` Attribut in der Antwort angezeigt werden, programmgesteuert zu überprüfen.

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

Erstellen Sie mehrere benutzerdefinierte Objektdatensätze und verknüpfen Sie sie jeweils mit einem anderen Lead mithilfe des Endpunkts [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST). Ein Lead kann mit vielen benutzerdefinierten Objektdatensätzen verknüpft werden. Dies wird als „Eins-zu-viele“-Beziehung bezeichnet.

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

Jeder der drei oben referenzierten Leads gehört zu einer statischen Liste mit dem Namen „Autokäufer“, deren `id` 1081 ist, wie unten durch Aufruf des Endpunkts [Leads nach Listen-ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1) zu sehen ist.

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

Erstellen wir nun einen Exportvorgang, um diese Datensätze abzurufen. Über den Endpunkt [Benutzerdefinierten Objektauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) geben wir benutzerdefinierte Objektattribute im `fields` und eine statische Listen-ID im `filter` an.

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

Dadurch wird in der Antwort ein Status zurückgegeben, der angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, aber noch nicht gestartet. Dazu muss der Endpunkt [Benutzerdefinierter Objektauftrag in die Warteschlange einreihen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) über die `apiName` und die `exportId` aus der Erstellungsstatusantwort aufgerufen werden.

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

Dies antwortet mit der anfänglichen `status` „In Warteschlange“, nach der auf „Verarbeitung läuft“ gesetzt wird, wenn ein Exportsteckplatz verfügbar ist.

## Status des Abrufauftrags

Der Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags dessen Status abfragen, um den Fortschritt zu ermitteln. Abfrage mit dem Endpunkt [Abrufen des benutzerdefinierten Objektauftragsstatus ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET). Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine niedrigere Abfrageintervall nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Das Statusfeld kann mit einem der folgenden Elemente antworten: Erstellt, In Warteschlange, Verarbeitung läuft, Abgebrochen, Abgeschlossen oder Fehlgeschlagen.

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

Der Status-Endpunkt antwortet und gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Sobald der Auftrag `status` in „Abgeschlossen“ geändert wurde, kann er heruntergeladen werden.

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

## Daten abrufen

Um die Datei eines abgeschlossenen benutzerdefinierten Objektexports abzurufen, rufen Sie einfach den Endpunkt [Benutzerdefinierte Objektdatei exportieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) mit Ihrem `apiName` und Ihrer `exportId` auf.

Die Antwort enthält eine -Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der -Datei. Wenn ein angefordertes benutzerdefiniertes Objektattribut leer ist (keine Daten enthält), wird `null` in das entsprechende Feld in der Exportdatei eingefügt.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Um das partielle und fortsetzungsfreundliche Abrufen extrahierter Daten zu unterstützen, unterstützt der Datei-Endpunkt optional den HTTP-Header-Bereich vom Typ Byte. Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Weitere Informationen zur Verwendung der Kopfzeile „Bereich“ finden Sie in Marketo [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem Endpunkt [Export eines benutzerdefinierten Objektauftrags abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST) abgebrochen werden. Dies antwortet mit einer `status`, die angibt, dass der Vorgang abgebrochen wurde.

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
