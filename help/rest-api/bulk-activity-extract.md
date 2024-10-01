---
title: Extrahieren einer Massenaktivität
feature: REST API
description: Stapelverarbeitung von Aktivitätsdaten aus Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 8c22255673fee1aa0f5b47393a241fcf6680778b
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 7%

---

# Extrahieren einer Massenaktivität

[Referenz zum Endpunkt der Massenaktivität extrahieren](https://developer.adobe.com/marketo-apis/api/mapi/)

Der Satz aus REST-APIs zur Massenaktivität-Extrahierung bietet eine programmatische Oberfläche zum Abrufen großer Mengen von Aktivitätsdaten aus Marketo.  Für Fälle, bei denen keine niedrige Latenz erforderlich ist und erhebliche Mengen von Aktivitätsdaten aus Marketo übertragen werden müssen, z. B. CRM-Integration, ETL, Data Warehouse und Datenarchivierung.

## Berechtigungen

Für die Massen-Aktivitäts-Extract-APIs ist es erforderlich, dass der API-Benutzer über die Berechtigungen &quot;Schreibgeschützte Aktivität&quot;oder &quot;Lese- und Schreibaktivität&quot;verfügt.

## Filter

| Filtertyp | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| createdAt | Datumsbereich | Ja | Akzeptiert ein JSON-Objekt mit den Mitgliedern `startAt` und `endAt`. `startAt` akzeptiert einen Datum, der das Wasserzeichen mit niedrigem Wasserzeichen darstellt, und `endAt` akzeptiert einen Datum, der das Wasserzeichen mit hohem Wasserzeichen darstellt. Der Zeitraum muss 31 Tage oder weniger betragen. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die innerhalb des Datumsbereichs erstellt wurden. Die Datenzeiten sollten im ISO-8601-Format vorliegen, ohne Millisekunden. |
| activityTypeIds | Array\[Integer\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `activityTypeIds`. Der Wert muss ein Array von Ganzzahlen sein, das den gewünschten Aktivitätstypen entspricht. Die Aktivität &quot;Lead löschen&quot;wird nicht unterstützt (verwenden Sie stattdessen den Endpunkt [Gelöschte Leads abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) ). Rufen Sie Aktivitätstyp-IDs mit dem Endpunkt [Aktivitätstypen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) ab. |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Array\[Integer\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `primaryAttributeValueIds`. Der Wert ist ein Array von IDs, die die primären Attribute angeben, nach denen gefiltert werden soll. Es können maximal 50 IDs angegeben werden. Die IDs sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität &quot;Formular ausfüllen&quot;zu filtern, übergeben Sie den Namen des Formulars an den Endpunkt [Formular nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) , um die Formular-ID abzurufen. Im Folgenden finden Sie eine Liste von Aktivitätstypen, für die die primäre Attributfilterung unterstützt wird. |
| [primaryAttributeValues](#primaryattributevalues-options) | Array\[String\] | Nein | Akzeptiert ein JSON-Objekt mit einem Element, `primaryAttributeValues`. Der Wert ist ein Array von Namen, die die primären Attribute angeben, nach denen gefiltert werden soll. Es können maximal 50 Namen angegeben werden. Die Namen sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität &quot;Formular ausfüllen&quot;zu filtern, übergeben Sie die Formular-ID an den Endpunkt [Formular mit ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) , um den Formularnamen abzurufen. Im Folgenden finden Sie eine Liste von Aktivitätstypen, für die die primäre Attributfilterung unterstützt wird. |

### primaryAttributeValueIds options {#primaryattributevalueids-options}

| Aktivitätstyp | Primäre Attributwert-ID | Abrufen-Endpunkt | Asset-Gruppe |
| --- | --- | --- | --- |
| Datenwert ändern | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Bewertung ändern | Lead-Feld-ID | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programm-ID | [Programm nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Marketingprogramm |
| Zu Liste hinzufügen | Statische Listen-ID | [Abrufen einer statischen Liste nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Aus Liste entfernen | Statische Listen-ID | [Abrufen einer statischen Liste nach Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische Liste |
| Formular ausfüllen | Formular-ID | [Formular nach Name abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Webformular |

Bei Verwendung von `primaryAttributeValueIds` muss der Filter `activityTypeIds` vorhanden sein und darf nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen. Wenn Sie beispielsweise nach Webformular-Assets filtern, ist in `activityTypeIds` nur die Aktivitätstyp-ID &quot;Formular ausfüllen&quot;zulässig.

Beispiel-Anfrageinhalt:

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

### primaryAttributeValues options {#primaryattributevalues-options}

| Aktivitätstyp | Primärer Attributwert | Abrufen-Endpunkt | Asset-Gruppe |
| --- | --- | --- | --- |
| Datenwert ändern | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Bewertung ändern | Lead-Feld displayName | [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Attributname |
| Status in Entwicklung ändern | Programmname | [Programm von ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Marketingprogramm |
| Zu Liste hinzufügen | Statischer Listenname | [Statische Liste mit ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Aus Liste entfernen | Statischer Listenname | [Statische Liste mit ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische Liste |
| Formular ausfüllen | Formularname | [Formular mit ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Webformular |

Beachten Sie, dass Sie &quot;&lt;<em>program</em>>&quot;verwenden müssen.&lt;<em>asset</em>>&quot;, um den Namen für die folgenden Asset-Gruppen anzugeben: Marketing-Programm, statische Liste, Webformular. Beispielsweise würde ein Formular mit dem Namen &quot;MPS Outbound&quot;, das sich unter einem Programm mit dem Namen &quot;GL_OP_ALL_2021&quot;befindet, als &quot;GL_OP_ALL_2021.MPS Outbound&quot;angegeben.

Beispiel-Anfrageinhalt:

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

Bei Verwendung von `primaryAttributeValues` muss der Filter `activityTypeIds` vorhanden sein und darf nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen. Wenn Sie beispielsweise nach Webformular-Assets filtern, ist in `activityTypeIds` nur die Aktivitätstyp-ID &quot;Formular ausfüllen&quot;zulässig. `primaryAttributeValues` und `primaryAttributeValueIds` können nicht zusammen verwendet werden.

## Optionen

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| filter | Array[Object] | Ja | Akzeptiert ein Filterarray. Genau ein `createdAt` -Filter muss im Array enthalten sein. Ein optionaler `activityTypeIds` -Filter kann enthalten sein. Die Filter werden auf den verfügbaren Aktivitätensatz angewendet und der resultierende Aktivitätensatz wird vom Exportauftrag zurückgegeben. |
| format | Zeichenfolge | Nein | Akzeptiert eine der folgenden Optionen: CSV, TSV, SSV Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertdatei gerendert, sofern festgelegt. Wenn nicht festgelegt, wird standardmäßig CSV verwendet. |
| columnHeaderName | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenüberschriftsnamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportauftrag enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Felder | Array[String] | Nein | Optionales Array von Zeichenfolgen mit Feldwerten. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Standardmäßig werden die folgenden Felder zurückgegeben: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`. Dieser Parameter kann verwendet werden, um die Anzahl der zurückgegebenen Felder zu reduzieren, indem eine Teilmenge aus der obigen Liste angegeben wird. Beispiel:&quot;fields&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Ein zusätzliches Feld &quot;actionResult&quot; kann angegeben werden, um die Aktivitätsaktion (&quot;succeeded&quot;, &quot;skipped&quot; oder &quot;failed&quot;) einzuschließen. |


## Erstellen eines Auftrags

Um Datensätze zu exportieren, müssen Sie zunächst den Auftrag und die Datensätze definieren, die Sie abrufen möchten.  Erstellen Sie den Auftrag mithilfe des Endpunkts [Exportaktivitätsauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) .  Beim Exportieren von Aktivitäten können zwei Hauptfilter angewendet werden: `createdAt`, was immer erforderlich ist, und `activityTypeIds`, was optional ist.  Der Filter `createdAt` wird verwendet, um einen Datumsbereich zu definieren, in dem Aktivitäten mithilfe der Parameter `startAt` und `endAt` erstellt wurden. Beide sind Datumszeitfelder und stellen das frühestmögliche Erstellungsdatum bzw. das neueste zulässige Erstellungsdatum dar.  Sie können auch optional nur nach bestimmten Aktivitätstypen filtern, indem Sie den Filter `activityTypeIds` verwenden.  Dies ist nützlich, um Ergebnisse zu entfernen, die für Ihren Anwendungsfall nicht relevant sind.

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

Der Auftrag hat jetzt den Status &quot;Erstellt&quot;, befindet sich aber noch nicht in der Verarbeitungswarteschlange.  Um ihn in die Warteschlange zu stellen, damit er mit der Verarbeitung beginnen kann, rufen Sie den Endpunkt [Export Activity Job-Warteschlange einschließen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) mit der exportId aus der Erstellungsstatusantwort auf.

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

Jetzt meldet der Status, dass der Auftrag in die Warteschlange gestellt wurde.  Wenn ein Worker für diesen Auftrag verfügbar wird, wechselt der Status zu &quot;Verarbeitung&quot;, und der Auftrag beginnt, Datensätze aus Marketo zu aggregieren.

## Abruf-Auftragsstatus

Der Auftragsstatus kann nur für Aufträge abgerufen werden, die von demselben API-Benutzer erstellt wurden.

Die Massen-Aktivitätsextraktion von Marketo ist ein asynchroner Endpunkt. Daher muss der Auftragsstatus abgerufen werden, um zu bestimmen, wann der Auftrag abgeschlossen ist.  Umfrage mit dem Endpunkt [Export Activity Job Status abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) wie folgt:

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

Das Statusfeld kann mit einem der folgenden Werte reagieren:

- Erstellt
- In Warteschl. versch
- Verarbeitung läuft
- Abgebrochen
- Abgeschlossen
- Fehlgeschlagen

## Abrufen Ihrer Daten

Rufen Sie nach Abschluss des Auftrags Ihre Daten mit dem Endpunkt [Exportaktivitätsdatei abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) ab.

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

Die Antwort enthält eine Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der Datei.

Wenn ein angefordertes Lead-Feld leer ist (keine Daten enthält), wird `then null` in das entsprechende Feld in der Exportdatei eingefügt.  Im folgenden Beispiel ist das Feld campaignId für die zurückgegebene Aktivität leer.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Um das teilweise und wiederverwendbare Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional den HTTP-Header `Range` des Typs `bytes`.  Wenn der Header nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben.  Weitere Informationen zur Verwendung der Bereichskopfzeile mit Marketo [Massenextraktion](bulk-extract.md) finden Sie.

## Abbruch eines Auftrags

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach über den Endpunkt [Exportaktivitätsauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) abgebrochen werden:

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

Diese Antwort weist darauf hin, dass der Auftrag abgebrochen wurde.
