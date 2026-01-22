---
title: REST-API
feature: REST API
description: Erfahren Sie, wie Sie die Marketo-REST-API verwenden, API-Benutzer und LaunchPoint einrichten, Kontingente und Beschränkungen anzeigen, sich mit dem Autorisierungs-Header authentifizieren und Leads abrufen.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 1%

---

# REST-API

Marketo stellt eine REST-API bereit, die die Remote-Ausführung vieler Systemfunktionen ermöglicht. Von der Erstellung von Programmen bis zum Massenimport von Leads gibt es viele Optionen, die eine feinkörnige Steuerung einer Marketo-Instanz ermöglichen.

Diese APIs lassen sich im Allgemeinen in zwei Kategorien einteilen[ „Lead-](https://developer.adobe.com/marketo-apis/api/mapi/)&quot; und [Asset](https://developer.adobe.com/marketo-apis/api/asset/). Lead-Datenbank-APIs ermöglichen das Abrufen und Interagieren von Marketo-Personendatensätzen und zugehörigen Objekttypen wie Opportunities und Unternehmen. Asset-APIs ermöglichen die Interaktion mit Marketing-Material und Workflow-bezogenen Datensätzen.

>[!NOTE]
>Die SOAP-API wird nicht mehr unterstützt und ist nach dem 31. März 2026 nicht mehr verfügbar. Alle neuen Entwicklungen sollten mit der Marketo [REST-API](./rest-api.md) durchgeführt werden, und bestehende Services sollten bis zu diesem Datum migriert werden, um Unterbrechungen im Service zu vermeiden. Wenn Sie über einen Service verfügen, der die SOAP-API verwendet, finden Sie im SOAP-API[Migrationshandbuch](../soap-api/migration.md) Informationen zur Migration.
>

>[!IMPORTANT]
>Siehe diesen [Nation-Beitrag](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) über die Einstellung des doppelten Schrägstrichs in API-Gateway-URLs.
>

- **Tägliches Kontingent:**-Abonnements werden mit 50.000 API-Aufrufen pro Tag zugewiesen (die täglich um 12 Uhr :00AM zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Account Manager erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz ist auf 100 Aufrufe pro 20 Sekunden beschränkt.
- **Parallelitätslimit:**  Maximal zehn gleichzeitige API-Aufrufe.

Die Größe von Standardaufrufen ist auf eine URI-Länge von 8 KB und eine Textkörpergröße von 1 MB beschränkt, obwohl der Textkörper für unsere Bulk-APIs 10 MB betragen kann. Wenn in bei Ihrem Aufruf ein Fehler auftritt, gibt die API normalerweise trotzdem den Status-Code 200 zurück, aber die JSON-Antwort enthält ein „success“-Element mit dem Wert `false` und ein Array von Fehlern im „errors“-Element. Weitere Informationen zu Fehlern [hier](error-codes.md).

## Erste Schritte

Für die folgenden Schritte sind Administratorrechte für Ihre Marketo-Instanz erforderlich.

Für Ihren ersten Aufruf an Marketo rufen Sie einen Lead-Datensatz ab. Um mit Marketo arbeiten zu können, müssen Sie API-Anmeldeinformationen abrufen, um authentifizierte Aufrufe an Ihre Instanz durchführen zu können. Melden Sie sich bei Ihrer -Instanz an und gehen Sie zu **[!UICONTROL Admin]** > **[!UICONTROL Benutzer und Rollen]**.

![Admin-Benutzer und -Rollen](assets/admin-users-and-roles.png)

Klicken Sie auf **[!UICONTROL Rollen]** und dann auf Neue Rolle und weisen Sie mindestens die Berechtigung „Schreibgeschützter Lead“ (oder „Schreibgeschützte Person„) der Rolle in der Zugriffs-API-Gruppe zu. Geben Sie unbedingt einen beschreibenden Namen an und klicken Sie auf **[!UICONTROL Erstellen]**.

![Neue Rolle](assets/new-role.png)

Kehren Sie nun zur Registerkarte [!UICONTROL Benutzer] zurück und klicken Sie auf **[!UICONTROL Neuen Benutzer einladen]**. Geben Sie Ihrem Benutzer einen beschreibenden Namen, der angibt, dass er ein API-Benutzer ist, und eine E-Mail-Adresse und klicken Sie auf **[!UICONTROL Weiter]**.

![Neue Benutzerinformationen](assets/new-user-info.png)

Aktivieren Sie dann die Option [!UICONTROL Nur API] und weisen Sie Ihrem Benutzer die von Ihnen erstellte API-Rolle zu und klicken Sie auf **[!UICONTROL Weiter]**.

![Berechtigungen für neue Benutzer](assets/new-user-permissions.png)

Um die Benutzererstellung abzuschließen, klicken Sie auf **[!UICONTROL Senden]**.

![Nachricht für neuen Benutzer](assets/new-user-message.png)

Gehen Sie dann zum Menü [!UICONTROL Admin] und klicken Sie auf **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Klicken Sie auf das **[!UICONTROL Neu]**-Menü und wählen Sie **[!UICONTROL Neuer Service]**. Geben Sie Ihrem Service einen beschreibenden Namen und wählen Sie **[!UICONTROL Benutzerdefiniert]** aus dem [!UICONTROL Service] Dropdown-Menü aus. Geben Sie eine Beschreibung ein, wählen Sie dann Ihren neuen Benutzer aus dem Dropdown-Menü [!UICONTROL Nur API] und klicken Sie auf **[!UICONTROL Erstellen]**.

![Neuer Launchpoint-Service](assets/admin-launchpoint-new-service.png)

Klicken Sie **[!UICONTROL Details anzeigen]** für Ihren neuen Service, um auf die Client-ID und den geheimen Client-Schlüssel zuzugreifen. Aktuell können Sie auf die Schaltfläche **[!UICONTROL Token abrufen]** klicken, um ein Zugriffs-Token zu generieren, das eine Stunde lang gültig ist. Speichern Sie das Token vorerst in einer Notiz.

![Token abrufen](assets/get-token.png)

Gehen Sie dann zum Menü **[!UICONTROL Admin]** und dann zu **[!UICONTROL Web-Services]**.

![Web-Services](assets/admin-web-services.png)

Suchen Sie den [!UICONTROL Endpunkt] im Feld REST-API und speichern Sie ihn in einer Notiz.

![REST-Endpunkt](assets/admin-web-services-rest-endpoint-1.png)

Beim Aufrufen von REST-API-Methoden muss jeder Aufruf ein Zugriffstoken enthalten, damit der Aufruf erfolgreich ist. Das Zugriffstoken muss als HTTP-Kopfzeile gesendet werden.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>Die Unterstützung für die Authentifizierung mit dem **access_token**-Abfrageparameter wird am 30. Juni 2025 entfernt. Wenn Ihr Projekt einen Abfrageparameter verwendet, um das Zugriffstoken zu übergeben, sollte es so bald wie möglich aktualisiert werden, um die **Authorization**-Kopfzeile zu verwenden. Für die neue Entwicklung sollte ausschließlich der **Authorization**-Header verwendet werden.

Öffnen Sie eine neue Browser-Registerkarte und geben Sie Folgendes ein, indem Sie die entsprechenden Informationen verwenden, um aufzurufen [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Wenn Sie keinen Lead-Eintrag mit Ihrer E-Mail-Adresse in Ihrer Datenbank haben, ersetzen Sie ihn durch einen anderen, von dem Sie wissen, dass er dort ist. Drücken Sie die Eingabetaste in Ihrer URL-Leiste und Sie sollten eine JSON-Antwort erhalten, die der folgenden ähnelt:

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

Jeder Ihrer API-Benutzer wird einzeln im API-Nutzungsbericht gemeldet, sodass Sie durch die Aufspaltung Ihrer Web-Services nach Benutzer einfach die Nutzung jeder Ihrer Integrationen erfassen können. Wenn die Anzahl der API-Aufrufe an Ihre Instanz das Limit überschreitet und nachfolgende Aufrufe fehlschlagen, können Sie mit dieser Vorgehensweise das Volumen aus jedem Ihrer Services berücksichtigen und bewerten, wie Sie das Problem beheben können. Überprüfen Sie Ihre Nutzung, indem Sie zu **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** wechseln und auf die Anzahl der Aufrufe in den letzten sieben Tagen klicken.
