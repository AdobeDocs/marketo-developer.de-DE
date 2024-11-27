---
title: Benutzerverwaltung
feature: REST API
description: Führen Sie CRUD-Vorgänge für Benutzerdatensätze durch.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: b89a22e136a7b81a5cd1af9ba7b0caa41c619ff4
workflow-type: tm+mt
source-wordcount: '1180'
ht-degree: 1%

---

# Benutzerverwaltung

[Endpoint-Referenz für die Benutzerverwaltung](https://developer.adobe.com/marketo-apis/api/user/)

Marketo bietet eine Reihe von User Management-Endpunkten, mit denen Sie CRUD-Vorgänge für Benutzerdatensätze in Marketo durchführen können. Benutzer werden durch Senden einer Einladung an einen Benutzer erstellt, der dann ein Kennwort festlegt und zum ersten Mal Zugriff auf Marketo erhält.

Im Gegensatz zu anderen Marketo REST-APIs bei der Verwendung der User Management-APIs:

- Sie müssen die HTTP-Header-Methode verwenden, um das Zugriffstoken zur Authentifizierung zu senden. Sie können das Zugriffstoken nicht als Abfragezeichenfolgenparameter übergeben. Weitere Informationen zur Authentifizierung finden Sie unter [hier](authentication.md).
- Sie müssen beim Erstellen der Benutzerrolle für [Benutzerspezifischer Dienst](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) für die REST-API eine Rollenberechtigung aus zwei verschiedenen Gruppen auswählen:
   1. Zugriffsberechtigung für Benutzer über die Gruppe [Zugriff auf Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. &quot;Zugriff auf User Management-API&quot;über die Gruppe [Zugriff auf API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Antwortkörper enthalten nicht das boolesche Attribut &quot;success&quot;, das auf den Erfolg oder das Fehlschlagen eines Aufrufs hinweist. Stattdessen müssen Sie den HTTP-Antwortstatus-Code auswerten. Wenn ein Aufruf erfolgreich ist, wird ein Statuscode &quot;200&quot;zurückgegeben. Wenn ein Aufruf fehlschlägt, wird ein Statuscode der Nicht-200-Ebene zurückgegeben, und der Antworttext enthält das standardmäßige Array &quot;errors&quot;mit Fehler-Code und beschreibender Fehlermeldung.
- Das Format der Datums-/Uhrzeitzeichenfolgen ist `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm`. Dies gilt für die folgenden Attribute: `createdAt`, `updatedAt`, `expiresAt`.
- Benutzermanagement-API-Endpunkten wird &quot;/rest&quot;nicht wie anderen Endpunkten vorangestellt.

## Abfrage

Die Unterstützung von Abfragen für die Benutzerverwaltung beinhaltet die Möglichkeit, alle Benutzer, Rollen und Arbeitsbereiche abzurufen. Sie können auch einen einzelnen Benutzerdatensatz nach Benutzer-ID oder einen Rollen-/Arbeitsbereichsdatensatz nach Benutzer-ID abrufen.

### Benutzer nach ID

Der Endpunkt [Benutzer von ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) verwendet einen einzelnen `userid` -Pfadparameter und gibt einen einzelnen Benutzerdatensatz für einen Benutzer zurück, der seine Einladung angenommen hat.

```
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Eingeladener Benutzer nach ID

Der Endpunkt [Eingeladenen Benutzer von ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) verwendet einen einzelnen `userid` -Pfadparameter und gibt einen einzelnen Benutzerdatensatz für einen &quot;ausstehenden&quot;Benutzer zurück (der seine Einladung noch nicht angenommen hat).

```
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Rollen und Arbeitsbereiche nach ID

Der Endpunkt [Rollen und Arbeitsbereiche nach ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) akzeptiert einen einzelnen `userid` -Pfadparameter und gibt eine Liste von Benutzerrollen und Workspace-Datensätzen zurück. Die Antwort enthält ein Array mit einem Objekt, das die Rolle, die Arbeitsbereichs-ID und den Namen für den angegebenen Benutzer enthält.

```
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Benutzer durchsuchen

Der Endpunkt [Get Users](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) gibt eine Liste aller Benutzerdatensätze zurück. Der optionale Parameter `pageSize` ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt. Der Standardwert ist 20. Maximal 200. Der optionale Parameter `pageOffset` ist eine Ganzzahl, die angibt, wo Einträge abgerufen werden sollen. Kann mit `pageSize` verwendet werden. Der Standardwert ist 0.

```
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "jamie@lannister.com",
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@houselannister.com",
    "id": 6785,
    "apiOnly": false
  },
  {
    "userid": "jeoffery@housebaratheon.com",
    "firstName": "Jeoffery",
    "lastName": "Baratheon",
    "emailAddress": "jeoffery@housebaratheon.com",
    "id": 7718,
    "apiOnly": false
  },
  {
    "userid": "rickon@housestark.com",
    "firstName": "Rickon",
    "lastName": "Stark",
    "emailAddress": "rickon@housestark.com",
    "id": 8612,
    "apiOnly": false
  }
]
```

>[!NOTE]
>
>Im obigen Codebeispiel richtet sich das angezeigte `userid` an einen Kunden, der zu Adobe IMS migriert wurde. Für Kunden, die noch migriert werden müssen, wird im Feld `userid` eine normale E-Mail-Adresse angezeigt.

### Rollen durchsuchen

Der Endpunkt [Rollen abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) gibt eine Liste aller Rolleneinträge zurück.

```
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Arbeitsbereiche durchsuchen

Der Endpunkt [Arbeitsbereiche abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) gibt eine Liste aller Workspace-Datensätze zurück.

```
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Benutzer einladen

Bei [integrierten Adobe IMS-Abonnements](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur die Einladung von [Nur-API-Benutzern](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Verwenden Sie stattdessen die [Adobe User Management-API](https://developer.adobe.com/umapi/), um [Standardbenutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) einzuladen.

Der Endpunkt [Benutzer einladen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) , um eine E-Mail-Einladung &quot;Willkommen bei Marketo&quot;an neue Benutzer zu senden. Der E-Mail-Textkörper enthält den Link &quot;Bei Marketo anmelden&quot;, über den der Benutzer zum ersten Mal auf Marketo zugreifen kann. Um die Einladung anzunehmen, klickt der E-Mail-Empfänger auf den Link &quot;Bei Marketo anmelden&quot;, erstellt sein Kennwort und erhält Zugriff auf Marketo. Bis der Annahmevorgang abgeschlossen ist, ist die Einladung &quot;ausstehend&quot; und der Benutzerdatensatz kann nicht bearbeitet werden. Eine ausstehende Einladung läuft sieben Tage nach dem Versand ab. Weitere Informationen zum Verwalten von Benutzern finden Sie [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Parameter werden im Anfragetext im Format `application/json` übergeben.

Die folgenden Parameter sind erforderlich:  0, 1, 2. `emailAddress``firstName``lastName, userRoleWorkspaces` Der Parameter `userRoleWorkspaces` ist ein Array von Objekten, die die Attribute `accessRoleId` und `workspaceId` enthalten.

Der Parameter `userid` ist ein eindeutiger Benutzer-ID-Zeichenfolgenwert, der für die Benutzeranmeldung verwendet wird und als E-Mail-Adresse formatiert werden muss. Wenn der Wert von `userid` nicht in der Anfrage angegeben ist, wird standardmäßig der im Parameter `emailAddress` angegebene Wert verwendet.

Der boolesche Parameter `apiOnly` gibt an, ob der Benutzer ein [Nur-API-Benutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) ist. Der Parameter `expiresAt` gibt an, wann die Benutzeranmeldung abläuft und im W3C ISO-8601-Format formatiert wird (ohne Millisekunden). Wenn der Benutzer nicht in der Anfrage angegeben wird, läuft er nie ab. Der Parameter `reason` ist eine Zeichenfolge, die den Grund für die Einladung des Benutzers beschreibt.

Der Endpunkt gibt bei Erfolg den Wert &quot;true&quot;zurück. Andernfalls wird eine Fehlermeldung zurückgegeben.

```
POST /userservice/management/v1/users/invite.json
```

```
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```
true
```

Nachstehend finden Sie ein Beispiel für die E-Mail-Einladung &quot;Willkommen bei Marketo&quot;, die an den neuen Benutzer gesendet wird. Die Betreffzeile der E-Mail lautet &quot;Marketo Login Information&quot;, der Absender ist die E-Mail-Adresse des reinen API-Benutzers, der mit dem benutzerdefinierten Dienst [REST API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) verknüpft ist, und der Empfänger ist wie in den Parametern firstName, lastName und emailAddress angegeben.

![Einladungs-Benutzer-E-Mail](assets/invite-user-email.png)

Der Benutzer nimmt die Einladung in die E-Mail an, indem er zweimal sein Passwort eingibt und auf die Schaltfläche &quot;KENNWORT ERSTELLEN&quot;klickt. Sie erhält dann erstmalig Zugriff auf Marketo.

## Benutzer aktualisieren

Die Aktualisierungsunterstützung für Benutzer umfasst die Möglichkeit, Benutzerattribute zu aktualisieren oder Benutzer zu löschen. Nur Benutzer, die ihre Einladung angenommen haben, können aktualisiert werden. Attribute werden als Parameter des Anfrageinhalts im Format application/json übergeben.

### Benutzerattribute aktualisieren

Bei [in Adobe IMS integrierten Abonnements](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur die Aktualisierung der Attribute von [Nur API-Benutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) . Um Attribute für [Standardbenutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) zu aktualisieren, verwenden Sie stattdessen die [Adobe User Management-API](https://developer.adobe.com/umapi/) .

Der Endpunkt [Benutzerattribute aktualisieren](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) verwendet einen einzelnen `userid` -Pfadparameter und gibt einen einzelnen Benutzerdatensatz zurück. Der Anfrageinhalt enthält mindestens ein zu aktualisierendes Benutzerattribut: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Benutzer löschen

Bei [in Adobe IMS integrierten Abonnements](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur das Löschen von [Nur API-Benutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Um [Standardbenutzer](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) zu löschen, verwenden Sie stattdessen die [Adobe User Management-API](https://developer.adobe.com/umapi/) .

Der Endpunkt [Benutzer löschen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) akzeptiert einen einzelnen `userid` -Pfadparameter und löscht den entsprechenden Benutzer aus der Instanz. Dies ist ein zerstörerischer Löschvorgang, der nicht rückgängig gemacht werden kann. Bei erfolgreicher Ausführung wird ein Statuscode &quot;200&quot;zurückgegeben. Andernfalls wird eine Fehlermeldung zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Eingeladenen Benutzer löschen

Der Endpunkt [Eingeladenen Benutzer löschen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) verwendet einen einzelnen `userid` -Pfadparameter und löscht den entsprechenden &quot;ausstehenden&quot;Benutzer aus der Instanz (der Benutzer hatte seine Einladung noch nicht angenommen). Dies ist ein zerstörerischer Löschvorgang, der nicht rückgängig gemacht werden kann. Bei erfolgreicher Ausführung wird ein Statuscode &quot;200&quot;zurückgegeben. Andernfalls wird eine Fehlermeldung zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Rollen aktualisieren

Die Aktualisierung der Unterstützung für Rollen beinhaltet die Möglichkeit, Rollen hinzuzufügen und zu löschen. Attribute werden als Parameter des Anfrageinhalts im Format application/json übergeben.

## Rollen hinzufügen

Der Endpunkt [Rollen hinzufügen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) verwendet einen einzelnen `userid` -Pfadparameter und fügt dem entsprechenden Benutzer mindestens eine Benutzerrolle hinzu. Der Anfragetext enthält eine Liste von einem oder mehreren Objekten, von denen jedes ein  `accessRoleId` und ein `workspaceId` -Attribut. Bei erfolgreicher Ausführung wird die gesamte Liste mit `accessRoleId/workspaceId` -Paaren für den angegebenen Benutzer zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Rollen löschen

Der Endpunkt [Rollen löschen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) akzeptiert einen einzelnen `userid` -Pfadparameter und löscht eine oder mehrere Benutzerrollen vom entsprechenden Benutzer. Der Anfragetext enthält eine Liste von einem oder mehreren Objekten, von denen jedes ein  `accessRoleId` und ein `workspaceId` -Attribut. Bei erfolgreicher Ausführung wird die verbleibende Liste der accessRoleId-/workspaceId-Paare für den angegebenen Benutzer zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
