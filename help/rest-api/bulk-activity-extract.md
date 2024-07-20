---
title: Extrahieren einer Massenaktivität
feature: REST API
description: Stapelverarbeitung von Aktivitätsdaten aus Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1381'
ht-degree: 7%

---

# Extrahieren einer Massenaktivität

[Referenz zum Endpunkt der Massenaktivität extrahieren](https://developer.adobe.com/marketo-apis/api/mapi/)

Der Satz aus REST-APIs zur Massenaktivität-Extrahierung bietet eine programmatische Oberfläche zum Abrufen großer Mengen von Aktivitätsdaten aus Marketo.  Für Fälle, die keine niedrige Latenz erfordern und erhebliche Mengen von Aktivitätsdaten aus Marketo übertragen müssen, z. B. CRM-Integration, ETL, Data Warehouse und Datenarchivierung.

## Berechtigungen

Für die Massen-Aktivitäts-Extract-APIs ist es erforderlich, dass der API-Benutzer über die Berechtigungen &quot;Schreibgeschützte Aktivität&quot;oder &quot;Lese- und Schreibaktivität&quot;verfügt.

## Filter

<table>
  <tbody>
    <tr>
      <td>Filtertyp</td>
      <td>Datentyp</td>
      <td>Erforderlich</td>
      <td>Hinweise</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>Datumsbereich</td>
      <td>Ja</td>
      <td>Akzeptiert ein JSON-Objekt mit den Elementen startAt und endAt. startAt akzeptiert einen Datum, der das niedrige Wasserzeichen darstellt, und endAt akzeptiert einen Datum, der das hohe Wasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die innerhalb des Datumsbereichs erstellt wurden. Die Datetimes sollten im ISO-8601-Format (ohne Millisekunden) erstellt werden.</td>
    </tr>
    <tr>
      <td>activityTypeIds</td>
      <td>Array[Integer]</td>
      <td>Nein</td>
      <td>Akzeptiert ein JSON-Objekt mit einem Element, activityTypeIds. Der Wert muss ein Array von Ganzzahlen sein, das den gewünschten Aktivitätstypen entspricht. Die Aktivität "Lead löschen"wird nicht unterstützt (verwenden Sie stattdessen den Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET">Gelöschte Leads abrufen</a>). Rufen Sie Aktivitätstypen-IDs mithilfe des Endpunkts <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET">Aktivitätstypen abrufen</a>ab.</td>
    </tr>
    <tr>
      <td>primaryAttributeValueIds</td>
      <td>Array[Integer]</td>
      <td>Nein</td>
      <td>Akzeptiert ein JSON-Objekt mit einem Member, primaryAttributeValueIds. Der Wert ist ein Array von IDs, die die primären Attribute angeben, nach denen gefiltert werden soll. Es können maximal 50 IDs angegeben werden. Die IDs sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität "Formular ausfüllen"zu filtern, übergeben Sie den Formularnamen an den Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Formular nach Name abrufen</a> , um die Formular-ID abzurufen. Im Folgenden finden Sie eine Liste von Aktivitätstypen, für die die primäre Attributfilterung unterstützt wird.
        <table>
          <tbody>
            <tr>
              <td>Aktivitätstyp</td>
              <td>Primäre Attributwert-ID</td>
              <td>Abrufen-Endpunkt</td>
              <td>Asset-Gruppe</td>
            </tr>
            <tr>
              <td>Datenwert ändern</td>
              <td>Lead-Feld-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschreiben</a></td>
              <td>Attributname</td>
            </tr>
            <tr>
              <td>Bewertung ändern</td>
              <td>Lead-Feld-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschreiben</a></td>
              <td>Attributname</td>
            </tr>
            <tr>
              <td>Status in Entwicklung ändern</td>
              <td>Programm-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET">Programm nach Namen abrufen</a></td>
              <td>Marketingprogramm</td>
            </tr>
            <tr>
              <td>Zu Liste hinzufügen</td>
              <td>Statische Listen-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Abrufen einer statischen Liste nach Name</a></td>
              <td>Statische Liste</td>
            </tr>
            <tr>
              <td>Aus Liste entfernen</td>
              <td>Statische Listen-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Abrufen einer statischen Liste nach Name</a></td>
              <td>Statische Liste</td>
            </tr>
            <tr>
              <td>Formular ausfüllen</td>
              <td>Formular-ID</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Formular nach Namen abrufen</a></td>
              <td>Webformular</td>
            </tr>
          </tbody>
        </table>
        Bei Verwendung von primaryAttributeValueIds muss der Filter activityTypeIds vorhanden sein und darf nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen.Beispiel:Wenn Sie beispielsweise nach Webformular-Assets filtern, ist nur die Aktivitätstyp-ID "Formular ausfüllen"in activityTypeIds.Beispiel-Anfragetext:{"filter":{"createdAt":{"startAt": "22211": -07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValueIds" : [11 6,102,95,8]}}primaryAttributeValueIds und primaryAttributeValues können nicht zusammen verwendet werden.</td>
    </tr>
    <tr>
      <td>primaryAttributeValues</td>
      <td>Array[String]</td>
      <td>Nein</td>
      <td>Akzeptiert ein JSON-Objekt mit einem Member, primaryAttributeValues. Der Wert ist ein Array von Namen, die die primären Attribute angeben, nach denen gefiltert werden soll. Es können maximal 50 Namen angegeben werden. Die Namen sind die eindeutige Kennung für ein Lead-Feld oder ein Asset und können durch Aufruf des entsprechenden REST-API-Endpunkts abgerufen werden. Um beispielsweise nach einem bestimmten Formular für die Aktivität "Formular ausfüllen"zu filtern, übergeben Sie die Formular-ID an den Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Formular mit ID abrufen</a> , um den Formularnamen abzurufen. Im Folgenden finden Sie eine Liste von Aktivitätstypen, für die die primäre Attributfilterung unterstützt wird.
        <table>
          <tbody>
            <tr>
              <td>Aktivitätstyp</td>
              <td>Primärer Attributwert</td>
              <td>Abrufen-Endpunkt</td>
              <td>Asset-Gruppe</td>
            </tr>
            <tr>
              <td>Datenwert ändern</td>
              <td>Lead-Feld displayName</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschreiben</a></td>
              <td>Attributname</td>
            </tr>
            <tr>
              <td>Bewertung ändern</td>
              <td>Lead-Feld displayName</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschreiben</a></td>
              <td>Attributname</td>
            </tr>
            <tr>
              <td>Status in Entwicklung ändern</td>
              <td>Programmname</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET">Programm nach ID abrufen</a></td>
              <td>Marketingprogramm</td>
            </tr>
            <tr>
              <td>Zu Liste hinzufügen</td>
              <td>Statischer Listenname</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Abrufen einer statischen Liste nach ID</a></td>
              <td>Statische Liste</td>
            </tr>
            <tr>
              <td>Aus Liste entfernen</td>
              <td>Statischer Listenname</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Abrufen einer statischen Liste nach ID</a></td>
              <td>Statische Liste</td>
            </tr>
            <tr>
              <td>Formular ausfüllen</td>
              <td>Formularname</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Formular nach ID abrufen</a></td>
              <td>Webformular</td>
            </tr>
          </tbody>
        </table>
        Beachten Sie, dass Sie "&lt;<em>program</em>&gt;"verwenden müssen.&lt;<em>asset</em>&gt;"-Notation, um den Namen für die folgenden Asset-Gruppen eindeutig anzugeben: Marketing-Programm, statische Liste, Web-Formular.Beispiel: Ein Formular mit dem Namen "MPS Outbound", das sich unter dem Programm mit dem Namen "GL_OP_ALL_2021" befindet, würde beispielsweise als "GL_OP_ALL_2021.MPS Outbound"angegeben. Beispielanfragetext:{"filter":{"createdAt":{"startAt": "2021-07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:0 0"},"activityTypeIds":[2],"primaryAttributeValues":["GL_OP_ALL_2021.MPS Outbound"]}}Bei Verwendung von primaryAttributeValues muss der Filter activityTypeIds vorhanden sein und nur Aktivitäts-IDs enthalten, die mit der entsprechenden Asset-Gruppe übereinstimmen. Wenn Sie beispielsweise nach Webformular-Assets filtern, ist nur die Aktivitätstyp-ID "Formular ausfüllen"in activityTypeIds.primaryAttributeValues und primaryAttributeValueIds zulässig.</td>
    </tr>
  </tbody>
</table>

## Optionen

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| Filter | Array[Object] | Ja | Akzeptiert ein Filterarray. Genau ein createdAt -Filter muss im Array enthalten sein. Es kann ein optionaler activityTypeIds -Filter verwendet werden. Die Filter werden auf den verfügbaren Aktivitätensatz angewendet und der daraus resultierende Aktivitätensatz wird vom Exportauftrag zurückgegeben. |
| format | Zeichenfolge | Nein | Akzeptiert eines von: CSV, TSV, SSV Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder als Datei mit durch Leerzeichen getrennten Werten gerendert, wenn set.Defaults auf CSV gesetzt, wenn nicht festgelegt. |
| columnHeaderName | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenüberschriftsnamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportauftrag enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Felder | Array[String] | Nein | Optionales Array von Zeichenfolgen mit Feldwerten. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Standardmäßig werden die folgenden Felder zurückgegeben: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`. Dieser Parameter kann verwendet werden, um die Anzahl der zurückgegebenen Felder zu reduzieren, indem eine Teilmenge aus der obigen Liste angegeben wird. Beispiel:&quot;fields&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Ein zusätzliches Feld &quot;actionResult t&quot;kann angegeben werden, um die Aktivitätsaktion (&quot;erfolgreich&quot;, &quot;übersprungen&quot;oder &quot;fehlgeschlagen&quot;) einzuschließen. |


## Erstellen eines Auftrags

Um Datensätze zu exportieren, müssen Sie zunächst den Auftrag und die Datensätze definieren, die Sie abrufen möchten.  Erstellen Sie den Auftrag mithilfe des Endpunkts [Exportaktivitätsauftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) .  Beim Exportieren von Aktivitäten können zwei Hauptfilter angewendet werden: `createdAt`, was immer erforderlich ist, und `activityTypeIds`, was optional ist.  Der Filter createdAt wird verwendet, um einen Datumsbereich zu definieren, in dem Aktivitäten mithilfe der Parameter `startAt` und `endAt` erstellt wurden. Beide sind Datumszeitfelder und stellen das frühestmögliche Erstellungsdatum bzw. das neueste zulässige Erstellungsdatum dar.  Sie können auch optional nur nach bestimmten Aktivitätstypen filtern, indem Sie den Filter `activityTypeIds` verwenden.  Dies ist nützlich, um Ergebnisse zu entfernen, die für Ihren Anwendungsfall nicht relevant sind.

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

Der Auftrag hat jetzt den Status &quot;Erstellt&quot;, befindet sich aber noch nicht in der Verarbeitungswarteschlange.  Um ihn in die Warteschlange zu stellen und die Verarbeitung zu starten, müssen wir den Endpunkt [Export Activity Job-Warteschlange einschließen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) mit der exportId aus der Erstellungsstatusantwort aufrufen.

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

Jetzt meldet der Status, dass der Auftrag in die Warteschlange gestellt wurde.  Wenn ein Worker für diesen Auftrag verfügbar wird, wechselt der Status zu &quot;Verarbeitung&quot;, und der Auftrag beginnt mit der Aggregation von Datensätzen aus Marketo.

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
