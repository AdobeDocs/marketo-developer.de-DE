---
title: Massenaktivität-Extrakt
feature: REST API
description: Marketo Bulk Activity Extract REST-API zum Exportieren von Aktivitätsdaten mit hohem Volumen unter Verwendung eines 31-tägigen Datumsbereichs, Aktivität und primären Attributfiltern für ETL und CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 7%

---

# Massenaktivität-Extrakt

[Referenz für Massenaktivität-Extraktionsendpunkt](https://developer.adobe.com/marketo-apis/api/mapi/)

Der Satz von REST-APIs für die Massenaktivität-Extraktion bietet eine programmgesteuerte Schnittstelle zum Abrufen großer Mengen von Aktivitätsdaten aus Marketo.  Für Fälle, in denen keine niedrige Latenz erforderlich ist und erhebliche Mengen von Aktivitätsdaten aus Marketo übertragen werden müssen, z. B. CRM-Integration, ETL, Data Warehousing und Datenarchivierung.

## Berechtigungen

Die APIs zum Extrahieren von Massenaktivitäten erfordern, dass der API-Benutzer über die Berechtigungen „Schreibgeschützte Aktivität“ oder „Lese-/Schreibaktivität“ verfügt.

## Filter

| Filtertyp | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| createdAt | Datumsbereich | Ja | Akzeptiert ein JSON-Objekt mit den `startAt` und `endAt`. `startAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und `endAt` akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die innerhalb des Datumsbereichs zugegriffen werden kann. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden. |
| activityTypeIds | Array\[Ganzzahl\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `activityTypeIds`. Der Wert muss ein Array von Ganzzahlen sein, das den gewünschten Aktivitätstypen entspricht. Die Aktivität „Lead löschen“ wird nicht unterstützt (stattdessen den Endpunkt [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) verwenden). Rufen Sie Aktivitätstyp-IDs mithilfe des Endpunkts [Aktivitätstypen abrufen“ ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Array\[Ganzzahl\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `primaryAttributeValueIds`. Der Wert ist ein Array von IDs, die die primären Attribute angeben, nach denen gefiltert werden soll. Es können maximal 50 IDs angegeben werden. Die IDs sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität „Formular ausfüllen“ zu filtern, übergeben Sie den Formularnamen an den Endpunkt [Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) um die Formular-ID abzurufen. Im Folgenden finden Sie eine Liste der Aktivitätstypen, bei denen die Filterung primärer Attribute unterstützt wird. |
| [primaryAttributeValues](#primaryattributevalues-options) | Array\[String\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `primaryAttributeValues`. Der Wert ist ein Array von Namen, die die primären Attribute zum Filtern angeben. Es können maximal 50 Namen angegeben werden. Die Namen sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität „Formular ausfüllen“ zu filtern, übergeben Sie die Formular-ID an den Endpunkt [Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) um den Formularnamen abzurufen. Im Folgenden finden Sie eine Liste der Aktivitätstypen, bei denen die Filterung primärer Attribute unterstützt wird. |

### primaryAttributeValueIds-Optionen {#primaryattributevalueids-options}

| Aktivitätstyp | Primäre Attributwert-ID | Retrieval-Endpunkt | Asset-Gruppe |
| --- | --- | --- | --- |
| Datenwert ändern | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Ändern von Bewertung | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programm-ID | [Programm nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Marketingprogramm |
| Hinzufügen zur Liste | Statische Listen-ID | [Statische Liste nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Entfernen aus Liste | Statische Listen-ID | [Statische Liste nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Formular ausfüllen | Formular-ID | [Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Webformular |

Bei Verwendung von `primaryAttributeValueIds` muss der `activityTypeIds` vorhanden sein und darf nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen. Wenn Sie beispielsweise nach Web-Formular-Assets filtern, ist in `activityTypeIds` nur die Aktivitätstyp-ID „Formular ausfüllen“ zulässig.

Beispiel-Anfragetext:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

`primaryAttributeValueIds` und `primaryAttributeValues` können nicht zusammen verwendet werden.

### primaryAttributeValues-Optionen {#primaryattributevalues-options}

| Aktivitätstyp | Primärer Attributwert | Retrieval-Endpunkt | Asset-Gruppe |
| --- | --- | --- | --- |
| Datenwert ändern | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Ändern von Bewertung | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programmname | [Programm nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Marketingprogramm |
| Hinzufügen zur Liste | Name der statischen Liste | [Statische Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Entfernen aus Liste | Name der statischen Liste | [Statische Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Formular ausfüllen | Formularname | [Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Webformular |

Beachten Sie, dass Sie &quot;&lt;<em>program</em>> verwenden müssen.&lt;<em>asset</em>>&quot; verwenden, um den Namen für die folgenden Asset-Gruppen anzugeben: Marketing-Programm, statische Liste, Web-Formular. Beispielsweise würde ein Formular mit dem Namen „MPS Outbound“, das sich unter einem Programm mit dem Namen „GL_OP_ALL_2021“ befindet, als „GL_OP_ALL_2021.MPS Outbound“ angegeben.

Beispiel-Anfragetext:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Bei Verwendung von `primaryAttributeValues` muss der `activityTypeIds` vorhanden sein und darf nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen. Wenn Sie beispielsweise nach Web-Formular-Assets filtern, ist in `activityTypeIds` nur die Aktivitätstyp-ID „Formular ausfüllen“ zulässig. `primaryAttributeValues` und `primaryAttributeValueIds` können nicht zusammen verwendet werden.

## Optionen

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| filter | Array[Objekt] | Ja | Akzeptiert ein Filterarray. Es muss genau ein `createdAt` Filter im Array enthalten sein. Ein optionaler `activityTypeIds` kann enthalten sein. Die Filter werden auf den barrierefreien Aktivitätssatz angewendet, und der resultierende Aktivitätssatz wird vom Exportvorgang zurückgegeben. |
| Format | Zeichenfolge | Nein | Akzeptiert eine der folgenden Dateien: CSV, TSV, SSV Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertedatei gerendert, falls festgelegt. Die Standardeinstellung ist CSV, wenn nicht festgelegt. |
| columnHeaderNames | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenkopfzeilennamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportvorgang enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Felder | array[string] | Nein | Optionales Zeichenfolgen-Array, das Feldwerte enthält. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Standardmäßig werden die folgenden Felder zurückgegeben: <ul><li>`marketoGUIDleadId`</li><li> `activityDate` </li><li>`activityTypeId` </li><li>`campaignId`</li><li> `primaryAttributeValueId` </li><li>`primaryAttributeValue`</li><li> `attributes`</li></ul>. Dieser Parameter kann verwendet werden, um die Anzahl der zurückgegebenen Felder zu reduzieren, indem eine Teilmenge aus der obigen Liste angegeben wird:`"fields": ["leadId", "activityDate", "activityTypeId"]`. Sie können ein zusätzliches `actionResult` angeben, um die Aktivitätsaktion einzuschließen: `("succeeded", "skipped", or "failed")`. |

## Erstellen von Aufträgen

Um Datensätze zu exportieren, müssen Sie zunächst den Auftrag und die Menge der Datensätze definieren, die Sie abrufen möchten.  Erstellen Sie den Auftrag mit [ Endpunkt „Exportaktivitätsauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Beim Exportieren von Aktivitäten können zwei primäre Filter angewendet werden: `createdAt`, was immer erforderlich ist, und `activityTypeIds`, was optional ist.  Der `createdAt` Filter wird verwendet, um einen Datumsbereich zu definieren, in dem Aktivitäten erstellt wurden. Dabei werden die Parameter `startAt` und `endAt` verwendet, die beide Datums-/Uhrzeitfelder sind und das früheste zulässige Erstellungsdatum bzw. das späteste zulässige Erstellungsdatum darstellen.  Optional können Sie auch mit dem `activityTypeIds` Filter nur nach bestimmten Aktivitätstypen filtern.  Dies ist nützlich, um Ergebnisse zu entfernen, die für Ihren Anwendungsfall nicht relevant sind.

```
POST /bulk/v1/activities/export/create.json
```

```json
{
   "format": "CSV",
   "filter": {
      "createdAt": {
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [
         1,
         12,
         13
      ]
   }
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Der Auftrag hat jetzt den Status „Erstellt“, befindet sich jedoch noch nicht in der Verarbeitungswarteschlange.  Um ihn in die Warteschlange einzustellen, damit er mit der Verarbeitung beginnen kann, rufen Sie den Endpunkt [Exportaktivitätsauftrag in die Warteschlange](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) mit der exportId aus der Antwort auf den Erstellungsstatus auf.

```
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Jetzt meldet der Status, dass der Auftrag in die Warteschlange gestellt wurde.  Wenn ein Worker für diesen Auftrag verfügbar wird, wird der Status auf „Verarbeitung läuft“ geändert und der Vorgang beginnt, Datensätze aus Marketo zu aggregieren.

## Status des Abrufauftrags

Der Auftragsstatus kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Marketos Massenaktivität-Extraktion ist ein asynchroner Endpunkt, sodass der Auftragsstatus abgefragt werden muss, um zu bestimmen, wann der Auftrag abgeschlossen ist.  Abfrage mit dem Endpunkt [Exportaktivitätsstatus abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) wie folgt:

```
GET /bulk/v1/activities/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

Das Statusfeld kann mit einem der folgenden Werte antworten:

- Erstellt
- In Warteschl. versch
- Verarbeitung läuft
- Abgebrochen
- Abgeschlossen
- Fehlgeschlagen

## Daten abrufen

Sobald der Auftrag abgeschlossen ist, rufen Sie Ihre Daten mit dem Endpunkt [Exportaktivitätsdatei abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) ab.

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

Die Antwort enthält eine -Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der -Datei.

Wenn ein angefordertes Lead-Feld leer ist (keine Daten enthält), wird `then null` in der Exportdatei im entsprechenden Feld platziert.  Im folgenden Beispiel ist das `campaignId` Feld für die zurückgegebene Aktivität leer.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Um das teilweise und fortsetzungsfreundliche Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional die HTTP-Header-`Range` vom Typ `bytes`.  Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben.  Weitere Informationen zur Verwendung des Bereichs-Headers mit Marketo [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem Endpunkt [Exportaktivitätsauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) abgebrochen werden:

```
POST /bulk/v1/activities/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Diese Antwort weist einen Status auf, der angibt, dass der Vorgang abgebrochen wurde.
