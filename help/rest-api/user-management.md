---
title: Benutzerverwaltung
feature: REST API
description: Durchführen von CRUD-Vorgängen für Benutzerdatensätze.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1180'
ht-degree: 1%

---

# Benutzerverwaltung

[User Management-Endpunkt-Referenz](https://developer.adobe.com/marketo-apis/api/user/)

Marketo bietet eine Reihe von User Management-Endpunkten, mit denen Sie CRUD-Vorgänge für Benutzerdatensätze in Marketo durchführen können. Benutzer werden erstellt, indem eine Einladung an einen Benutzer gesendet wird, der dann ein Kennwort festlegt und zum ersten Mal Zugriff auf Marketo erhält.

Im Gegensatz zu anderen Marketo-REST-APIs sollten Sie bei der Verwendung der User Management-APIs Folgendes beachten:

- Sie müssen die HTTP-Header-Methode verwenden, um das Zugriffstoken zur Authentifizierung zu senden. Zugriffstoken kann nicht als Abfragezeichenfolgenparameter übergeben werden. Weitere Informationen zur Authentifizierung finden Sie [hier](authentication.md).
- Sie müssen eine Rollenberechtigung aus zwei verschiedenen Gruppen auswählen, wenn Sie die Benutzerrolle für die REST-API [Benutzerdefinierter ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api)) erstellen:
   1. Berechtigung „Zugriff auf Benutzer“ aus der Gruppe [Zugriff auf Admin](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. „Zugriff auf User Management-API“ aus der Gruppe [Zugriff-API](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Antworttexte enthalten nicht das boolesche Attribut „success“, das den Erfolg oder Misserfolg eines Aufrufs angibt. Stattdessen müssen Sie den HTTP-Antwort-Status-Code auswerten. Wenn ein Aufruf erfolgreich ist, wird ein Status-Code von 200 zurückgegeben. Wenn ein Aufruf fehlschlägt, wird ein Status-Code ungleich 200 zurückgegeben und der Antworttext enthält das standardmäßige Array „errors“ mit Fehlercode und einer beschreibenden Fehlermeldung.
- Das Format von Datums-/Uhrzeitzeichenfolgen ist `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm`. Dies gilt für die folgenden Attribute: `createdAt`, `updatedAt`, `expiresAt`.
- User Management-API-Endpunkte haben nicht das Präfix &quot;/rest“ wie andere Endpunkte.

## Abfrage

Die Abfrageunterstützung für die Benutzerverwaltung umfasst die Möglichkeit, alle Benutzer, Rollen und Arbeitsbereiche abzurufen. Außerdem können Sie einen einzelnen Benutzerdatensatz nach Benutzer-ID oder einen Rollen-/Arbeitsbereich-Datensatz nach Benutzer-ID abrufen.

### Benutzer nach ID

Der Endpunkt [Benutzer nach ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) nimmt einen einzelnen `userid`-Pfadparameter und gibt einen einzelnen Benutzerdatensatz für einen Benutzer zurück, der seine Einladung akzeptiert hat.

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

Der Endpunkt [Eingeladenen Benutzer nach ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) nimmt einen einzelnen `userid`-Pfadparameter und gibt einen einzelnen Benutzerdatensatz für einen „ausstehenden“ Benutzer zurück (hat die Einladung noch nicht angenommen).

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

Der Endpunkt [Rollen und Arbeitsbereiche nach ID abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) nimmt einen einzelnen `userid`-Pfadparameter und gibt eine Liste von Benutzerrollen- und Arbeitsbereichsdatensätzen zurück. Die Antwort enthält ein -Array mit einem -Objekt, das die Rollen- und Arbeitsbereich-ID sowie den Namen für den angegebenen Benutzer enthält.

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

Der Endpunkt [Benutzer abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) gibt eine Liste aller Benutzerdatensätze zurück. Der optionale `pageSize` ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt. Der Standardwert ist 20. Maximal 200. Der optionale `pageOffset` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann mit `pageSize` verwendet werden. Der Standardwert ist 0.

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
>Im obigen Code-Beispiel ist der angezeigte `userid` für einen Kunden, der zu Adobe IMS migriert wurde. Für Kunden, die noch nicht migriert haben, wird im Feld `userid` eine reguläre E-Mail-Adresse angezeigt.

### Rollen durchsuchen

Der Endpunkt [Rollen abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) gibt eine Liste aller Rollendatensätze zurück.

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

### Durchsuchen von Arbeitsbereichen

Der Endpunkt [Arbeitsbereiche abrufen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) gibt eine Liste aller Arbeitsbereich-Datensätze zurück.

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

Bei [Adobe IMS-integrierten Abonnements](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur Einladungen von [nur API-](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user)). Um [Standardbenutzer](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) einzuladen, verwenden Sie stattdessen die [Adobe User Management-](https://developer.adobe.com/umapi/).

Der Endpunkt [Benutzer einladen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) zum Senden einer E-Mail-Einladung „Willkommen bei Marketo&quot; an neue Benutzende. Der Textkörper der E-Mail enthält einen Link „Bei Marketo anmelden“, über den der Benutzer zum ersten Mal auf Marketo zugreifen kann. Um die Einladung anzunehmen, klickt der E-Mail-Empfänger auf den Link „Bei Marketo anmelden“, erstellt sein Kennwort und erhält Zugriff auf Marketo. Bis zum Abschluss des Annahmevorgangs ist die Einladung „ausstehend“ und der Benutzerdatensatz darf nicht bearbeitet werden. Eine ausstehende Einladung läuft sieben Tage nach dem Versand ab. Weitere Informationen zur Benutzerverwaltung finden Sie [hier](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Parameter werden im Anfragetext im `application/json` übergeben.

Die folgenden Parameter sind erforderlich:  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. Der `userRoleWorkspaces` ist ein Array von Objekten, die `accessRoleId`- und `workspaceId` enthalten.

Der `userid`-Parameter ist ein eindeutiger Zeichenfolgenwert für die Benutzerkennung, der für die Benutzeranmeldung verwendet wird und als E-Mail-Adresse formatiert sein muss. Wenn in der Anfrage nicht angegeben, wird der Wert von `userid` standardmäßig auf den in `emailAddress` Parameter angegebenen Wert festgelegt.

Der boolesche `apiOnly` gibt an, ob es sich bei dem Benutzer um einen [API-only-Benutzer) ](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Der `expiresAt`-Parameter gibt an, wann die Benutzeranmeldung abläuft, und ist mit dem W3C ISO-8601-Format (ohne Millisekunden) formatiert. Wenn dies nicht in der Anfrage angegeben wird, läuft der Benutzer nie ab. Der `reason` ist eine Zeichenfolge, die den Grund für die Benutzereinladung beschreibt.

Der Endpunkt gibt bei Erfolg den Wert „true“ zurück, andernfalls wird eine Fehlermeldung zurückgegeben.

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

Nachfolgend finden Sie ein Beispiel für die E-Mail-Einladung „Willkommen bei Marketo&quot;, die an den neuen Benutzer gesendet wird. Die Betreffzeile der E-Mail lautet &quot;Marketo-Anmeldeinformationen“. Der Absender ist die E-Mail-Adresse des nur mit der [REST-API verknüpften benutzerdefinierten Service](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) und der Empfänger entspricht den Angaben in den Parametern „firstName“, „lastName“ und „emailAddress“.

![E-Mail zur Benutzereinladung](assets/invite-user-email.png)

Die Benutzerin bzw. der Benutzer akzeptiert die E-Mail-Einladung, indem sie bzw. er ihr Passwort zweimal eingibt und auf die Schaltfläche „PASSWORT ERSTELLEN“ klickt. Danach erhält sie zum ersten Mal Zugang zu Marketo.

## Benutzer aktualisieren

Die Aktualisierungsunterstützung für Benutzer umfasst die Möglichkeit, Benutzerattribute zu aktualisieren oder einen Benutzer zu löschen. Nur Benutzer, die ihre Einladung angenommen haben, können aktualisiert werden. Attribute werden als Parameter an den Anfragetext im application/json-Format übergeben.

### Benutzerattribute aktualisieren

Bei [Adobe IMS-integrierten Abonnements](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur die Aktualisierung von Attributen [nur API-](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) Benutzer. Um Attribute für [Standardbenutzer“ zu aktualisieren](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) verwenden Sie stattdessen die [Adobe User Management-](https://developer.adobe.com/umapi/).

Der [Endpunkt Benutzerattribute aktualisieren](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) nimmt einen einzelnen `userid` und gibt einen einzelnen Benutzerdatensatz zurück. Der Anfragetext enthält ein oder mehrere zu aktualisierende Benutzerattribute: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

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

Bei [Adobe IMS-integrierten Abonnements](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) unterstützt dieser Endpunkt nur das Löschen von [Nur-API-](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user)). Um [Standardbenutzer](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) zu löschen, verwenden Sie stattdessen die [Adobe User Management-](https://developer.adobe.com/umapi/).

Der [Delete User](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST)-Endpunkt verwendet einen einzelnen `userid` und löscht den entsprechenden Benutzer aus der -Instanz. Dies ist ein destruktiver Löschvorgang und kann nicht rückgängig gemacht werden. Bei Erfolg wird ein Status-Code von 200 zurückgegeben. Andernfalls wird eine Fehlermeldung zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Eingeladenen Benutzer löschen

Der Endpunkt [Eingeladenen Benutzer löschen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) verwendet einen einzelnen `userid`-Pfadparameter und löscht den entsprechenden „ausstehenden“ Benutzer aus der Instanz (der Benutzer hatte die Einladung noch nicht angenommen). Dies ist ein destruktiver Löschvorgang und kann nicht rückgängig gemacht werden. Bei Erfolg wird ein Status-Code von 200 zurückgegeben. Andernfalls wird eine Fehlermeldung zurückgegeben.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Aufgabengebiete aktualisieren

Die Aktualisierungsunterstützung für Rollen bietet die Möglichkeit, Rollen hinzuzufügen und zu löschen. Attribute werden als Parameter an den Anfragetext im application/json-Format übergeben.

## Funktionen hinzufügen

Der Endpunkt [Rollen hinzufügen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) nimmt einen einzelnen `userid` und fügt dem entsprechenden Benutzer eine oder mehrere Benutzerrollen hinzu. Der Anfragetext enthält eine Liste eines oder mehrerer Objekte, von denen jedes einen  `accessRoleId` und ein `workspaceId`. Bei Erfolg wird die gesamte Liste der `accessRoleId/workspaceId` für den angegebenen Benutzer zurückgegeben.

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

Der [Delete Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST)-Endpunkt nimmt einen einzelnen `userid` und löscht eine oder mehrere Benutzerrollen aus dem entsprechenden Benutzer. Der Anfragetext enthält eine Liste eines oder mehrerer Objekte, von denen jedes einen  `accessRoleId` und ein `workspaceId`. Bei Erfolg wird die verbleibende Liste der accessRoleId/workspaceId-Paare für den angegebenen Benutzer zurückgegeben.

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
