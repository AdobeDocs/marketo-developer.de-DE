---
title: Massenaktivität-Extrakt
feature: REST API
description: Marketo Bulk Activity Extract REST-API zum Exportieren von Aktivitätsdaten mit hohem Volumen unter Verwendung eines 31-tägigen Datumsbereichs, Aktivität und primären Attributfiltern für ETL und CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
TQID: https://experienceleague.adobe.com/lIlXNjatN-F77Dv3xsVkQ3hAWwLZ4wlSW0zKNkFJFMA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1268
ht-degree: 7%

---

# Massenaktivität-Extrakt

[Referenz für Massenaktivität-Extraktionsendpunkt](https://developer.adobe.com/marketo-apis/api/mapi)

Die REST-APIs für die Massenaktivität-Extraktion rufen große Mengen von Aktivitätsdaten aus Marketo ab. Verwenden Sie diese APIs für Prozesse, für die keine niedrige Latenz erforderlich ist, z. B. CRM-Integration, ETL, Data Warehousing und Datenarchivierung.

## Berechtigungen

Der API-Benutzer muss über die Berechtigung „Schreibgeschützte Aktivität“ oder „Lese-/Schreibaktivität“ verfügen.

## Filter

| Filtertyp | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| `createdAt` | Datumsbereich | Ja | Ein JSON-Objekt, das `startAt` und `endAt` enthält. `startAt` ist die Datetime mit niedrigem Wasserzeichen und `endAt` die Datetime mit hohem Wasserzeichen. Der Bereich muss 31 Tage oder weniger betragen. Der Auftrag gibt alle Datensätze zurück, auf die innerhalb des Datumsbereichs zugegriffen werden kann. Verwenden Sie ISO-8601-Datetime-Werte ohne Millisekunden. |
| `activityTypeIds` | Array\[Ganzzahl\] | Nein | Ein Array von Ganzzahlen für die angeforderten Aktivitätstypen. „Lead löschen“ wird nicht unterstützt. Verwenden Sie stattdessen den Endpunkt [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Rufen Sie Aktivitätstyp-IDs mit dem Endpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) ab. |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Array\[Ganzzahl\] | Nein | Ein Array, das maximal 50 IDs für primäre Attribute akzeptiert. Jede ID identifiziert ein Lead-Feld oder Asset eindeutig. Rufen Sie IDs durch Aufrufen des entsprechenden REST-API-Endpunkts ab. Um beispielsweise nach einem bestimmten Formular für die Aktivität „Formular ausfüllen“ zu filtern, übergeben Sie den Formularnamen an den Endpunkt [Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) um die Formular-ID abzurufen. Siehe [primaryAttributeValueIds-Optionen](#primaryattributevalueids-options) für unterstützte Aktivitätstypen. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Array\[String\] | Nein | Ein Array, das maximal 50 Namen für primäre Attribute akzeptiert. Jeder Name identifiziert ein Lead-Feld oder Asset eindeutig. Rufen Sie Namen ab, indem Sie den entsprechenden REST-API-Endpunkt aufrufen. Um beispielsweise nach einem bestimmten Formular für die Aktivität „Formular ausfüllen“ zu filtern, übergeben Sie die Formular-ID an den Endpunkt [Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) um den Formularnamen abzurufen. Siehe [primaryAttributeValues](#primaryattributevalues-options) für unterstützte Aktivitätstypen. |

### primaryAttributeValueIds-Optionen {#primaryattributevalueids-options}

| Aktivitätstyp | Primäre Attributwert-ID | Retrieval-Endpunkt | Asset-Gruppe |
| --- | --- | --- | --- |
| Datenwert ändern | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Ändern von Bewertung | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programm-ID | [Programm nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Marketingprogramm |
| Hinzufügen zur Liste | Statische Listen-ID | [Statische Liste nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Entfernen aus Liste | Statische Listen-ID | [Statische Liste nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Formular ausfüllen | Formular-ID | [Formular nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Webformular |

Wenn Sie `primaryAttributeValueIds` verwenden, müssen Sie auch den `activityTypeIds` Filter einbeziehen. Dieser Filter kann nur Aktivitäts-IDs enthalten, die der entsprechenden Asset-Gruppe entsprechen. Wenn Sie beispielsweise Web-Formular-Assets filtern, können `activityTypeIds` nur die Aktivitätstyp-ID „Formular ausfüllen“ enthalten.

Die folgende Anfrage enthält den `primaryAttributeValueIds`:

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
| Datenwert ändern | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Ändern von Bewertung | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programmname | [Programm nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) | Marketingprogramm |
| Hinzufügen zur Liste | Name der statischen Liste | [Statische Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Entfernen aus Liste | Name der statischen Liste | [Statische Liste nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Formular ausfüllen | Formularname | [Formular nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) | Webformular |

Verwenden Sie `&lt;program&gt;.&lt;asset&gt;` Notation, um Namen für die Asset-Gruppen des Marketing-Programms, der statischen Liste und des Web-Formulars anzugeben. Geben Sie beispielsweise das Formular „MPS Outbound“ im Programm „GL_OP_ALL_2021“ als „GL_OP_ALL_2021.MPS Outbound“ an.

Die folgende Anfrage enthält den `primaryAttributeValues`:

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

Wenn Sie `primaryAttributeValues` verwenden, müssen Sie auch den `activityTypeIds` Filter einbeziehen. Dieser Filter kann nur Aktivitäts-IDs enthalten, die der entsprechenden Asset-Gruppe entsprechen. Wenn Sie beispielsweise Web-Formular-Assets filtern, können `activityTypeIds` nur die Aktivitätstyp-ID „Formular ausfüllen“ enthalten.

`primaryAttributeValues` und `primaryAttributeValueIds` können nicht zusammen verwendet werden.

## Optionen

| Parameter | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| `filter` | Objekt | Ja | Ein Objekt, das Filter enthält, die für den barrierefreien Aktivitätssatz gelten. Schließen Sie genau einen `createdAt` ein. Sie können auch einen `activityTypeIds` Filter einschließen. Der Exportvorgang gibt die resultierende Gruppe von Aktivitäten zurück. |
| `format` | Zeichenfolge | Nein | Das Exportdateiformat: CSV, TSV oder SSV. Diese Werte erzeugen kommagetrennte, tabulatorgetrennte bzw. raumgetrennte Werte. Der Standardwert ist CSV. |
| `columnHeaderNames` | Objekt | Nein | Ein JSON-Objekt mit Schlüssel-Wert-Paaren für Felder und Spaltenüberschriften. Jeder Schlüssel muss ein Feld benennen, das im Exportvorgang enthalten ist. Sein Wert legt die exportierte Spaltenüberschrift für dieses Feld fest. |
| `fields` | Array\[String\] | Nein | Ein Array von Feldern, die in die Exportdatei aufgenommen werden sollen. Standardmäßig umfasst die Antwort `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` und `attributes`. Um eine Teilmenge zurückzugeben, geben Sie Felder aus dieser Liste an, z. B. `"fields": ["leadId", "activityDate", "activityTypeId"]`. Sie können auch angeben, `actionResult` die Aktivitätsaktion einbezogen werden soll: `("succeeded", "skipped", or "failed")`. |

## Erstellen von Aufträgen

Erstellen Sie einen Exportvorgang, um die abzurufenden Datensätze zu definieren. Verwenden [&#x200B; Endpunkts „Exportaktivitätsauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).

Für jeden Auftrag ist ein `createdAt` erforderlich. Die Parameter `startAt` und `endAt` Datum/Uhrzeit definieren die frühesten und letzten zulässigen Erstellungsdaten für Aktivitäten. Um nicht relevante Aktivitätstypen auszuschließen, schließen Sie auch den optionalen `activityTypeIds` ein.

Die folgende Anfrage erstellt einen CSV-Exportvorgang für ausgewählte Aktivitätstypen innerhalb eines Datumsbereichs:

```http
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

Die Antwort gibt einen `exportId` und den Status „Erstellt“ zurück. Ein erstellter Auftrag befindet sich noch nicht in der Verarbeitungswarteschlange.

Um den Auftrag zur Warteschlange hinzuzufügen, rufen Sie den Endpunkt [Exportaktivitätsauftrag in die Warteschlange einreihen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) mit dem `exportId` aus der Erstellungsantwort auf.

```http
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

Der Antwortstatus ist jetzt „In Warteschlange“. Wenn ein Worker verfügbar ist, ändert sich der Status in „Verarbeitung läuft“ und der Vorgang beginnt mit dem Aggregieren von Datensätzen aus Marketo.

## Status des Abrufauftrags

Der Auftragsstatus kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Bulk Activity Extract verarbeitet Aufträge asynchron. Abfrage des Endpunkts [Exportaktivitätsstatus abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) um festzustellen, wann ein Auftrag abgeschlossen ist:

```http
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

Das Feld `status` gibt einen der folgenden Werte zurück:

- `Created`
- `Queued`
- `Processing`
- `Canceled`
- `Completed`
- `Failed`

## Daten abrufen

Wenn der Auftragsstatus „Abgeschlossen“ ist, rufen Sie die exportierten Daten mit dem Endpunkt [Exportaktivitätsdatei abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) ab:

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

Der Antworttext enthält die Datei im für den Auftrag konfigurierten Format.

Wenn ein angefordertes Aktivitätsfeld keine Daten enthält, wird `null` im entsprechenden Exportdatei-Feld angezeigt. Das folgende Beispiel zeigt exportierte Aktivitätsdaten:

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Beim teilweisen oder wiederaufnehmbaren Abrufen unterstützt der Datei-Endpunkt die optionale HTTP-`Range`-Kopfzeile mit einem `bytes`. Wenn Sie diese Kopfzeile auslassen, gibt der Endpunkt die gesamte Datei zurück. Weitere Informationen zur Verwendung des `Range`-Headers finden Sie unter [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Um einen falsch konfigurierten oder unnötigen Auftrag zu stoppen, rufen Sie den Endpunkt [Exportaktivitätsauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) auf:

```http
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

Der Antwortstatus gibt an, dass der Vorgang abgebrochen wurde.
