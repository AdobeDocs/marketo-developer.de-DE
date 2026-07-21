---
title: Programm-Mitglieder
feature: REST API
description: Verwenden Sie die Marketo-REST-API zum Lesen, Erstellen, Aktualisieren und Löschen von Programmmitgliedern, Verwalten von Standard- und benutzerdefinierten Feldern und Abfragen mithilfe von durchsuchbaren Feldern.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
TQID: https://experienceleague.adobe.com/scEHyXYq9C7cCS1kIX810wG7ahT9fsa448NwIfBmzQM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d1d0a9cd-295d-4976-8c39-ddae266f240eid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1670
ht-degree: 2%

---

# Programm-Mitglieder

[Endpunkt-Referenz für Programmmitglieder](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members)

Marketo bietet APIs zum Lesen, Erstellen, Aktualisieren und Löschen von Programmmitgliedsdatensätzen. Das Feld Lead-ID verknüpft Programmmitglieder-Datensätze mit Lead-Datensätzen.

Jeder Datensatz enthält Standardfelder und kann bis zu 20 benutzerdefinierte Felder enthalten. In diesen Feldern werden programmspezifische Elementdaten zur Verwendung in Formularen, Filtern, Triggern und Flussaktionen gespeichert. Sie können diese Daten auf der Registerkarte [Mitglieder“ des Programms in ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) Benutzeroberfläche von Marketo Engage einsehen.

## beschreiben

Der Endpunkt [Programmelement beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) folgt dem Standardmuster für Lead-Datenbankobjekte.

- Das `searchableFields`-Array identifiziert Felder, die für Abfragen gültig sind.
- Das `fields`-Array enthält Metadaten wie den REST-API-Namen, den Anzeigenamen und ob das Feld aktualisierbar ist.

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

## Abfrage

Verwenden Sie den [Get Program Members](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMembersUsingGET)-Endpunkt, um Mitglieder eines Programms abzurufen. Für die Anfrage sind ein `programId` Pfadparameter sowie `filterType`- und `filterValues` erforderlich.

`programId` gibt das zu durchsuchende Programm an.

