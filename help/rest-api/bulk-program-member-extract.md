---
title: "Massen-Programmteilnahme"
feature: REST API
description: "Stapelverarbeitung der Extraktion von Mitgliederdaten."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 4%

---


# Massen-Programm Mitgliederextraktion

[Referenz zum Extract Endpoint des Bulk Program](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

Der Satz von REST-APIs zur Mitgliederextrahierung von Massen-Programmteilnehmern bietet eine programmatische Schnittstelle zum Abrufen großer Mengen von Programmmitgliedern-Datensätzen aus Marketo. Dies ist die empfohlene Schnittstelle für Anwendungsfälle, die einen kontinuierlichen Datenaustausch zwischen Marketo und einem oder mehreren externen Systemen erfordern, zum ETL-, Daten-Warehouse- und Archivierungszwecken.

## Berechtigungen

Die Bulk-Programm-Teilnehmer-Extract-APIs erfordern, dass der Eigentümer-API-Benutzer über eine oder beide der Lese-/Schreibzugriff-Lead- oder Lese-SchreibLead-Berechtigungen verfügt.

## Beschreibung

[Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) ist die primäre &quot;Source of Truth&quot;, die angibt, ob Felder zur Verwendung verfügbar sind, und Metadaten zu diesen Feldern. Die `name` enthält den REST-API-Namen.

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

Programmmitglieder unterstützen verschiedene Filteroptionen. Für einen Auftrag können mehrere Filtertypen angegeben werden. In diesem Fall werden sie UND zusammen zugewiesen. Sie müssen entweder die `programId` oder `programIds` Filter. Alle anderen Filter sind optional. Die `updatedAt` -Filter erfordert zusätzliche Infrastrukturkomponenten, die noch nicht für alle Abonnements bereitgestellt wurden.

<table>
  <tbody>
    <tr>
      <td>Filtertyp</td>
      <td>Datentyp</td>
      <td>Hinweise</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Ganze Zahl</td>
      <td>Akzeptiert die ID eines Programms. Aufträge geben alle zugänglichen Datensätze zurück, die zum Zeitpunkt der Auftragsverarbeitung Mitglieder des Programms sind.Programm-IDs mithilfe der <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Abrufen von Programmen</a> endpoint.Kann nicht mit dem Filter programIds verwendet werden.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Integer]</td>
      <td>Akzeptiert ein Array von bis zu 10 Programm-IDs. Aufträge geben alle zugänglichen Datensätze zurück, die zum Zeitpunkt der Auftragsverarbeitung zu den Programmen gehören. Als erstes Feld wird der Exportdatei das zusätzliche Feld "programId" hinzugefügt. In diesem Feld wird das Programm identifiziert, aus dem ein Programmmitgliedschaftsdatensatz extrahiert wurde.Programm-IDs abrufen mithilfe der <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Abrufen von Programmen</a> endpoint.Kann nicht mit dem Filter programId verwendet werden.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Boolesch</td>
      <td>Akzeptiert einen booleschen Wert, der zum Filtern von Aufzeichnungen zur Programmmitgliedschaft für verwendet wird <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">Personen, die keine Inhalte mehr haben</a>.</td>
    </tr>
    <tr>
      <td>retureCadence</td>
      <td>Zeichenfolge</td>
      <td>Akzeptiert eine Zeichenfolge, die zum Filtern der Einträge der Programmmitgliedschaft für eine bestimmte Pflegekatenz verwendet wird. Zulässige Werte sind:
        <ul>
          <li>pause - cadence is pause</li>
          <li>Norm - Kadenz ist normal</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Akzeptiert ein Array von Statusnamen der Programmmitglieder. Mehrere Statusnamen werden ODER zusammen angegeben. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, deren Programmteilstatus mit einem der angegebenen Statusnamen übereinstimmt. Es können sowohl standardmäßige als auch benutzerdefinierte Statusnamen verwendet werden. Wenn der Filter statusNames mit dem Filter "programIds"verwendet wird, wird jedes Programm auf Mitgliedschaftsdatensätze überprüft, deren Status mit einem der Statusnamen übereinstimmt. Wenn in keinem der Programme ein Statusname gefunden wird, wird der Fehler "1003, Ungültige Daten"zurückgegeben.
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
      <td>Akzeptiert ein JSON-Objekt mit den Elementen startAt und endAt. startAt akzeptiert einen Datum, der das niedrige Wasserzeichen darstellt, und endAt akzeptiert einen Datum, der das hohe Wasserzeichen darstellt. Der Zeitraum muss 31 Tage oder weniger betragen. Datums- und Uhrzeitformate sollten im ISO-8601-Format vorliegen, ohne Millisekunden. Aufträge mit diesem Filtertyp geben alle verfügbaren Datensätze zurück, die zuletzt im Datumsbereich aktualisiert wurden.</td>
    </tr>
  </tbody>
</table>

Für einige Abonnements ist der Filtertyp nicht verfügbar. Wenn Sie für Ihr Abonnement nicht verfügbar sind, erhalten Sie beim Aufrufen des Endpunkts &quot;Create Export Program Member Job&quot;(&quot;1035, Unsupported filter type for target subscription&quot;) eine Fehlermeldung. Kunden können sich an den Marketo-Support wenden, damit diese Funktion in ihrem Abonnement aktiviert wird.

