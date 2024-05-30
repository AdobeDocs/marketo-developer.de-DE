---
title: "REST API"
feature: REST API
description: "REST API - Übersicht"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 1%

---


# REST-API

Marketo stellt eine REST-API bereit, die die Remote-Ausführung vieler Systemfunktionen ermöglicht. Von der Erstellung von Programmen bis hin zum Massenimport von Leads gibt es viele Optionen, die eine detaillierte Steuerung einer Marketo-Instanz ermöglichen.

Diese APIs lassen sich im Allgemeinen in zwei große Kategorien einteilen: [Lead-Datenbank](https://developer.adobe.com/marketo-apis/api/mapi/), und [Asset](https://developer.adobe.com/marketo-apis/api/asset/). Lead-Datenbank-APIs ermöglichen das Abrufen und die Interaktion mit Marketo-Personendatensätzen und zugehörigen Objekttypen wie Chancen und Unternehmen. Asset-APIs ermöglichen die Interaktion mit Marketingmaterialien und Workflow-bezogenen Datensätzen.

- **Tägliche Quote:** An die Abonnements werden täglich 50.000 API-Aufrufe gesendet (die täglich um 12.00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Kundenbetreuer erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz auf 100 Aufrufe pro 20 Sekunden begrenzt.
- **Concurrency Limit:**  Maximal zehn gleichzeitige API-Aufrufe.

Die Größe von Standardaufrufen ist auf eine URI-Länge von 8 KB und eine Textgröße von 1 MB beschränkt, obwohl der Hauptteil für unsere Bulk-APIs 10 MB betragen kann. Wenn bei Ihrem -Aufruf ein Fehler auftritt, gibt die API in der Regel weiterhin den Status-Code 200 zurück, aber die JSON-Antwort enthält ein &quot;success&quot;-Mitglied mit dem Wert `false`und ein Array von Fehlern im Element &quot;errors&quot;. Weitere Informationen zu Fehlern [here](error-codes.md).

## Erste Schritte

Für die folgenden Schritte sind Administratorberechtigungen in Ihrer Marketo-Instanz erforderlich.

Bei Ihrem ersten Marketo-Aufruf rufen Sie einen Lead-Datensatz ab. Um mit Marketo arbeiten zu können, müssen Sie API-Anmeldeinformationen abrufen, um authentifizierte Aufrufe an Ihre Instanz durchführen zu können. Melden Sie sich bei Ihrer Instanz an und wechseln Sie zum **[!UICONTROL Admin]** -> **[!UICONTROL Benutzer und Rollen]**.

![Admin-Benutzer und Rollen](assets/admin-users-and-roles.png)

Klicken Sie auf [!UICONTROL Rollen] und anschließend Neue Rolle und weisen Sie mindestens die Berechtigung &quot;Schreibgeschütztes Lead&quot;(oder &quot;Schreibgeschützte Person&quot;) der Rolle in der Access-API-Gruppe zu. Geben Sie ihm einen beschreibenden Namen und klicken Sie auf [!UICONTROL Erstellen].

![Neue Rolle](assets/new-role.png)

Kehren Sie nun zur Registerkarte Benutzer zurück und klicken Sie auf Neuen Benutzer einladen . Geben Sie Ihrem Benutzer einen beschreibenden Namen, der anzeigt, dass es sich um einen API-Benutzer handelt, und eine E-Mail-Adresse und klicken Sie auf **[!UICONTROL Nächste]**.

![Neue Benutzerinformationen](assets/new-user-info.png)

Aktivieren Sie dann die Option Nur API und weisen Sie Ihrem Benutzer die von Ihnen erstellte API-Rolle zu und klicken Sie auf **[!UICONTROL Nächste]**.

![Berechtigungen für neue Benutzer](assets/new-user-permissions.png)

Um den Benutzererstellungsprozess abzuschließen, klicken Sie auf **[!UICONTROL Senden]**.

![Neue Benutzermeldung](assets/new-user-message.png)

Navigieren Sie zum Menü &quot;Admin&quot;und klicken Sie auf **[!UICONTROL LaunchPoint]**.

![Startpunkt](assets/admin-launchpoint.png)

Klicken Sie auf das Menü Neu und wählen Sie [!UICONTROL Neuer Dienst]. Geben Sie Ihrem Dienst einen beschreibenden Namen und wählen Sie &quot;Benutzerdefiniert&quot;aus dem Dropdown-Menü Dienst . Geben Sie eine Beschreibung ein, wählen Sie dann Ihren neuen Benutzer aus dem Dropdown-Menü &quot;Nur API-Benutzer&quot;aus und klicken Sie auf [!UICONTROL Erstellen].

![Neuer Launchpoint-Dienst](assets/admin-launchpoint-new-service.png)

Klicken Sie für Ihren neuen Dienst auf Details anzeigen , um auf die Client-ID und das Client-Geheimnis zuzugreifen. Zunächst können Sie auf die [!UICONTROL Token abrufen] -Schaltfläche, um ein Zugriffstoken zu generieren, das für eine Stunde gültig ist. Speichern Sie das Token zunächst in einer Notiz.

![Token abrufen](assets/get-token.png)

Navigieren Sie dann zum Menü &quot;Admin&quot;und klicken Sie auf **[!UICONTROL Web-Services]**.

![Web-Services](assets/admin-web-services.png)

Suchen Sie den Endpunkt im Feld REST-API und speichern Sie ihn zunächst in einem Hinweis.

![REST-Endpunkt](assets/admin-web-services-rest-endpoint-1.png)

Öffnen Sie eine neue Browser-Registerkarte und geben Sie mithilfe der entsprechenden Informationen Folgendes ein: [Abrufen von Leads nach Filtertyp](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
```

Wenn Sie keinen Lead-Datensatz mit Ihrer E-Mail-Adresse in Ihrer Datenbank haben, ersetzen Sie ihn durch einen Datensatz, von dem Sie wissen, dass er vorhanden ist. Drücken Sie die Eingabetaste in Ihre URL-Leiste und Sie sollten eine JSON-Antwort erhalten, die dieser ähnelt:

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

Jeder Ihrer API-Benutzer wird einzeln im API-Nutzungsbericht aufgeführt. Indem Sie Ihre Webdienste nach Benutzern aufteilen, können Sie die Nutzung jeder Ihrer Integrationen einfach erfassen. Wenn die Anzahl der API-Aufrufe an Ihre Instanz den Grenzwert überschreitet und dazu führt, dass nachfolgende Aufrufe fehlschlagen, können Sie mithilfe dieser Vorgehensweise das Volumen der einzelnen Dienste berücksichtigen und bewerten, wie das Problem gelöst werden kann. Sehen Sie sich Ihre Nutzung an, indem Sie **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** und auf die Anzahl der Aufrufe in den letzten sieben Tagen klicken.
