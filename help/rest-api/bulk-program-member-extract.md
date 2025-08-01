---
title: Massenprogramm-Mitgliederextraktion
feature: REST API
description: Batch-Verarbeitung der Mitgliederdatenextraktion.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 4%

---

# Massenprogramm-Mitgliederextraktion

[Referenz zum Extrahieren von Endpunkten für Massenprogrammmember](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

Der Satz von REST-APIs zum Extrahieren von Programmmitgliedern stellt eine programmgesteuerte Schnittstelle zum Abrufen großer Mengen von Programmmitgliederdatensätzen aus Marketo bereit. Dies ist die empfohlene Oberfläche für Anwendungsfälle, in denen Daten aus Gründen der ETL, des Data Warehousing und der Archivierung kontinuierlich zwischen Marketo und einem oder mehreren externen Systemen ausgetauscht werden müssen.

## Berechtigungen

Die APIs zum Extrahieren von Programmmitgliedern erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der Berechtigungen Schreibgeschützter Lead oder Lese-/Schreib-Lead verfügt.

## beschreiben

[Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) ist die primäre Datenquelle für die Frage, ob Felder zur Verwendung verfügbar sind, und für Metadaten zu diesen Feldern. Das `name`-Attribut enthält den REST-API-Namen.

```
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Filter

Programmmitglieder unterstützen verschiedene Filteroptionen. Für einen Auftrag können mehrere Filtertypen angegeben werden. In diesem Fall werden sie zusammen mit ANDs verwendet. Sie müssen entweder den `programId` oder den `programIds` angeben. Alle anderen Filter sind optional. Der `updatedAt` erfordert zusätzliche Infrastrukturkomponenten, die noch nicht für alle Abonnements bereitgestellt wurden.

<table>
  <tbody>
    <tr>
      <td>Filtertyp</td>
      <td>Datentyp</td>
      <td>Hinweise</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Ganzzahl</td>
      <td>Akzeptiert die ID eines Programms. Aufträge geben alle zugänglichen Datensätze zurück, die zu dem Zeitpunkt Mitglieder des Programms sind, zu dem der Auftrag mit der Verarbeitung beginnt.Programm-IDs über den Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Programme abrufen</a>.Kann nicht mit dem Filter programIds verwendet werden.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Ganzzahl]</td>
      <td>Akzeptiert ein Array von bis zu 10 Programm-IDs. Jobs geben alle zugänglichen Datensätze zurück, die zu dem Zeitpunkt Mitglied der Programme sind, zu dem der Job mit der Verarbeitung beginnt. Ein zusätzliches Feld „programId“ wird der Exportdatei als erstes Feld hinzugefügt. In diesem Feld wird das Programm angegeben, aus dem ein Programmmitgliedschaftsdatensatz extrahiert wurde.Abrufen von Programm-IDs mit dem Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Programme abrufen</a>.Kann nicht mit dem programId-Filter verwendet werden.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Boolesch</td>
      <td>Akzeptiert einen booleschen Wert, der zum Filtern von Programmmitgliedschaftsdatensätzen für <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">Personen, die nicht mehr genügend Inhalt haben</a> verwendet wird.</td>
    </tr>
    <tr>
      <td>Kadenz des Nährwerts</td>
      <td>String</td>
      <td>Akzeptiert eine Zeichenfolge, mit der Programmmitgliedschaftsdatensätze nach einer bestimmten Pflegekadenz gefiltert werden. Zulässige Werte sind:
        <ul>
          <li>Pause - Kadenz wurde angehalten</li>
          <li>Norm - Kadenz ist normal</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[Zeichenfolge]</td>
      <td>Akzeptiert ein Array von Statusnamen der Programmmitglieder. Mehrere Statusnamen werden gemeinsam mit ODER bearbeitet. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und deren Programmmitgliedsstatus mit einem der angegebenen Statusnamen übereinstimmt. Es können sowohl standardmäßige als auch benutzerdefinierte Statusnamen verwendet werden. Wenn der Filter „statusNames“ mit dem Filter „programIds“ verwendet wird, wird jedes Programm auf Mitgliedschaftsdatensätze überprüft, deren Status mit einem der Statusnamen übereinstimmt. Wenn in keinem der Programme ein Statusname gefunden wird, wird der Fehler „1003, Invalid Data“ zurückgegeben.
        <table>
          <tbody>
            <tr>
              <td>Teilnahme</td>
              <td>Nach Bedarf teilgenommen</td>
              <td>Hardbounce aufgetreten</td>
            </tr>
            <tr>
              <td>Angeklickt</td>
              <td>Kontaktiert</td>
              <td>Konvertiert</td>
            </tr>
            <tr>
              <td>Eingebunden</td>
              <td>Ausgefülltes Formular</td>
              <td>Beeinflusst</td>
            </tr>
            <tr>
              <td>Eingeladen</td>
              <td>Mitglied</td>
              <td>Keine Anzeige</td>
            </tr>
            <tr>
              <td>Nicht im Programm</td>
              <td>In Liste</td>
              <td>Geöffnet</td>
            </tr>
            <tr>
              <td>Registriert</td>
              <td>Wird registriert</td>
              <td>Registrierungsfehler</td>
            </tr>
            <tr>
              <td>Gesendet</td>
              <td>Abonniert</td>
              <td>Abbestellt</td>
            </tr>
            <tr>
              <td>Angesehen</td>
              <td>Besucht</td>
              <td>Besuchter Stand</td>
            </tr>
            <tr>
              <td>Auf der Warteliste</td>
              <td>Web-Inhalt</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updatedAt*</td>
      <td>Datumsbereich</td>
      <td>Akzeptiert ein JSON-Objekt mit den Membern startAt und endAt. startAt akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und endAt akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Datetimes sollten im ISO-8601-Format vorliegen, ohne Millisekunden. Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zuletzt innerhalb des Datumsbereichs aktualisiert wurden.</td>
    </tr>
  </tbody>
</table>

Filtertyp ist für einige Abonnements nicht verfügbar. Wenn für Ihr Abonnement nicht verfügbar ist, erhalten Sie eine Fehlermeldung beim Aufruf des Endpunkts „Programmabonnementauftrag erstellen“ („1035, Nicht unterstützter Filtertyp für Zielabonnement„). Kunden können sich an den Marketo-Support wenden, um diese Funktion in ihrem Abonnement aktivieren zu lassen.

## Optionen

Der Endpunkt Abonnentenauftrag für Exportprogramm erstellen bietet mehrere Formatierungsoptionen. Diese Optionen bieten Benutzenden folgende Möglichkeiten:

- Geben Sie die Felder an, die in die exportierte Datei aufgenommen werden sollen
- Spaltenüberschriften dieser Felder umbenennen
- Geben Sie das Format der exportierten Datei an

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| Felder | array[string] | Ja | Der Feldparameter akzeptiert ein JSON-Zeichenfolgen-Array. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Die folgenden Feldtypen können exportiert werden:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Geben Sie ein Feld mithilfe des REST-API-Namens an, der mithilfe der Endpunkte „Lead2 beschreiben“ und/oder „Programmteilnehmer beschreiben“ abgerufen werden kann. |
| columnHeaderNames | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenkopfzeilennamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportvorgang enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Format | Zeichenfolge | Nein | Akzeptiert eine der folgenden Optionen: CSV, TSV, SSV. Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertedatei gerendert, sofern festgelegt. Die Standardeinstellung ist CSV, wenn nicht festgelegt. |


## Erstellen von Aufträgen

Die Parameter für den Auftrag werden vor dem Start des Exports mithilfe des Endpunkts [Export-Programmelement-Auftrag erstellen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) definiert. Wir müssen die `filter` definieren, die die Programm-ID und die für den Export erforderlichen `fields` enthalten. Optional können wir den `format` der Datei und die `columnHeaderNames` definieren.

```
POST /bulk/v1/program/members/export/create.json
```

```json
{
   "format": "CSV",
   "fields": [
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": {
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

Dadurch wird eine Statusantwort zurückgegeben, die angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, aber noch nicht gestartet. Dazu muss der Endpunkt [Exportprogrammmitgliedsauftrag in die Warteschlange einreihen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) über den `exportId` aus der Erstellungsstatusantwort aufgerufen werden:

```
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

Daraufhin wird mit dem ersten `status` „In Warteschlange“ geantwortet und auf „Wird verarbeitet“ gesetzt, wenn ein Exportsteckplatz verfügbar ist.

## Status des Abrufauftrags

Hinweis: Der Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags dessen Status abfragen, um den Fortschritt zu ermitteln. Abfrage mit dem Endpunkt [Abruf des Status des Exportprogrammmitglieds](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine niedrigere Abfrageintervall nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Das Statusfeld kann mit einem der folgenden Elemente antworten: Erstellt, In Warteschlange, Verarbeitung läuft, Abgebrochen, Abgeschlossen, Fehlgeschlagen.

```
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Der Status-Endpunkt antwortet und gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Sobald der Auftrag `status` in „Abgeschlossen“ geändert wurde, kann er heruntergeladen werden.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Daten abrufen

Um die Datei eines abgeschlossenen Exports von Programmmitgliedern abzurufen, rufen Sie einfach den Endpunkt [Abrufen der ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET)-Datei für Programmmitglieder) mit Ihrem `exportId` auf.

Die Antwort enthält eine -Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der -Datei. Wenn ein angefordertes Programmmitgliedsfeld leer ist (keine Daten enthält), wird `null` in der Exportdatei im entsprechenden Feld platziert.

```
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Um das partielle und fortsetzungsfreundliche Abrufen extrahierter Daten zu unterstützen, unterstützt der Datei-Endpunkt optional den HTTP-Header-Bereich vom Typ Byte. Wenn die Kopfzeile nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Weitere Informationen zur Verwendung des Bereichs-Headers mit Marketo [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem Endpunkt [Exportprogrammabonnementauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) abgebrochen werden:

```
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

Dies antwortet mit einer `status`, die angibt, dass der Vorgang abgebrochen wurde.
