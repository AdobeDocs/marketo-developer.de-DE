---
title: "Authentifizierung"
feature: REST API
description: '"Authentifizierung von Marketo-Benutzern für die API-Nutzung".'
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---


# Authentifizierung

Marketos REST-APIs sind mit 2-Legierungen OAuth 2.0 authentifiziert. Client-IDs und Client-Geheimnisse werden durch von Ihnen definierte benutzerdefinierte Dienste bereitgestellt. Jeder benutzerdefinierte Dienst gehört einem Nur-API-Benutzer, der über verschiedene Rollen und Berechtigungen verfügt, die den Dienst für die Durchführung bestimmter Aktionen autorisieren. Ein Zugriffstoken ist mit einem einzelnen benutzerdefinierten Dienst verknüpft. Der Ablauf von Zugriffstoken ist unabhängig von Token, die mit anderen benutzerdefinierten Diensten verknüpft sind, die in einer Instanz vorhanden sein können.

## Erstellen eines Zugriffstokens

Die `Client ID` und `Client Secret` finden Sie im Abschnitt **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL LaunchPoint]** durch Auswahl des benutzerdefinierten Dienstes und Klicken auf **[!UICONTROL Details anzeigen]**.

![REST-Dienstdetails abrufen](assets/authentication-service-view-details.png)

![Launchpoint-Anmeldedaten](assets/admin-launchpoint-credentials.png)

Die `Identity URL` finden Sie im Abschnitt **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** im Abschnitt REST-API.

Erstellen Sie ein Zugriffstoken mit einer HTTP-GET- (oder POST-)Anfrage wie folgt:

```
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Wenn Ihre Anfrage gültig war, erhalten Sie eine JSON-Antwort ähnlich der folgenden:

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

Antwortdefinition

- `access_token` - Das Token, das Sie mit nachfolgenden Aufrufen übergeben, um sich bei der Zielinstanz zu authentifizieren.
- `token_type` - Die OAuth-Authentifizierungsmethode.
- `expires_in` - Die verbleibende Lebensdauer des aktuellen Tokens in Sekunden (danach ist es ungültig). Wenn ein Zugriffstoken ursprünglich erstellt wurde, beträgt seine Lebensdauer 3600 Sekunden oder eine Stunde.
- `scope` - Der Besitzer des benutzerdefinierten Dienstes, der für die Authentifizierung verwendet wurde.

## Verwenden eines Zugriffstokens

Bei Aufrufen von REST-API-Methoden muss in jedem Aufruf ein Zugriffstoken enthalten sein, damit der Aufruf erfolgreich ist.

Sie können zwei Methoden verwenden, um ein Token in Ihre Aufrufe einzuschließen: als HTTP-Header oder als Abfragezeichenfolgenparameter:

1. HTTP-Header

   `Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int`

1. Abfrageparameter

   `access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

## Tipps und Best Practices

Das Verwalten des Zugriffstoken-Ablaufs ist wichtig, um sicherzustellen, dass Ihre Integration reibungslos funktioniert und dass während des normalen Betriebs keine unerwarteten Authentifizierungsfehler auftreten. Achten Sie beim Erstellen der Authentifizierung für Ihre Integration darauf, das Token und den Ablaufzeitraum zu speichern, die in der Identitätsantwort enthalten sind.

Bevor Sie einen REST-Aufruf durchführen, sollten Sie die Gültigkeit des Tokens auf der Grundlage seiner verbleibenden Lebensdauer überprüfen. Wenn das Token abgelaufen ist, verlängern Sie es, indem Sie [Identität](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET)-Endpunkt. Dadurch wird sichergestellt, dass Ihr REST-Aufruf nie aufgrund eines abgelaufenen Tokens fehlschlägt. Auf diese Weise können Sie die Latenz Ihrer REST-Aufrufe vorhersehbar verwalten, was für Endbenutzer-orientierte Anwendungen von entscheidender Bedeutung ist.

Wenn ein abgelaufenes Token zum Authentifizieren eines REST-Aufrufs verwendet wird, schlägt der REST-Aufruf fehl und gibt einen 602-Fehler-Code zurück. Wenn ein ungültiges Token zum Authentifizieren eines REST-Aufrufs verwendet wird, wird ein 601-Fehlercode zurückgegeben. Wenn einer dieser Codes empfangen wird, sollte der Client das Token durch Aufruf des Identity-Endpunkts verlängern.

Wenn Sie den Identitäts-Endpunkt aufrufen, bevor Ihr Token abgelaufen ist, werden in der Antwort dasselbe Token und die verbleibende Lebensdauer zurückgegeben.

Beachten Sie, dass Ihre Zugriffstoken pro benutzerspezifischem Service und nicht auf Benutzerbasis gehören. Obwohl zwei Identitätsantworten auf denselben Benutzer übertragen werden können, sind die Zugriffstoken und Ablaufzeiträume unabhängig voneinander, wenn sie mit Anmeldeinformationen von zwei verschiedenen Diensten erstellt wurden. Beachten Sie dies bei mehreren Anmeldesätzen in derselben Anwendung. Die Client-ID kann ein nützlicher Schlüssel für die unabhängige Verwaltung sein.
