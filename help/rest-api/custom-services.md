---
title: "Benutzerdefinierte Dienste"
feature: REST API
description: "Authentifizierungsberechtigungen mit Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 6%

---


# Benutzerdefinierte Dienste

Ein benutzerdefinierter Dienst stellt Anmeldedaten für die Authentifizierung bei Marketo bereit. Anmeldeinformationen sind erforderlich, um ein Zugriffstoken von Marketo zu erhalten. [Identitätsdienst](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Jeder benutzerdefinierte Dienst ist auf einen einzigen API-Nur-Benutzer beschränkt, von dem er seine Berechtigungen ableitet.

## Rollen

Der erste Schritt beim Erstellen eines benutzerdefinierten Dienstes besteht darin, eine Rolle zu erstellen, die Sie auf den relevanten Nur-API-Benutzer anwenden können. Dies geschieht über die **[!UICONTROL Admin]** > **[!UICONTROL Benutzer und Rollen]** > **[!UICONTROL Rollen]** Menü.

Rollen sind Container für individuelle Berechtigungen, die den Zugriff auf bestimmte Funktionen zulassen oder beschränken. Bei Abonnements, für die die Option Arbeitsbereiche und Partitionen aktiviert ist, werden die Berechtigungen je Arbeitsbereich vergeben. Wenn ein Benutzer über eine Berechtigung in einem Arbeitsbereich, aber nicht in einem anderen verfügt, kann er nur zulässige Aktionen in diesem Arbeitsbereich durchführen. Um eine Rolle zu erstellen, klicken Sie auf die Schaltfläche Neue Rolle .

![Benutzer und Rollen](assets/admin-users-and-roles-roles.png)

Geben Sie Ihrer Rolle einen beschreibenden Namen. Nur-API-Benutzer verfügen über einen bestimmten Berechtigungssatz, der sich von den normalen Benutzerberechtigungen unterscheidet. API-Berechtigungen gibt es in ihrer eigenen Hierarchie unter der Baumstruktur &quot;Access API&quot;.

![Neue Rollenberechtigungen](assets/new-role-access-api-permissions.png)

### Rollenberechtigungen

Nur Berechtigungen in der Gruppe &quot;Access API&quot;werden auf API-Benutzer angewendet, d. h. die Erteilung aller Administratorberechtigungen gewährt einem Benutzer keine API-Berechtigungen.

Überlegen Sie beim Erstellen einer Rolle sorgfältig, welche Aktionen Sie der Anwendung ermöglichen sollten, die diese Rolle verwendet. Gewähren Sie nur die für die Durchführung dieser Aktionen erforderlichen Mindestberechtigungen. Durch die Zulassung eines unnötigen Berechtigungssatzes können Integrationen unerwünschte Aktionen in Ihrem Abonnement durchführen. Sie können die [Berechtigungs-Tool](endpoint-reference.md) um Ihre Mindestberechtigungen zu bestimmen. Die vollständige Liste der [Berechtigungen](#permission_list).

## Benutzer

Nachdem Sie eine Rolle erstellt haben, müssen Sie einen Benutzer &quot;Nur API&quot; erstellen. Nur-API-Benutzer sind ein spezieller Benutzertyp in Marketo, da sie von anderen Benutzern verwaltet werden und nicht für die Anmeldung bei Marketo verwendet werden können. Nur-API-Benutzer können:

- Erstellen benutzerdefinierter Dienste
- Zugriffsberechtigungen für diese Dienste
- Zugriff auf REST-APIs

>[!MORELIKETHIS]
>
>Um einen reinen API-Benutzer zu erstellen, navigieren Sie zum **[!UICONTROL Admin]** > **[!UICONTROL Benutzer und Rollen]** > **[!UICONTROL Benutzer]** Menü und klicken Sie [!UICONTROL Neuen Benutzer einladen].


![Neue Benutzerinformationen](assets/new-user-info.png)

Geben Sie Ihrem Benutzer einen beschreibenden Namen und eine E-Mail-Adresse (diese muss nicht gültig sein), basierend auf dem Dienst und der Anwendung, für die sie verwendet wird. Füllen Sie die erforderlichen Felder im Dialogfeldmenü aus, klicken Sie auf das Kontrollkästchen &quot;Nur API&quot;und weisen Sie dem Benutzer eine Ihrer API-Rollen zu. Dadurch werden dem Benutzer die für diese Rolle festgelegten Berechtigungen zugewiesen.

![Berechtigungen für neue Benutzer](assets/new-user-permissions.png)

Klicken Sie abschließend auf &quot;Senden&quot;, um den reinen API-Benutzer zu erstellen.

Wenn Sie eine neue Anwendung mit Anmeldeinformationen bereitstellen, sollten Sie dringend erwägen, einen neuen Benutzer für den Dienst einzurichten, selbst wenn für sie dieselben Berechtigungen wie für eine andere vorhandene Integration festgelegt sind. Statistiken und Fehler zur Nutzung von API-Aufrufen werden pro Benutzer verfolgt. Die Bereitstellung eines Benutzers für jede Anwendung kann Ihnen dabei helfen, die Nutzung und Probleme mit bestimmten Anwendungen zu isolieren. Dies ist nützlich, wenn Sie bei Problemen mit den täglichen API-Aufrufbeschränkungen oder Fehler aufgrund von API-Aufrufen durch Integrationen feststellen.

## Benutzerdefinierte Dienste

Benutzerdefinierte Dienste stellen die eigentlichen Anmeldeinformationen, die Client-ID und das Client-Geheimnis bereit, die für die Authentifizierung mit einer Marketo-Instanz erforderlich sind. Gehen Sie zur Bereitstellung eines Programms zu Ihrem **[!UICONTROL Admin]** > **[!UICONTROL Integrationen]** > **[!UICONTROL LaunchPoint]** und wählen Sie **[!UICONTROL Neuer Dienst]**.

Geben Sie Ihrem Dienst einen beschreibenden Namen und wählen Sie in der Liste &quot;Dienst&quot;die Option &quot;Benutzerdefiniert&quot;aus. Geben Sie dem Dienst eine ausführliche Beschreibung und wählen Sie einen geeigneten Benutzer aus der Liste &quot;Nur API-Benutzer&quot;aus und klicken Sie dann auf [!UICONTROL Erstellen].

![Neuer benutzerdefinierter Dienst](assets/admin-launchpoint-new-service.png)

Dadurch wird Ihrer Liste der LaunchPoint-Dienste ein neuer Dienst und die Option &quot;Details anzeigen&quot;hinzugefügt. Klicken Sie auf &quot;Details anzeigen&quot;. Sie erhalten die Client-ID und das Client-Geheimnis, die für die Authentifizierung erforderlich sind, den Eigentümer-Benutzer und eine Option zum Abrufen des Tokens für kurzfristige Tests. Das Token, das Sie aus diesem Dialogfeld erhalten, hat dieselbe Lebensdauer wie die Token, die normalerweise aus dem [Identitätsdienst](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) und ist 3.600 Sekunden ab der Erstellung gültig.

![Token abrufen](assets/get-token.png)

## Arbeitsbereiche und Partitionen

Bei Abonnements mit Arbeitsbereichen und Partitionen wird die Möglichkeit zum Zugriff auf einen bestimmten Datensatz oder ein bestimmtes Asset basierend auf den Berechtigungen gewährt, die die Rolle eines Benutzers in einem bestimmten Arbeitsbereich besitzt. Jeder Arbeitsbereich erhält Zugriff auf eine oder mehrere Partitionen im Menü Arbeitsbereiche und Partitionen und ein Lead gehört zu einer Partition. Wenn der Nur-API-Benutzer Zugriff auf Lead-Datensätze in einem Arbeitsbereich hat, kann er auf alle Datensätze in Partitionen zugreifen, auf die dieser Arbeitsbereich Zugriff hat.

Assets gehören zu Arbeitsbereichen. Daher wird die Fähigkeit zum Lesen oder Schreiben eines Assets dadurch bestimmt, ob der Benutzer in der entsprechenden Arbeitsfläche über eine Rolle verfügt, die berechtigt ist, diesen Asset-Datensatz in der Arbeitsfläche zu lesen oder zu schreiben.

## Berechtigungsliste

Im Folgenden finden Sie eine Liste aller Berechtigungen, die nur für API-Benutzer verfügbar sind, und was sie einem Benutzer mit dieser Berechtigung erlauben.

| Rollenberechtigung | Gewährt Zugriff auf ... |
| --- | --- |
| Assets genehmigen | Genehmigen von Assets |
| Kampagne ausführen | Kampagnen anfordern oder planen |
| Schreibgeschützte Aktivität | Abrufen von Lead-Aktivitäten |
| Metadaten der schreibgeschützten Aktivität | Abrufen von Metadaten für Lead-Aktivitäten |
| Schreibgeschützte Assets | Abrufen von Asset-Details |
| Schreibgeschützte Kampagne | Kampagnendetails abrufen |
| Schreibgeschütztes Unternehmen | Abrufen von Firmendetails |
| Schreibgeschütztes benutzerdefiniertes Objekt | Abrufen benutzerdefinierter Objektdetails |
| Schreibgeschützter Lead | Abrufen von Lead-Details |
| Schreibgeschütztes benanntes Konto | Abrufen von benannten Kontodetails |
| Schreibgeschützte Liste benannter Konten | Abrufen von Details zur benannten Kontoliste |
| Schreibgeschützte Geschäftschance | Opportunity-Details abrufen |
| Schreibgeschützter Verkaufsmitarbeiter | Details zu Vertriebsmitarbeitern abrufen |
| Aktivität mit Lese-/Schreibzugriff | Abrufen und Erstellen von Lead-Aktivitäten |
| Metadaten der Aktivität mit Lese-/Schreibzugriff | Abrufen und Erstellen von Metadaten für Lead-Aktivitäten |
| Assets mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Assets |
| Kampagne mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Kampagnen |
| Unternehmen mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Unternehmen |
| Objekt mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren benutzerdefinierter Objekte |
| Lead mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Lead-Details |
| Benanntes Konto mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren benannter Konten |
| Liste benannter Konten mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren benannter Kontolisten |
| Verkaufschance mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Möglichkeiten |
| Verkaufsmitarbeiter mit Lese-/Schreibzugriff | Verkaufspersonen abrufen, erstellen und aktualisieren |