## Optionen

Der Endpunkt &quot;Create Export Program Member Job&quot;bietet mehrere Formatierungsoptionen. Diese Optionen bieten Benutzern folgende Möglichkeiten:

- Geben Sie die in die exportierte Datei einzuschließenden Felder an
- Spaltenüberschriften dieser Felder umbenennen
- Format der exportierten Datei angeben

| Parameter | Datentyp | Erforderlich | Hinweise |
|---|---|---|---|
| Felder | Array[Zeichenfolge] | Ja | Der Feldparameter akzeptiert ein JSON-Array von Zeichenfolgen. Die aufgelisteten Felder sind in der exportierten Datei enthalten. Die folgenden Feldtypen können exportiert werden:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Geben Sie ein Feld mithilfe des REST-API-Namens an, das mit &quot;Desbe Lead2&quot;und/oder &quot;Describe Program Member endpoints&quot;abgerufen werden kann. |
| columnHeaderName | Objekt | Nein | Ein JSON-Objekt, das Schlüssel-Wert-Paare von Feld- und Spaltenüberschriftsnamen enthält. Der Schlüssel muss der Name eines Felds sein, das im Exportauftrag enthalten ist. Der Wert ist der Name der exportierten Spaltenüberschrift für dieses Feld. |
| format | Zeichenfolge | Nein | Akzeptiert eines von: CSV, TSV, SSV. Die exportierte Datei wird als Datei mit kommagetrennten Werten, tabulatorgetrennten Werten oder durch Leerzeichen getrennten Werten gerendert, sofern festgelegt. Wenn nicht festgelegt, wird standardmäßig CSV verwendet. |


## Erstellen eines Auftrags

Die Parameter für den Auftrag werden definiert, bevor der Export mit der [Vorgang &quot;Export Program Member Job&quot;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) -Endpunkt. Wir müssen die `filter` mit der Programm-ID und der `fields` die für den Export benötigt werden. Optional können wir die `format` der Datei und der `columnHeaderNames`.

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

Dadurch wird eine Statusantwort zurückgegeben, die angibt, dass der Auftrag erstellt wurde. Der Auftrag wurde definiert und erstellt, wurde jedoch noch nicht gestartet. Dazu muss die Variable [Vorgang &quot;Enqueue Export Program Member&quot;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) Endpunkt muss mit der `exportId` aus der Erstellungsstatus-Antwort:

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

Dies wird mit einer ersten `status` des Felds &quot;In Warteschlange&quot;, nach dem der Wert auf &quot;Verarbeitung&quot;eingestellt wird, wenn ein Exportfenster verfügbar ist.

## Abruf-Auftragsstatus

Hinweis: Der Status kann nur für Aufträge abgerufen werden, die vom selben API-Benutzer erstellt wurden.

Da es sich um einen asynchronen Endpunkt handelt, müssen wir nach der Erstellung des Auftrags seinen Status abfragen, um seinen Fortschritt zu bestimmen. Umfrage mit der [Abrufen des Status eines Programmteilnahmeauftrags](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) -Endpunkt. Der Status wird nur einmal alle 60 Sekunden aktualisiert, sodass eine kürzere Abrufhäufigkeit nicht empfohlen wird und in fast allen Fällen immer noch zu hoch ist. Das Statusfeld kann mit einer der folgenden Optionen reagieren: Erstellt, In Warteschlange, Verarbeitung, Abgebrochen, Abgeschlossen, Fehlgeschlagen.

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

Der Statusendpunkt gibt an, dass der Auftrag noch verarbeitet wird, sodass die Datei noch nicht zum Abrufen verfügbar ist. Nach dem Auftrag `status` ändert sich in &quot;Abgeschlossen&quot;. Es steht zum Download zur Verfügung.

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

## Abrufen Ihrer Daten

Um die Datei eines abgeschlossenen Programmteilsexports abzurufen, rufen Sie einfach die [Mitgliederdatei des Exportprogramms abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) Endpunkt mit `exportId`.

Die Antwort enthält eine Datei, die so formatiert ist, wie der Auftrag konfiguriert wurde. Der Endpunkt antwortet mit dem Inhalt der Datei. Wenn ein angefordertes Feld für Programmmitglieder leer ist (keine Daten enthält), dann `null` wird in das entsprechende Feld in der Exportdatei eingefügt.

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

Um das teilweise und wiederverwendbare Abrufen extrahierter Daten zu unterstützen, unterstützt der Dateiendpunkt optional den HTTP-Header-Bereich der Typ-Bytes. Wenn der Header nicht festgelegt ist, wird der gesamte Inhalt zurückgegeben. Weitere Informationen zur Verwendung der Bereichskopfzeile mit Marketo [Massenextraktion](bulk-extract.md).

## Abbruch eines Auftrags

Wenn ein Auftrag falsch konfiguriert wurde oder unnötig wird, kann er einfach mit dem [Vorgang &quot;Export Program Member Job&quot;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) Endpunkt:

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

Dies antwortet mit einer `status` gibt an, dass der Auftrag abgebrochen wurde.
