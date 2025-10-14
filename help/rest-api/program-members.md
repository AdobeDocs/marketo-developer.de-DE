---
title: Programm-Mitglieder
feature: REST API
description: Verwenden Sie die Marketo-REST-API zum Lesen, Erstellen, Aktualisieren und Löschen von Programmmitgliedern, Verwalten von Standard- und benutzerdefinierten Feldern und Abfragen mithilfe von durchsuchbaren Feldern.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 2%

---

# Programm-Mitglieder

[Endpunkt-Referenz für Programmmitglieder](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo stellt APIs zum Lesen, Erstellen, Aktualisieren und Löschen von Programmmitgliedsdatensätzen bereit. Die Datensätze der Programmteilnehmer sind über das Feld Lead-ID mit den Lead-Datensätzen verknüpft. Die Datensätze bestehen aus einem Satz von Standardfeldern und optional bis zu 20 zusätzlichen benutzerdefinierten Feldern. Die Felder enthalten programmspezifische Daten für jedes Mitglied und können in Formularen, Filtern, Triggern und Flussaktionen verwendet werden. Diese Daten sind auf der Registerkarte [&#x200B; des Programms in der Benutzeroberfläche &#x200B;](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) Marketo Engage sichtbar.

## beschreiben

Der Endpunkt [Programmelement beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) folgt dem Standardmuster für Lead-Datenbankobjekte. Das `searchableFields`-Array liefert die Menge der Felder, die für die Abfrage gültig sind. Das `fields`-Array enthält Feldmetadaten, einschließlich REST-API-Name, Anzeigename und Feldaktualisierungsfähigkeit.

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

## Abfrage

Mit [&#x200B; Endpunkt &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET)Abrufen von Programmmitgliedern“ können Sie Mitglieder eines Programms abrufen. Dazu sind ein `programId` Pfadparameter sowie `filterType` und `filterValues` Abfrageparameter erforderlich.

`programId` wird verwendet, um anzugeben, nach welchem Programm gesucht werden soll.

`filterType` wird verwendet, um anzugeben, welches Feld als Suchfilter verwendet werden soll. Sie akzeptiert alle Felder in der Liste „searchableFields“, die vom Endpunkt [Programmmitglied beschreiben“ zurückgegeben &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2). Wenn Sie einen „filterType“ angeben, der ein benutzerdefiniertes Feld ist, muss der „dataType“ des benutzerdefinierten Felds entweder „string“ oder „integer“ sein. Wenn Sie einen anderen filterType als „leadId“ angeben, können maximal 100.000 Programmmember-Einträge von der Anfrage verarbeitet werden. Je nachdem, wie Ihre Marketo-Instanz konfiguriert ist, erhalten Sie einen der folgenden Fehler:

- Wenn die Gesamtzahl der Programmmitglieder 100.000 überschreitet, wird ein Fehler zurückgegeben: „1003, Gesamtmitgliedsgröße: 100.001 überschreitet das zulässige Limit von 100.000 für den Filter“.
- Wenn die Gesamtzahl der Programmmitglieder _die mit dem Filter übereinstimmen_ 100.000 überschreitet, wird ein Fehler zurückgegeben: „1003, übereinstimmende Mitgliedschaftsgröße: 100.001 überschreitet das für diese API zulässige Limit (100.000)“.

Um ein Programm abzufragen, dessen Mitgliederzahl das Limit überschreitet, verwenden Sie stattdessen die [Bulk Program Member Extract API](bulk-program-member-extract.md).

`filterValues` wird verwendet, um anzugeben, nach welchen Werten gesucht werden soll, und akzeptiert bis zu 300 Werte in einem kommagetrennten Format. Der Aufruf sucht nach Datensätzen, bei denen das Feld des Programmmitglieds mit einem der eingeschlossenen filterValues übereinstimmt.

Alternativ können Sie nach Datumsbereich filtern, indem Sie `updatedAt` als filterType mit `startAt`- und `endAt` angeben. Der Bereich muss sieben Tage oder weniger betragen. Datetimes sollten im ISO-8601-Format sein, ohne Millisekunden.

Der optionale Abfrageparameter `fields` akzeptiert eine kommagetrennte Liste von Feld-API-Namen, die vom Endpunkt [Programmelement beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) zurückgegeben wird. Wenn enthalten, enthält jeder Datensatz in der Antwort die angegebenen Felder. Wenn sie weggelassen wird, werden standardmäßig die Felder `acquiredBy`, `leadId`, `membershipDate`, `programId` und `reachedSuccess` zurückgegeben.

Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den `batchSize` Abfrageparameter verwenden, um diese Zahl zu reduzieren. Wenn das **moreResult**-Attribut wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

Wenn die Gesamtlänge Ihrer GET-Anfrage 8 KB überschreitet, wird ein HTTP-Fehler zurückgegeben: „414, URI zu lang“. Als Problemumgehung können Sie Ihre GET in „POST“ ändern, `_method=GET` Parameter hinzufügen und die Abfragezeichenfolge im Anfragetext platzieren.

```
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

Es gibt zwei Endpunkte, die die Erstellung/Aktualisierung von Programmmitgliedern unterstützen. Bei einem können Sie nur den Status des Programmmitglieds aktualisieren. Die andere ermöglicht es Ihnen, den Satz von Programmteilnehmerfeldern zu aktualisieren, die als „aktualisierbar“ markiert sind. Mit beiden Endpunkten können Sie bis zu 300 Programmmitglieder-Datensätze pro Aufruf ändern.

### Status des Programmmitglieds

Der [Endpunkt „Programmmitgliedsstatus &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST)&quot; wird verwendet, um den Programmstatus für ein oder mehrere Mitglieder zu erstellen oder zu aktualisieren.

Der erforderliche `programId` gibt das Programm an, das Mitglieder zum Erstellen oder Aktualisieren enthält.

Der erforderliche `statusName` gibt den Programmstatus an, der auf eine Liste der Leads angewendet werden soll. Der statusName muss mit einem verfügbaren Status für den Kanal des Programms übereinstimmen. Gültige Status können mit dem Endpunkt [Kanäle abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) abgerufen werden. Wenn der Status eines Leads einen größeren Schrittwert als den designierten Statusnamen hat, wird dieser Lead übersprungen.

Der erforderliche `input` ist ein Array von `leadId`, die Programmmitgliedern entsprechen. Pro Aufruf können bis zu 300 LeadIds gesendet werden. Für jeden Datensatz wird ein Upsert-Vorgang ausgeführt. Wenn die Lead-ID mit einem Programmmitglied verknüpft ist, wird der Mitgliedschaftsstatus aktualisiert. Andernfalls wird ein neuer Programmteilnehmer-Datensatz erstellt, der Datensatz wird mit der Lead-ID verknüpft und der Mitgliedschaftsstatus wird zugewiesen.

Der Endpunkt antwortet mit der `status` „aktualisiert“, „erstellt“ oder „übersprungen“. Wenn er übersprungen wird, wird auch ein `reasons`-Array eingeschlossen. Der Endpunkt antwortet auch mit einem `seq` Feld, das ein Index ist, mit dem die gesendeten Datensätze mit der Reihenfolge der Antwort korreliert werden können.

Wenn der Aufruf erfolgreich ist, wird eine Aktivität vom Typ „Programmstatus ändern“ in das Aktivitätsprotokoll des Leads geschrieben.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
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

Der Endpunkt [Daten zu Programmmitgliedern synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) wird verwendet, um Felddaten zu Programmmitgliedern für ein oder mehrere Mitglieder zu aktualisieren. Sie können alle benutzerdefinierten Felder oder Standardfelder ändern, die „aktualisierbar“ sind (siehe [Endpunkt „Programmmitglied beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2)).

Der erforderliche `programId` gibt das Programm an, das zu aktualisierende Mitglieder enthält.

Der erforderliche `input` ist ein Array. Jedes Array-Element enthält einen `leadId` und ein oder mehrere zu aktualisierende Felder (unter Verwendung des API-Namens). Für jeden Datensatz wird ein Aktualisierungsvorgang durchgeführt. Die LeadId muss mit einem Programmmitglied verknüpft sein. Die Felder müssen aktualisierbar sein. Pro Aufruf können bis zu 300 LeadIds gesendet werden.

Der Endpunkt antwortet mit der `status` „aktualisiert“ oder „übersprungen“. Wenn er übersprungen wird, wird auch ein `reasons`-Array eingeschlossen. Der Endpunkt antwortet auch mit einem `seq` Feld, das ein Index ist, mit dem die gesendeten Datensätze mit der Reihenfolge der Antwort korreliert werden können.

Wenn der Aufruf erfolgreich ist, wird eine Aktivität vom Typ „Programmteilnehmerdaten ändern“ in das Aktivitätsprotokoll des Leads geschrieben.

```
POST /rest/v1/programs/{programId}/members.json
```

```
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

Das Programmelement-Objekt enthält Standardfelder und optionale benutzerdefinierte Felder. Standardfelder sind in jedem Marketo Engage-Abonnement vorhanden, während benutzerdefinierte Felder vom Benutzer nach Bedarf erstellt werden. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind Anzeigename, API-Name und Datentyp. Diese Attribute werden zusammen als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder im Programmmitgliedsobjekt abfragen, erstellen und aktualisieren. Diese APIs erfordern, dass der besitzende API-Benutzer über eine Rolle mit einer oder beiden der Berechtigungen **Schema-Standardfeld lesen/schreiben** oder **Schema-benutzerdefiniertes Feld lesen/schreiben** verfügt.

### Abfragefelder

Die Abfrage der Felder der Programmteilnehmer ist unkompliziert. Sie können ein einzelnes Feld für Programmmitglieder nach API-Namen abfragen oder den Satz aller Felder für Programmmitglieder abfragen. Je nach den verwendeten Rollenberechtigungen können sowohl Standardfelder als auch benutzerdefinierte Felder abgerufen werden. Ausgeblendete Felder werden ebenfalls abgerufen.

#### Nach Name

Der Endpunkt [Abrufen des Programmmitgliedsfelds nach Name](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) ruft Metadaten für ein einzelnes Feld im Programmmitgliedsobjekt ab. Der erforderliche `fieldApiName`-Pfadparameter gibt den API-Namen des Felds an. Die Antwort ähnelt dem Endpunkt Programmmitglied beschreiben , enthält jedoch zusätzliche Metadaten wie das `isCustom` , das angibt, ob es sich bei dem Feld um ein benutzerdefiniertes Feld handelt.

```
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

Der Endpunkt [Abrufen von &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET)-Feldern“ ruft Metadaten für alle Felder im Programmmitgliedsobjekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können den `batchSize` Abfrageparameter verwenden, um diese Zahl zu reduzieren. Wenn das Attribut `moreResult` wahr ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult „false“ zurückgibt. Dies bedeutet, dass keine Ergebnisse verfügbar sind. Die von dieser API zurückgegebene `nextPageToken` sollte immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

```
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

Der Endpunkt [Create Program Member Fields](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) erstellt ein oder mehrere benutzerdefinierte Felder im Programmmitgliedsobjekt. Dieser Endpunkt bietet Funktionen, die mit denen vergleichbar sind, die [in der Marketo Engage-Benutzeroberfläche verfügbar](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Sie können mit diesem Endpunkt maximal 20 benutzerdefinierte Felder erstellen.

Achten Sie sorgfältig auf jedes Feld, das Sie in Ihrer Produktionsinstanz von Marketo Engage mithilfe der -API erstellen. Nachdem ein Feld erstellt wurde, können Sie es nicht mehr löschen ([&#x200B; kann nur ausgeblendet werden](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). Die Zunahme von nicht verwendeten Feldern ist eine schlechte Praxis, die Ihre Instanz überladen wird.

Der erforderliche `input` ist ein Array von Feldobjekten für Programmmitglieder. Jedes Objekt enthält ein oder mehrere Attribute. Erforderliche Attribute sind die `displayName`, `name` und `dataType`, die dem Anzeigenamen der Benutzeroberfläche des Felds, dem API-Namen des Felds bzw. dem Feldtyp entsprechen. Optional können Sie `description`, `isHidden`, `isHtmlEncodingInEmail` und `isSensitive` angeben.

Es gibt einige Regeln für die `name` und `displayName`. Das `name` muss eindeutig sein, mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen oder Unterstriche enthalten. Der *`isplayName` muss eindeutig sein und darf keine Sonderzeichen enthalten. Eine gängige Benennungskonvention besteht darin, [&#x200B; (Binnenmajuskel](https://en.wikipedia.org/wiki/Camel_case#) auf `displayName` anzuwenden, um `name` zu erzeugen. Ein `displayName` von „Mein benutzerdefiniertes Feld“ würde beispielsweise einen `name` von „myCustomField“ erzeugen.

```
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

Der Endpunkt [Programm-Member-](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) aktualisieren ein einzelnes benutzerdefiniertes Feld im Programmmember-Objekt. Im Allgemeinen sind Feldaktualisierungsvorgänge, die über die Marketo Engage-Benutzeroberfläche ausgeführt werden, mithilfe der -API erreichbar. In der folgenden Tabelle sind einige Unterschiede zusammengefasst.

| Attribut | Von API aktualisierbar? | Von der Benutzeroberfläche aktualisierbar? | Von API aktualisierbar? | Von der Benutzeroberfläche aktualisierbar? |
|---|---|---|---|---|
| dataType | nein | nein | nein | ja |
| Beschreibung | ja | ja | ja | ja |
| displayName | nein | nein | ja | ja |
| isCustom | nein | nein | nein | nein |
| isHidden | nein | ja | Ja (wenn von API erstellt) | ja |
| isHtmlEncodingInEmail | ja | ja | ja | ja |
| isSensitive | ja | ja | ja | ja |
| length | nein | nein | nein | nein |
| name | nein | nein | nein | nein |

Der erforderliche `fieldApiName` gibt den API-Namen des zu aktualisierenden Felds an. Der erforderliche `input` ist ein Array, das ein einzelnes Lead-Feld-Objekt enthält. Das Feldobjekt enthält ein oder mehrere Attribute.

```
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

Der [Delete Program Members](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST)-Endpunkt wird zum Löschen von Programmteilnehmer-Datensätzen verwendet. Der erforderliche `programId`-Pfadparameter gibt das Programm an, das zu löschende Member enthält. Der Anfragetext enthält ein `input` Array von Lead-IDs. Maximal 300 Lead-IDs  pro Aufruf zulässig.

Der Endpunkt antwortet mit der `status` „gelöscht“ oder „übersprungen“. Wenn er übersprungen wird, wird auch ein `reasons`-Array eingeschlossen. Der Endpunkt antwortet auch mit einem `seq` Feld, das ein Index ist, mit dem die gesendeten Datensätze mit der Reihenfolge der Antwort korreliert werden können.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
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
