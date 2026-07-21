---
title: Massenprogramm-Mitgliederextraktion
feature: REST API
description: Verwenden Sie die Marketo Bulk Program Member Extract REST-APIs, um große Mitgliederdatensätze für ETL, Data Warehousing und Archivierung mit Berechtigungen und Feldmetadaten zu exportieren.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
TQID: https://experienceleague.adobe.com/w4qaVTKSe0EORaSiURB6WbJXi29JUdEgfkb2dnfuVFw
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1026
ht-degree: 5%

---

# Massenprogramm-Mitgliederextraktion

[Referenz zum Extraktionsendpunkt des Massenprogrammmitglieds](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

Die REST-APIs zum Extrahieren von Programmmitgliedern rufen große Mengen von Programmmitgliederdatensätzen aus Marketo ab. Verwenden Sie diese APIs für den kontinuierlichen Datenaustausch zwischen Marketo und externen Systemen, ETL, Data Warehousing und Archivierung.

## Berechtigungen

Der API-Benutzer muss über eine Rolle mit der Schreibschutz-Lead-Berechtigung, der Lese-Schreib-Lead-Berechtigung oder beidem verfügen.

## beschreiben

Verwenden Sie [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) um zu bestimmen, welche Felder verfügbar sind, und um deren Metadaten abzurufen. Das `name`-Attribut enthält den REST-API-Feldnamen.

```http
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

Exporte von Programmmitgliedern unterstützen mehrere Filteroptionen. Wenn ein Auftrag mehrere Filtertypen angibt, werden diese von der API mit einem AND-Vorgang kombiniert.

In jedem Auftrag muss entweder `programId` oder `programIds` angegeben werden. Alle anderen Filter sind optional. Der `updatedAt` erfordert eine Infrastruktur, die nicht in allen Abonnements verfügbar ist.

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
      <td>Akzeptiert die ID eines Programms. Jobs geben alle zugänglichen Datensätze zurück, die zu dem Zeitpunkt Mitglieder des Programms sind, zu dem der Job mit der Verarbeitung beginnt.Abrufen von Programm-IDs mit dem Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Programme abrufen</a>.Kann nicht mit programIds-Filter verwendet werden.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Ganzzahl]</td>
      <td>Akzeptiert ein Array von bis zu 10 Programm-IDs. Jobs geben alle zugänglichen Datensätze zurück, die zu dem Zeitpunkt Mitglieder der Programme sind, zu dem der Job mit der Verarbeitung beginnt.Ein zusätzliches Feld „programId“ wird der Exportdatei als erstes Feld hinzugefügt. In diesem Feld wird das Programm angegeben, aus dem ein Programmmitgliedschaftseintrag extrahiert wurde.Abrufen von Programm-IDs mit dem Endpunkt <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Programme abrufen</a>.Kann nicht mit dem programId-Filter verwendet werden.</td>
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
      <td>Akzeptiert ein JSON-Objekt mit den Membern startAt und endAt. startAt akzeptiert eine Uhrzeit-/Datumsangabe, die das Niedrigwasserzeichen darstellt, und endAt akzeptiert eine Uhrzeit-/Datumsangabe, die das Hochwasserzeichen darstellt. Der Bereich muss 31 Tage oder weniger betragen. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden.Aufträge mit diesem Filtertyp geben alle Datensätze zurück, auf die zugegriffen werden kann und die zuletzt innerhalb des Datumsbereichs aktualisiert wurden.</td>
    </tr>
  </tbody>
</table>

Einige Abonnements unterstützen diesen Filtertyp nicht. Wenn er nicht verfügbar ist, gibt der Endpunkt Exportprogrammelement-Vorgang erstellen `1035, Unsupported filter type for target subscription` zurück. Wenden Sie sich an den Marketo-Support, um diese Funktion für Ihr Abonnement anzufordern.

## Optionen

Der Endpunkt Abonnentenauftrag für Exportprogramm erstellen bietet Optionen für:

- Geben Sie die Felder an, die in die Exportdatei aufgenommen werden sollen.
- Benennen Sie die exportierten Spaltenüberschriften um.
- Geben Sie das Exportdateiformat an.

| Parameter | Datentyp | Erforderlich | Hinweise |
| --- | --- | --- | --- |
| Felder | array[string] | Ja | Der Feldparameter akzeptiert ein JSON-Zeichenfolgen-Array. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Die folgenden Feldtypen können exportiert werden:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Geben Sie ein Feld an, indem Sie den REST-API-Namen verwenden, der mithilfe der Endpunkte „Lead2 beschreiben“ und/oder „Programmteilnehmer beschreiben“ abgerufen werden kann. |
| columnHeaderNames | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenkopfzeilennamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportvorgang enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| Format | String | Nein | Akzeptiert eine der folgenden Optionen: CSV, TSV, SSV. Die exportierte Datei wird als kommagetrennte Werte, tabulatorgetrennte Werte oder durch Leerzeichen getrennte Wertedatei gerendert, sofern festgelegt. Die Standardeinstellung ist CSV, wenn nicht festgelegt. |

## Erstellen von Aufträgen

Verwenden Sie den Endpunkt [Abonnentenauftrag für Exportprogramm erstellen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) um den Exportauftrag zu definieren. Geben Sie ein `filter` an, das die Programm-ID und das zu exportierende `fields` enthält. Sie können auch `format` und `columnHeaderNames` angeben.

```http
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

Die Antwort bestätigt, dass der Auftrag erstellt wurde, der Export jedoch nicht automatisch gestartet wird. Übergeben Sie die zurückgegebene `exportId` an den Endpunkt [Enqueue-Exportprogrammelement-Vorgang](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST), um den Vorgang zu starten:

```http
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

Die Enqueue-Antwort gibt zunächst einen `Queued` zurück. Wenn ein Exportsteckplatz verfügbar ist, ändert sich der Status in `Processing`.

## Status des Abrufauftrags

Sie können den Status nur für Aufträge abrufen, die von demselben API-Benutzer erstellt wurden.

Da der Export asynchron ausgeführt wird, können Sie den Fortschritt mit dem Endpunkt [Abruf des Status des Exportprogrammmitglieds](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) abfragen. Der Status wird nur einmal alle 60 Sekunden aktualisiert, führen Sie daher keine häufigeren Abfragen durch.

Der Status kann `Created`, `Queued`, `Processing`, `Canceled`, `Completed` oder `Failed` sein.

```http
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

Diese Antwort zeigt an, dass der Auftrag noch verarbeitet wird, sodass die Datei nicht verfügbar ist. Wenn sich der Auftragsstatus in `Completed` ändert, kann die Datei heruntergeladen werden.

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

Um einen abgeschlossenen Export von Programmmitgliedern abzurufen, übergeben Sie die `exportId` an den Endpunkt [Abrufen der Elementdatei für ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET).

Der Endpunkt gibt die Datei in dem Format zurück, das für den Auftrag konfiguriert wurde. Wenn ein angefordertes Programmmitgliedsfeld keine Daten enthält, enthält das entsprechende Exportfeld `null`.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
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

Beim teilweisen oder wiederaufnehmbaren Abrufen unterstützt der Datei-Endpunkt den optionalen HTTP-`Range`-Header mit dem Bereichstyp `bytes`. Wenn Sie die -Kopfzeile nicht festlegen, gibt der Endpunkt die gesamte Datei zurück. Weitere Informationen finden Sie unter [Massenextraktion](bulk-extract.md).

## Abbrechen von Aufträgen

Um einen Auftrag abzubrechen, der falsch konfiguriert oder nicht mehr benötigt wird, rufen Sie den Endpunkt [Export-Programmabonnementauftrag abbrechen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) auf:

```http
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

Der Antwortstatus gibt an, dass der Vorgang abgebrochen wurde.
