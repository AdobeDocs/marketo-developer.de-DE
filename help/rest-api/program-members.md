---
title: "Programmmitglieder"
feature: REST API
description: "Erstellen und verwalten Sie Programmmitglieder."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Programm-Mitglieder

[Endpunktverweis für Programmmitglieder](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo stellt APIs zum Lesen, Erstellen, Aktualisieren und Löschen von Programmmitgliedern bereit. Programmmitglieder beziehen sich auf Lead-Datensätze über das Lead-ID-Feld. Die Datensätze bestehen aus einem Satz von Standardfeldern und optional bis zu 20 zusätzlichen benutzerdefinierten Feldern. Die Felder enthalten für jedes Mitglied programmspezifische Daten und können in Formularen, Filtern, Triggern und Flusshandlungen verwendet werden. Diese Daten sind im Programm [Registerkarte &quot;Mitglieder&quot;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) in der Marketo Engage-Benutzeroberfläche.

## Beschreibung

Die [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) Endpunkt folgt dem Standardmuster für Lead-Datenbank-Objekte. Die `searchableFields` -Array gibt Ihnen die für die Abfrage gültigen Felder an. Die `fields` -Array enthält Feldmetadaten, einschließlich REST-API-Name, Anzeigename und Aktualisierbarkeit der Felder.

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

## Anfrage

Die [Abrufen von Programmmitgliedern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) -Endpunkt ermöglicht es Ihnen, Mitglieder eines Programms abzurufen. Sie erfordert eine `programId` Pfadparameter und `filterType` und `filterValues` Abfrageparameter.

`programId` wird verwendet, um anzugeben, welches Programm durchsucht werden soll.

`filterType` wird verwendet, um anzugeben, welches Feld als Suchfilter verwendet werden soll. Es akzeptiert alle Felder in der Liste &quot;searchableFields&quot;, die von der [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) -Endpunkt. Wenn Sie einen filterType angeben, der ein benutzerdefiniertes Feld ist, muss der dataType des benutzerdefinierten Felds entweder &quot;string&quot;oder &quot;integer&quot;lauten. Wenn Sie einen anderen filterType als &quot;leadId&quot;angeben, können durch die Anfrage maximal 100.000 Programmmitgliedern verarbeitet werden. Je nachdem, wie Ihre Marketo-Instanz konfiguriert ist, erhalten Sie einen der folgenden Fehler:

- Wenn die Gesamtzahl der Programmmitglieder 100.000 überschreitet, wird ein Fehler zurückgegeben: &quot;1003, Gesamtmitgliedsgröße: 100.001 überschreitet die zulässige Maximalzahl von 100.000 für den Filter&quot;.
- Wenn die Gesamtzahl der Programmmitglieder _die mit dem Filter übereinstimmen_ über 100.000 hinausgeht, wird ein Fehler zurückgegeben: &quot;1003, Matching membership size: 100.001 überschreitet die zulässige Höchstgrenze (100.000) für diese API.&quot;

Um ein Programm abzufragen, dessen Mitgliedsanzahl das Limit überschreitet, verwenden Sie die Variable [Bulk Program Member Extract API](bulk-program-member-extract.md) anstatt.

`filterValues` wird verwendet, um anzugeben, nach welchen Werten gesucht werden soll, und akzeptiert bis zu 300 Werte in einem kommagetrennten Format. Der Aufruf sucht nach Datensätzen, in denen das Feld des Programmmitglieds mit einem der enthaltenen filterValues übereinstimmt.

Alternativ können Sie nach Datumsbereich filtern, indem Sie `updatedAt` als filterType mit `startAt` und `endAt` datetime-Parameter. Der Zeitraum muss sieben Tage oder weniger betragen. Die Datenzeiten sollten im ISO-8601-Format vorliegen, ohne Millisekunden.

Das optionale `fields` Der Abfrageparameter akzeptiert eine kommagetrennte Liste von Feld-API-Namen, die von der [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) -Endpunkt. Wenn dies eingeschlossen ist, enthält jeder Datensatz in der Antwort die angegebenen Felder. Wenn diese Option weggelassen wird, wird der Standardsatz der zurückgegebenen Felder `acquiredBy`, `leadId`, `membershipDate`, `programId`, und `reachedSuccess`.

Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können die `batchSize` -Abfrageparameter, um diese Zahl zu reduzieren. Wenn die Variable **moreResult** auf &quot;true&quot;gesetzt ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die `nextPageToken` von dieser API zurückgegebenen Daten sollten immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

Wenn die Gesamtlänge Ihrer GET-Anfrage 8 KB überschreitet, wird ein HTTP-Fehler zurückgegeben: &quot;414, URI zu lang&quot;(pro [RFC 7231](https://datatracker.ietf.org/doc/html/rfc72316.5.12)). Als Problemumgehung können Sie Ihre GET in POST ändern, `_method=GET` und platzieren Sie die Abfragezeichenfolge im Anfragetext.

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

## Erstellen und Aktualisieren

Es gibt zwei Endpunkte, die den Vorgang zum Erstellen/Aktualisieren von Programmmitgliedern unterstützen. Mit einem können Sie nur den Status der Programmmitglieder aktualisieren. Die andere Option ermöglicht die Aktualisierung der Felder der Programmmitglieder, die als &quot;aktualisierbar&quot;markiert sind. Mit beiden Endpunkten können Sie bis zu 300 Datensätze von Programmmitgliedern pro Aufruf ändern.

### Programmteilnahmestatus

Die [Status der Programmteilnehmer synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) Endpunkt wird verwendet, um den Programmstatus für ein oder mehrere Mitglieder zu erstellen oder zu aktualisieren.

Die erforderlichen `programId` path Parameter gibt das Programm an, das Mitglieder enthält, die erstellt oder aktualisiert werden sollen.

Die erforderlichen `statusName` gibt den Programmstatus an, der auf eine Liste von Leads angewendet werden soll. Der statusName muss mit dem verfügbaren Status für den Kanal des Programms übereinstimmen. Gültige Status können mit der [Abrufen von Kanälen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) -Endpunkt. Wenn der Status eines Leads einen größeren Schrittwert als der angegebene statusName aufweist, wird dieser Lead übersprungen.

Die erforderlichen `input` -Parameter ist ein Array von `leadId` die den Programmmitgliedern entsprechen. Sie können bis zu 300 leadIds pro Aufruf senden. Für jeden Datensatz wird ein Upsert-Vorgang ausgeführt. Wenn die leadId mit einem Programmmitglied verknüpft ist, wird dessen Mitgliedsstatus aktualisiert. Wenn nicht, wird ein neuer Datensatz mit Programmmitgliedern erstellt, der Datensatz wird mit der leadId verknüpft und der Status der Mitgliedschaft wird zugewiesen.

Der Endpunkt antwortet mit einer `status` von &quot;aktualisiert&quot;, &quot;erstellt&quot;oder &quot;übersprungen&quot;. Wenn übersprungen, wird eine `reasons` -Array enthalten. Der Endpunkt antwortet auch mit einer `seq` -Feld, ein Index, der verwendet werden kann, um die übermittelten Datensätze der Reihenfolge der Antwort zuzuordnen.

Wenn der Aufruf erfolgreich war, wird die Aktivität &quot;Programmstatus ändern&quot;in das Aktivitätsprotokoll des Leads geschrieben.

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

### Programmteilnehmerdaten

Die [Programmteilnehmerdaten synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) Endpunkt wird verwendet, um Felddaten für Programmmitglieder für ein oder mehrere Mitglieder zu aktualisieren. Sie können alle benutzerdefinierten Felder oder Standardfelder ändern, die &quot;aktualisiert werden können&quot;(siehe [Programmteilnehmer beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) -Endpunkt).

Die erforderlichen `programId` path -Parameter gibt das Programm an, das Mitglieder enthält, die aktualisiert werden sollen.

Die erforderlichen `input` -Parameter ist ein Array. Jedes Array-Element enthält eine `leadId` und eines oder mehrere zu aktualisierende Felder (unter Verwendung des API-Namens). Für jeden Datensatz wird ein Aktualisierungsvorgang durchgeführt. Die leadId muss mit einem Programmmitglied verknüpft sein. Die Felder müssen aktualisiert werden können. Sie können bis zu 300 leadIds pro Aufruf senden.

Der Endpunkt antwortet mit einer `status` von &quot;aktualisiert&quot;oder &quot;übersprungen&quot;. Wenn übersprungen, wird eine `reasons` -Array enthalten. Der Endpunkt antwortet auch mit einer `seq` -Feld, ein Index, der verwendet werden kann, um die übermittelten Datensätze der Reihenfolge der Antwort zuzuordnen.

Wenn der Aufruf erfolgreich war, wird die Aktivität &quot;Ändern der Programmteildaten&quot;in das Aktivitätsprotokoll des Leads geschrieben.

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

Das Programmteilobjekt enthält Standardfelder und optionale benutzerdefinierte Felder. Standardfelder sind in jedem Marketo Engage-Abonnement vorhanden, benutzerdefinierte Felder werden vom Benutzer nach Bedarf erstellt. Jede Felddefinition besteht aus einem Satz von Attributen, die das Feld beschreiben. Beispiele für Attribute sind der Anzeigename, der API-Name und der dataType. Diese Attribute werden kollektiv als Metadaten bezeichnet.

Mit den folgenden Endpunkten können Sie Felder für das Programmteilobjekt abfragen, erstellen und aktualisieren. Diese APIs erfordern, dass der Eigentümer-API-Benutzer eine Rolle mit einer oder beiden der **Standardfeld für Lese- und Schreibschema** oder **Benutzerdefiniertes Feld für Lese- und Schreibschema** Berechtigungen.

### Abfragefelder

Die Abfrage der Felder der Programmmitglieder ist unkompliziert. Sie können ein einzelnes Feld für Programmmitglieder nach API-Namen abfragen oder die Gruppe aller Felder für Programmmitglieder abfragen. Je nach den verwendeten Rollenberechtigungen können sowohl Standardfelder als auch benutzerdefinierte Felder abgerufen werden. Ausgeblendete Felder werden ebenfalls abgerufen.

#### Nach Name

Die [Abrufen des Felds &quot;Programmteilnehmer&quot;nach Name](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) -Endpunkt ruft Metadaten für ein einzelnes Feld im Programmmember-Objekt ab. Die erforderlichen `fieldApiName` path parameter gibt den API-Namen des Felds an. Die Antwort entspricht dem Endpunkt Programmteilnehmer beschreiben , enthält jedoch zusätzliche Metadaten wie die `isCustom` -Attribut, das angibt, ob das Feld ein benutzerdefiniertes Feld ist.

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

Die [Abrufen von Programmteilefeldern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) -Endpunkt ruft Metadaten für alle Felder im Programmmember-Objekt ab. Standardmäßig werden maximal 300 Datensätze zurückgegeben. Sie können die `batchSize` -Abfrageparameter, um diese Zahl zu reduzieren. Wenn die Variable `moreResult` auf &quot;true&quot;gesetzt ist, bedeutet dies, dass mehr Ergebnisse verfügbar sind. Rufen Sie diesen Endpunkt so lange auf, bis das Attribut moreResult &quot;false&quot;zurückgibt, was bedeutet, dass keine Ergebnisse verfügbar sind. Die `nextPageToken` von dieser API zurückgegebenen Daten sollten immer für die nächste Iteration dieses Aufrufs wiederverwendet werden.

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

Die [Erstellen von Programmteilefeldern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) -Endpunkt erstellt ein oder mehrere benutzerdefinierte Felder im Programmteilobjekt. Dieser Endpunkt bietet Funktionen, die mit dem [in der Marketo Engage-Benutzeroberfläche verfügbar](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Mit diesem Endpunkt können Sie bis zu 20 benutzerdefinierte Felder erstellen.

Achten Sie sorgfältig darauf, jedes Feld, das Sie in Ihrer Produktionsinstanz von Marketo Engage mithilfe der API erstellen. Nachdem ein Feld erstellt wurde, kann es nicht mehr gelöscht werden ([Sie können sie nur verbergen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). Die Verbreitung nicht verwendeter Felder ist eine schlechte Vorgehensweise, die Ihre Instanz übersichtlicher macht.

Die erforderlichen `input` -Parameter ist ein Array von Feldobjekten mit Programmmitgliedern. Jedes Objekt enthält ein oder mehrere Attribute. Die erforderlichen Attribute sind `displayName`, `name`, und `dataType` die dem Anzeigenamen des Felds in der Benutzeroberfläche, dem API-Namen des Felds und dem Feldtyp entsprechen. Optional können Sie `description`, `isHidden`, `isHtmlEncodingInEmail`, und `isSensitive`.

Es gibt einige Regeln für `name` und `displayName` Benennung. Die `name` muss eindeutig sein, mit einem Brief beginnen und nur Buchstaben, Zahlen oder Unterstriche enthalten. Die *`isplayName` muss eindeutig sein und darf keine Sonderzeichen enthalten. Eine gängige Benennungskonvention besteht darin, [Kamelgehäuse](https://en.wikipedia.org/wiki/Camel_case#) nach `displayName` zu produzieren `name`. Beispiel: eine `displayName` von &quot;Mein benutzerdefiniertes Feld&quot;würde eine `name` von &quot;myCustomField&quot;.

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

Die [Aktualisieren des Programmteilnehmers](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) endpoint aktualisiert ein einzelnes benutzerdefiniertes Feld im Programmmember-Objekt. Im Allgemeinen sind Feldaktualisierungsvorgänge, die über die Marketo Engage-Benutzeroberfläche durchgeführt werden, mit der API erreichbar. Die folgende Tabelle enthält einige Unterschiede.

| Attribut | Mit API aktualisieren? | Aktualisierbar nach Benutzeroberfläche? | Mit API aktualisieren? | Aktualisierbar nach Benutzeroberfläche? |
|---|---|---|---|---|
| dataType | no | no | no | yes |
| Beschreibung | yes | yes | yes | yes |
| displayName | no | no | yes | yes |
| isCustom | no | no | no | no |
| isHidden | no | yes | ja (falls von der API erstellt) | yes |
| isHtmlEncodingInEmail | yes | yes | yes | yes |
| isSensitive | yes | yes | yes | yes |
| length | no | no | no | no |
| name | no | no | no | no |

Die erforderlichen `fieldApiName` path parameter gibt den API-Namen des zu aktualisierenden Felds an. Die erforderlichen `input` -Parameter ist ein Array, das ein einzelnes Lead-Feld-Objekt enthält. Das field -Objekt enthält ein oder mehrere Attribute.

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

Die [Löschen von Programmmitgliedern](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) -Endpunkt wird zum Löschen von Programmmitgliedern-Datensätzen verwendet. Die erforderlichen `programId` path -Parameter gibt das Programm an, das Mitglieder enthält, die gelöscht werden sollen. Der Anfrageinhalt enthält eine `input` Array von Lead-IDs. Maximal 300 Lead-IDs pro Aufruf sind zulässig.

Der Endpunkt antwortet mit einer `status` von &quot;Gelöscht&quot;oder &quot;übersprungen&quot;. Wenn übersprungen, wird eine `reasons` -Array enthalten. Der Endpunkt antwortet auch mit einer `seq` -Feld, ein Index, der verwendet werden kann, um die übermittelten Datensätze der Reihenfolge der Antwort zuzuordnen.

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