`filterType` gibt das als Suchfilter zu verwendende Feld an. Sie akzeptiert alle Felder in der Liste „searchableFields“, die vom Endpunkt [Programmmitglied beschreiben“ zurückgegeben ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Für ein benutzerdefiniertes Feld muss der Datentyp entweder „Zeichenfolge“ oder „Ganzzahl“ sein.

Wenn „filterType“ nicht „leadId“ ist, kann die Anfrage maximal 100.000 Programmmember-Einträge verarbeiten. Je nach Konfiguration Ihrer Marketo-Instanz erhalten Sie einen der folgenden Fehler:

- Wenn die Gesamtzahl der Programmmitglieder 100.000 überschreitet, wird ein Fehler zurückgegeben: „1003, Gesamtmitgliedsgröße: 100.001 überschreitet das zulässige Limit von 100.000 für den Filter“.
- Wenn die Gesamtzahl der Programmmitglieder _die mit dem Filter übereinstimmen_ 100.000 überschreitet, wird ein Fehler zurückgegeben: „1003, übereinstimmende Mitgliedschaftsgröße: 100.001 überschreitet das für diese API zulässige Limit (100.000)“.

Um ein Programm abzufragen, dessen Mitgliederzahl das Limit überschreitet, verwenden Sie stattdessen die [Bulk Program Member Extract API](bulk-program-member-extract.md).

`filterValues` gibt die zu suchenden Werte an und akzeptiert bis zu 300 kommagetrennte Werte. Der Aufruf sucht nach Datensätzen, bei denen das Programmmitgliedsfeld mit einem der eingeschlossenen filterValues übereinstimmt.

Alternativ können Sie nach Datumsbereich filtern, indem Sie `updatedAt` als filterType angeben und die `startAt` und `endAt` Datums-/Uhrzeitparameter angeben. Der Bereich muss sieben Tage oder weniger betragen. Verwenden Sie das ISO-8601-Format ohne Millisekunden für Datums- und Uhrzeitwerte.

Der optionale `fields`-Abfrageparameter akzeptiert eine kommagetrennte Liste von Feld-API-Namen, die vom Endpunkt [Programmelement beschreiben](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) zurückgegeben werden. Wenn enthalten, enthält jeder Antwortdatensatz die angegebenen Felder. Wenn sie weggelassen wird, gibt die Antwort standardmäßig `acquiredBy`, `leadId`, `membershipDate`, `programId` und `reachedSuccess` zurück.

Standardmäßig gibt der Endpunkt maximal 300 Datensätze zurück. Verwenden Sie den `batchSize` Abfrageparameter, um diese Zahl zu reduzieren.

Wenn das **moreResult**-Attribut wahr ist, sind weitere Ergebnisse verfügbar. Fahren Sie mit dem Aufruf des Endpunkts mit dem zurückgegebenen `nextPageToken` fort, bis moreResult den Wert „false“ hat.

Wenn die Gesamtlänge der GET-Anfrage 8 KB überschreitet, gibt der Endpunkt den HTTP-Fehler „414, URI zu lang“ zurück. Um dieses Limit zu umgehen, ändern Sie die Anfrage von GET in POST, fügen Sie den `_method=GET` Parameter hinzu und platzieren Sie die Abfragezeichenfolge im Anfragetext.

```http
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Erstellen und aktualisieren

Zwei Endpunkte unterstützen Vorgänge zum Erstellen und Aktualisieren von Programmmitgliedern:

- Ein Endpunkt aktualisiert nur den Programmmitgliedsstatus.
- Ein Endpunkt aktualisiert die als „aktualisierbar“ markierten Programmteilnehmerfelder.

Jeder Endpunkt kann bis zu 300 Programmmitglieder-Datensätze pro Aufruf ändern.

### Status des Programmmitglieds

Verwenden Sie den Endpunkt [Programmmitgliedsstatus ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST), um den Programmstatus für ein oder mehrere Mitglieder zu erstellen oder zu aktualisieren.

Die erforderlichen Parameter sind:

- `programId`: Ein Pfadparameter, der das Programm mit den zu erstellenden oder zu aktualisierenden Mitgliedern angibt.
- `statusName`: Gibt den Programmstatus an, der auf eine Lead-Liste angewendet werden soll. Der statusName muss mit einem verfügbaren Status für den Kanal des Programms übereinstimmen. Rufen Sie gültige Status mit dem Endpunkt [Kanäle abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels/operation/getAllChannelsUsingGET) ab. Wenn der Status eines Leads einen größeren Schrittwert als den designierten Statusnamen hat, überspringt die Anfrage diesen Lead.
- `input`: Ein Array von `leadId`, die den Programmmitgliedern entsprechen. Pro Aufruf können bis zu 300 LeadIds gesendet werden.

Der Endpunkt führt für jeden Datensatz eine Upsert-Aktion durch. Wenn die LeadId mit einem Programmmitglied verknüpft ist, aktualisiert der Endpunkt seinen Mitgliedschaftsstatus. Andernfalls wird ein Programmteilnehmer-Datensatz erstellt, der Datensatz mit der Lead-ID verknüpft und der Mitgliedschaftsstatus zugewiesen.

Die Antwort enthält die `status` „aktualisiert“, „erstellt“ oder „übersprungen“. Ein übersprungenes Ergebnis enthält auch ein `reasons`-Array. Das `seq` ist ein Index, der jeden gesendeten Datensatz mit der Antwortreihenfolge korreliert.

Wenn der Aufruf erfolgreich ist, wird eine Aktivität vom Typ „Programmstatus ändern“ in das Aktivitätsprotokoll des Leads geschrieben.

```http
POST /rest/v1/programs/{programId}/members/status.json
```

```text
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Daten der Programmteilnehmer

Verwenden Sie den Endpunkt [Programmteilnehmerdaten synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberDataUsingPOST), um die Felddaten für Programmteilnehmer für ein oder mehrere Mitglieder zu aktualisieren. Sie können jedes benutzerdefinierte Feld oder jedes Standardfeld ändern, das vom Endpunkt [Programmteilnehmer beschreiben“ als ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) gekennzeichnet wird.

Die erforderlichen Parameter sind:

- `programId`: Ein Pfadparameter, der das Programm mit den zu aktualisierenden Elementen angibt.
- `input`: Ein Array, dessen Elemente eine `leadId` und ein oder mehrere Felder enthalten, die nach API-Namen aktualisiert werden sollen. Pro Aufruf können bis zu 300 LeadIds gesendet werden.

Der Endpunkt aktualisiert jeden Datensatz. Die LeadId muss mit einem Programmmitglied verknüpft sein und jedes Feld muss aktualisierbar sein.

Die Antwort enthält den `status` „aktualisiert“ oder „übersprungen“. Ein übersprungenes Ergebnis enthält auch ein `reasons`-Array. Das `seq` ist ein Index, der jeden gesendeten Datensatz mit der Antwortreihenfolge korreliert.

Wenn der Aufruf erfolgreich ist, wird eine Aktivität vom Typ „Programmteilnehmerdaten ändern“ in das Aktivitätsprotokoll des Leads geschrieben.

```http
POST /rest/v1/programs/{programId}/members.json
```

```text
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Felder

Das Programmelement-Objekt enthält Standardfelder und optionale benutzerdefinierte Felder. Standardfelder sind in jedem Marketo Engage-Abonnement vorhanden, während Benutzerinnen und Benutzer nach Bedarf benutzerdefinierte Felder erstellen.

Jedes Feld wird durch Attribute wie Anzeigename, API-Name und Datentyp definiert. Zusammen werden diese Attribute als Metadaten bezeichnet.

Die folgenden Endpunkte fragen, erstellen und aktualisieren Felder im Programmmitgliedsobjekt. Der API-Benutzer muss über eine Rolle mit der **Standardfeld für Lese- und Schreibschemas**, der Berechtigung **Benutzerdefiniertes Schema für Lese- und Schreibzugriff** oder beiden verfügen.

### Abfragefelder

Fragen Sie ein Feld für Programmmitglieder nach API-Namen ab oder rufen Sie alle Felder für Programmmitglieder ab. Die Rollenberechtigungen bestimmen, ob die Antwort Standardfelder, benutzerdefinierte Felder oder beides enthalten kann. Die Antwort enthält auch ausgeblendete Felder.

#### Nach Name

Der Endpunkt [Abrufen des Programmmitgliedsfelds nach Name](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) ruft Metadaten für ein Feld im Programmmitgliedsobjekt ab. Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an.

Die Antwort ähnelt der Antwort des Programmmitglieds Beschreiben , enthält jedoch zusätzliche Metadaten. Beispielsweise gibt das `isCustom`-Attribut an, ob das Feld benutzerdefiniert ist.

```http
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Durchsuchen

Der Endpunkt [Abrufen von ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldsUsingGET)-Feldern“ ruft Metadaten für alle Felder im Programmmitgliedsobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Verwenden Sie den `batchSize` Abfrageparameter, um diese Zahl zu reduzieren.

Wenn das `moreResult` „true“ ist, sind weitere Ergebnisse verfügbar. Fahren Sie mit dem Aufruf des Endpunkts mit dem zurückgegebenen `nextPageToken` fort, bis moreResult den Wert „false“ hat.

```http
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Erstellen von Feldern

Der Endpunkt [Create Program Member Fields](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) erstellt benutzerdefinierte Felder auf dem Programmmitgliedsobjekt. Sie bietet Funktionen, die mit denen der [Marketo Engage-Benutzeroberfläche vergleichbar ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Sie können mit diesem Endpunkt bis zu 20 benutzerdefinierte Felder erstellen.

Berücksichtigen Sie jedes Feld sorgfältig, bevor Sie es in einer Marketo Engage-Produktionsinstanz erstellen. Nachdem Sie ein Feld erstellt haben, können Sie es nicht löschen ([ können es nur ausblenden](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo). Nicht verwendete Felder sorgen für Unordnung in der Instanz.

Der erforderliche `input` ist ein Array von Feldobjekten für Programmmitglieder. Jedes Objekt enthält ein oder mehrere Attribute.

- Erforderliche Attribute sind `displayName`, `name` und `dataType`. Sie entsprechen dem Anzeigenamen der Benutzeroberfläche, dem API-Namen bzw. dem Feldtyp.
- Optionale Attribute sind `description`, `isHidden`, `isHtmlEncodingInEmail` und `isSensitive`.

Die Attribute `name` und `displayName` haben die folgenden Benennungsregeln:

- Das `name` muss eindeutig sein, mit einem Buchstaben beginnen und nur Buchstaben, Zahlen oder Unterstriche enthalten.
- Der *`isplayName` muss eindeutig sein und darf keine Sonderzeichen enthalten.

Eine gängige Konvention ist die Anwendung [Camel Case](https://en.wikipedia.org/wiki/Camel_case#) auf `displayName` zur Herstellung von `name`. Beispiel: Eine `displayName` von „Mein benutzerdefiniertes Feld“ erzeugt einen `name` von „myCustomField“.

```http
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Feld aktualisieren

Der Endpunkt [Programm-Member-Feld aktualisieren](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) aktualisiert ein benutzerdefiniertes Feld im Programmmember-Objekt. Die meisten in der Marketo Engage-Benutzeroberfläche verfügbaren Feldaktualisierungen sind auch über die API verfügbar. In der folgenden Tabelle sind die Unterschiede aufgeführt.

| Attribut | Von API aktualisierbar? | Von der Benutzeroberfläche aktualisierbar? | Von API aktualisierbar? | Von der Benutzeroberfläche aktualisierbar? |
| --- | --- | --- | --- | --- |
| dataType | nein | nein | nein | ja |
| Beschreibung | ja | ja | ja | ja |
| displayName | nein | nein | ja | ja |
| isCustom | nein | nein | nein | nein |
| isHidden | nein | ja | Ja (wenn von API erstellt) | ja |
| isHtmlEncodingInEmail | ja | ja | ja | ja |
| isSensitive | ja | ja | ja | ja |
| length | nein | nein | nein | nein |
| name | nein | nein | nein | nein |

Die Anfrage erfordert die folgenden Parameter:

- `fieldApiName`: Ein Pfadparameter, der den API-Namen des zu aktualisierenden Felds angibt.
- `input`: Ein Array, das ein Lead-Feld-Objekt mit einem oder mehreren Attributen enthält.

```http
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Löschen

Verwenden Sie den Endpunkt [Delete Program Members](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/deleteProgramMemberUsingPOST), um die Einträge der Programmteilnehmer zu löschen. Der erforderliche `programId` gibt das Programm an, das die zu löschenden Elemente enthält.

Der Anfragetext enthält ein `input` Array von Lead-IDs. Jeder Aufruf erlaubt maximal 300 Lead-IDs.

Die Antwort enthält den `status` „gelöscht“ oder „übersprungen“. Ein übersprungenes Ergebnis enthält auch ein `reasons`-Array. Das `seq` ist ein Index, der jeden gesendeten Datensatz mit der Antwortreihenfolge korreliert.

```http
POST /rest/v1/programs/{programId}/members/delete.json
```

```text
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
