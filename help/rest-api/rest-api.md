---
title: REST-API
feature: REST API
description: Erfahren Sie, wie Sie die Marketo-REST-API verwenden, API-Benutzer und LaunchPoint einrichten, Kontingente und Beschränkungen anzeigen, sich mit dem Autorisierungs-Header authentifizieren und Leads abrufen.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
TQID: https://experienceleague.adobe.com/GqhWI816wWX-2zf89wWj-GXpg9i615HRFVl2ljdYVj0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: c5f60233-d5ea-4453-a799-0ad258b4d399id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 803
ht-degree: 2%

---

# REST-API

Die Marketo REST-API bietet Remote-Zugriff auf viele Systemfunktionen. Sie können damit Programme erstellen, Leads stapelweise importieren und eine Marketo-Instanz auf einer detaillierten Ebene steuern.

Die REST-APIs lassen sich in zwei Kategorien einteilen:

- [Lead-Datenbank](https://developer.adobe.com/marketo-apis/api/mapi) APIs rufen Personendatensätze aus Marketo und zugehörige Objekttypen wie Opportunities und Unternehmen ab und interagieren mit ihnen.
- [Asset](https://developer.adobe.com/marketo-apis/api/asset)-APIs interagieren mit Marketing-Material und Workflow-bezogenen Datensätzen.

>[!NOTE]
>
>Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. Juli 2026 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo [REST-API](./rest-api.md) durchgeführt werden, und bestehende Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Service zu vermeiden. Wenn Sie über einen Service verfügen, der die SOAP-API verwendet, finden Sie im SOAP-API[Migrationshandbuch](../soap-api/migration.md) Informationen zur Migration.
>

>[!IMPORTANT]
>
>Siehe diesen [Nation-Beitrag](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) über die Einstellung des doppelten Schrägstrichs in API-Gateway-URLs.
>

- **Tägliches Kontingent:** Jedem Abonnement werden 50.000 API-Aufrufe pro Tag zugewiesen. Das Kontingent wird täglich um 0:00 Uhr CST zurückgesetzt. Wenden Sie sich an Ihren Account Manager, um das tägliche Kontingent zu erhöhen.
- **Ratenlimit:** Jede Instanz ist auf 100 API-Aufrufe pro 20 Sekunden beschränkt.
- **Parallelitätslimit:** Jede Instanz ermöglicht maximal zehn gleichzeitige API-Aufrufe.

Standard-API-Aufrufe haben eine maximale URI-Länge von 8 KB und eine maximale Textkörpergröße von 1 MB. Bulk-API-Aufrufe unterstützen eine maximale Textkörpergröße von 10 MB.

Wenn ein Aufruf einen Fehler enthält, gibt die API normalerweise dennoch den HTTP-Status-Code 200 zurück. Die JSON-Antwort enthält ein `success` mit dem Wert `false` und ein Array von Fehlern im `errors`. Weitere Informationen zu Fehlern finden Sie [hier](error-codes.md).

## Erste Schritte

Sie benötigen Administratorrechte in Ihrer Marketo-Instanz, um die folgenden Schritte ausführen zu können. Dieser Workflow erstellt API-Anmeldeinformationen und verwendet sie zum Abrufen eines Lead-Datensatzes.

Erstellen Sie zunächst einen API-Benutzer und rufen Sie Anmeldeinformationen für authentifizierte Aufrufe ab. Melden Sie sich bei Ihrer -Instanz an und gehen Sie zu **[!UICONTROL Admin]** > **[!UICONTROL Benutzer und Rollen]**.

![Admin-Benutzer und -Rollen](assets/admin-users-and-roles.png)

Wählen Sie die Registerkarte **[!UICONTROL Rollen]** und dann Neue Rolle aus. Weisen Sie der Rolle mindestens die Berechtigung „Schreibgeschützter Lead“ (oder „Schreibgeschützte Person„) aus der Gruppe „Zugriffs-API“ zu. Geben Sie der Rolle einen beschreibenden Namen und wählen Sie **[!UICONTROL Erstellen]**.

![Neue Rolle](assets/new-role.png)

Kehren Sie zur Registerkarte [!UICONTROL Benutzer] zurück und wählen Sie **[!UICONTROL Neuen Benutzer einladen]** aus. Geben Sie einen beschreibenden Namen ein, der den Benutzer als API-Benutzer identifiziert, geben Sie eine E-Mail-Adresse ein und wählen Sie **[!UICONTROL Weiter]**.

![Neue Benutzerinformationen](assets/new-user-info.png)

Wählen Sie die Option [!UICONTROL Nur API], weisen Sie die von Ihnen erstellte API-Rolle zu und klicken Sie auf **[!UICONTROL Weiter]**.

![Berechtigungen für neue Benutzer](assets/new-user-permissions.png)

Wählen Sie **[!UICONTROL Senden]** aus, um den Benutzer zu erstellen.

![Nachricht für neuen Benutzer](assets/new-user-message.png)

Gehen Sie dann zum Menü [!UICONTROL Admin] und wählen Sie **[!UICONTROL LaunchPoint]** aus.

![Launchpoint](assets/admin-launchpoint.png)

Wählen Sie **[!UICONTROL Neu]** > **[!UICONTROL Neuer Service]**. Geben Sie einen beschreibenden Namen und eine Beschreibung ein und wählen Sie **[!UICONTROL Benutzerdefiniert]** aus dem Menü [!UICONTROL Service] aus. Wählen Sie den neuen Benutzer aus dem Menü [!UICONTROL Nur API] und klicken Sie dann auf **[!UICONTROL Erstellen]**.

![Neuer Launchpoint-Service](assets/admin-launchpoint-new-service.png)

Wählen Sie **[!UICONTROL Details anzeigen]** für den neuen Service aus, um auf die Client-ID und das Client-Geheimnis zuzugreifen. Wählen Sie **[!UICONTROL Token abrufen]**, um ein Zugriffstoken zu generieren, das eine Stunde gültig ist. Speichern Sie das Token für den ersten API-Aufruf.

![Token abrufen](assets/get-token.png)

Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL Web-Services]**.

![Web-Services](assets/admin-web-services.png)

Suchen Sie den [!UICONTROL Endpunkt] im Feld REST-API und speichern Sie ihn für den ersten API-Aufruf.

![REST-Endpunkt](assets/admin-web-services-rest-endpoint-1.png)

Jeder REST-API-Aufruf muss ein Zugriffstoken in einer HTTP-Kopfzeile enthalten.

```text
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 30. Juni 2025 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

Öffnen Sie eine neue Browser-Registerkarte und geben Sie die folgende URL ein. Ersetzen Sie die Platzhalter durch den Endpunkt und die E-Mail-Adresse für Ihre Instanz, um „Leads [ Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET) aufzurufen.

```text
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Wenn Ihre Datenbank keinen Lead-Eintrag mit Ihrer E-Mail-Adresse enthält, verwenden Sie die E-Mail-Adresse eines bestehenden Leads. Senden Sie die URL, um eine JSON-Antwort ähnlich dem folgenden Beispiel zu erhalten:

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## API-Nutzung

Der API-Nutzungsbericht verfolgt jeden API-Benutzer separat. Wenn Sie jedem Webservice einen separaten Benutzer zuweisen, können Sie die API-Nutzung jeder Integration identifizieren.

Wenn -Aufrufe das Limit Ihrer Instanz überschreiten und nachfolgende -Aufrufe fehlschlagen, verwenden Sie den Bericht, um das Aufrufvolumen aus jedem Service zu identifizieren. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** und wählen Sie die Anzahl der Aufrufe aus, die in den letzten sieben Tagen getätigt wurden.

Die REST-Endpunkte, die die täglichen und die Nutzungs- und Fehlerstatistiken der letzten 7 Tage zurückgeben, finden Sie unter [Nutzung](usage.md).
