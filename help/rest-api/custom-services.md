---
title: Benutzerdefinierte Services
feature: REST API
description: Authentifizierungsdaten mit Marketo.
exl-id: 38b05c4c-4404-4c30-a7cb-d31b28a3a72e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 6%

---

# Benutzerdefinierte Services

Ein benutzerdefinierter Service stellt Anmeldeinformationen zur Authentifizierung bei Marketo bereit. Anmeldeinformationen werden benötigt, um ein Zugriffstoken vom Marketo ([) ](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Jeder benutzerdefinierte Dienst wird auf einen einzelnen Benutzer (nur API) beschränkt, von dem er seine Berechtigungen ableitet.

## Rollen

Der erste Schritt beim Erstellen eines benutzerdefinierten Service besteht darin, eine Rolle zu erstellen, die Sie auf den relevanten Benutzer „Nur API“ anwenden können. Dies erfolgt über das Menü **[!UICONTROL Admin]** > **[!UICONTROL Benutzer &amp; Rollen]** > **[!UICONTROL Rollen]**.

Rollen sind Container für einzelne Berechtigungen, die den Zugriff auf bestimmte Funktionen ermöglichen oder einschränken. In Abonnements mit aktivierten Arbeitsbereichen und Partitionen werden die Berechtigungen pro Arbeitsbereich erteilt. Wenn ein(e) Benutzende(r) in einem Arbeitsbereich über eine Berechtigung verfügt, in einem anderen jedoch nicht, kann er/sie nur zulässige Aktionen in diesem Arbeitsbereich ausführen. Um eine Rolle zu erstellen, klicken Sie auf die Schaltfläche Neue Rolle .

![Benutzer und Rollen](assets/admin-users-and-roles-roles.png)

Geben Sie Ihrer Rolle unbedingt einen beschreibenden Namen. Benutzer, die nur über eine API verfügen, verfügen über einen bestimmten Berechtigungssatz, der von den normalen Benutzerberechtigungen getrennt ist. API-Berechtigungen sind in ihrer eigenen Hierarchie unter der Struktur „Zugriff auf API“ vorhanden.

![Neue Rollenberechtigungen](assets/new-role-access-api-permissions.png)

### Rollenberechtigungen

Nur Berechtigungen in der Gruppe „Zugriff auf API“ werden auf API-Benutzer angewendet, d. h., bei Vergabe aller Administratorberechtigungen werden einem Benutzer keine API-Berechtigungen gewährt.

Überlegen Sie beim Erstellen einer Rolle sorgfältig, welche Aktionen Sie der Anwendung, die sie verwendet, gestatten sollten. Gewähren Sie nur den minimalen Berechtigungssatz, der für die Durchführung dieser Aktionen erforderlich ist. Durch das Zulassen eines unnötig toleranten Berechtigungssatzes können Integrationen unerwünschte Aktionen in Ihrem Abonnement durchführen. Sie können das [Berechtigungs-Tool](endpoint-reference.md) verwenden, um Ihren minimalen Berechtigungssatz zu bestimmen. Siehe die vollständige Liste von [Berechtigungen](#permission_list).

## Benutzer

Nachdem Sie eine Rolle erstellt haben, müssen Sie einen „Nur-API“-Benutzer erstellen. Benutzende, die nur über eine API verfügen, sind in Marketo ein spezieller Benutzertyp, da sie von anderen Benutzenden verwaltet werden und nicht zur Anmeldung bei Marketo verwendet werden können. Benutzer, die nur über eine API verfügen, können:

- Erstellen benutzerdefinierter Services
- Zugriffsberechtigungen für diese Services
- Zugriff auf REST-APIs

>[!MORELIKETHIS]
>
>Um einen Benutzer nur mit API zu erstellen, gehen Sie zum Menü **[!UICONTROL Admin]** > **[!UICONTROL Benutzer und Rollen]** > **[!UICONTROL Benutzer]** und klicken Sie auf [!UICONTROL Neuen Benutzer einladen].


![Neue Benutzerinformationen](assets/new-user-info.png)

Geben Sie Ihrem Benutzer einen beschreibenden Namen und eine E-Mail-Adresse (sie muss nicht gültig sein), basierend auf dem Service und der Anwendung, für die sie verwendet werden soll. Füllen Sie die erforderlichen Felder im Dialogfeldmenü aus, klicken Sie auf das Kontrollkästchen „Nur API“ und weisen Sie dem Benutzer eine Ihrer API-Rollen zu. Dadurch werden die für diese Rolle festgelegten Berechtigungen dem Benutzer zugewiesen.

![Berechtigungen für neue Benutzer](assets/new-user-permissions.png)

Klicken Sie abschließend auf „Senden“, um den Benutzer „Nur API“ zu erstellen.

Erwägen Sie bei der Bereitstellung einer neuen Anwendung mit Anmeldeinformationen dringend, einen neuen Benutzer für den Service zu ernennen, selbst wenn er denselben Berechtigungssatz wie eine andere vorhandene Integration hat. Statistiken und Fehler zur Nutzung von API-Aufrufen werden benutzerspezifisch verfolgt, sodass die Bereitstellung eines Benutzers für jede Anwendung dabei hilft, Nutzungsszenarien und Probleme auf bestimmte Anwendungen zu isolieren. Dies ist praktisch, wenn Probleme beim Erzielen der täglichen API-Aufrufbeschränkungen auftreten oder Fehler aufgrund von API-Aufrufen auftreten, die von Integrationen vorgenommen werden.

## Benutzerdefinierte Services

Benutzerdefinierte Services stellen die tatsächlichen Anmeldeinformationen, die Client-ID und das Client-Geheimnis, bereit, die erforderlich sind, um eine Authentifizierung mit einer Marketo-Instanz durchzuführen. Um einen Service bereitzustellen, gehen Sie zum Menü **[!UICONTROL Admin]** > **[!UICONTROL Integrationen]** > **[!UICONTROL LaunchPoint]** und wählen Sie **[!UICONTROL Neuer Service]**.

Geben Sie Ihrem Dienst einen beschreibenden Namen und wählen Sie in der Liste „Dienst“ die Option „Benutzerdefiniert“ aus. Geben Sie Ihrem Service eine ausführliche Beschreibung, wählen Sie einen geeigneten Benutzer aus der Liste Nur API-Benutzer aus und klicken Sie auf [!UICONTROL Erstellen].

![Neuer benutzerdefinierter Service](assets/admin-launchpoint-new-service.png)

Dadurch wird Ihrer Liste der LaunchPoint-Services ein neuer Service hinzugefügt und die Option „Details anzeigen“. Klicken Sie auf „Details anzeigen“ und Sie erhalten die Client-ID und das Client-Geheimnis, die für die Authentifizierung erforderlich sind, den Eigentümer der Benutzerin bzw. des Benutzers und eine Option zum Abrufen des Tokens für kurzfristige Testzwecke. Das Token, das Sie aus diesem Dialogfeld erhalten, hat dieselbe Lebensdauer wie Token, die normalerweise vom [Identity Service](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) abgerufen werden, und ist nach der Erstellung 3.600 Sekunden lang gültig.

![Token abrufen](assets/get-token.png)

## Arbeitsbereiche und Partitionen

Bei Abonnements mit Arbeitsbereichen und Partitionen wird die Möglichkeit zum Zugriff auf einen bestimmten Datensatz oder ein bestimmtes Asset auf Grundlage der Berechtigungen gewährt, die die Rolle eines Benutzers in einem bestimmten Arbeitsbereich hat. Jeder Arbeitsbereich erhält im Menü Arbeitsbereiche und Partitionen Zugriff auf eine oder mehrere Partitionen und ein Lead gehört zu einer einzelnen Partition. Wenn der Benutzer „Nur API“ Zugriff auf das Lesen oder Schreiben von Lead-Datensätzen in einem Arbeitsbereich hat, kann er auf alle Datensätze in Partitionen zugreifen, auf die dieser Arbeitsbereich Zugriff hat.

Assets gehören zu Arbeitsbereichen. Daher hängt die Fähigkeit zum Lesen oder Schreiben eines Assets davon ab, ob der/die Benutzende im entsprechenden Arbeitsbereich über eine Rolle verfügt, die zum Lesen oder Schreiben dieses Asset-Datensatztyps im Arbeitsbereich berechtigt ist.

## Berechtigungsliste

Im Folgenden finden Sie eine Liste aller Berechtigungen, die nur für Benutzer mit einer -API verfügbar sind, und der Berechtigungen, die sie einem Benutzer mit dieser Berechtigung erteilen.

| Rollenberechtigung | Gewährt Zugriff auf… |
| --- | --- |
| Assets genehmigen | Assets genehmigen |
| Kampagne ausführen | Kampagne anfordern oder planen |
| Schreibgeschützte Aktivität | Lead-Aktivitäten abrufen |
| Metadaten der schreibgeschützten Aktivität | Abrufen von Metadaten der Lead-Aktivität |
| Schreibgeschützte Assets | Abrufen von Asset-Details |
| Schreibgeschützte Kampagne | Abrufen von Kampagnendetails |
| Schreibgeschütztes Unternehmen | Abrufen von Unternehmensdetails |
| Schreibgeschütztes benutzerdefiniertes Objekt | Abrufen benutzerdefinierter Objektdetails |
| Schreibgeschützter Lead | Lead-Details abrufen |
| Schreibgeschütztes benanntes Konto | Abrufen benannter Kontodetails |
| Schreibgeschützte Liste benannter Konten | Abrufen von Details zur Liste benannter Konten |
| Schreibgeschützte Geschäftschance | Opportunity-Details abrufen |
| Schreibgeschützter Verkaufsmitarbeiter | Details zu Vertriebsperson abrufen |
| Aktivität mit Lese-/Schreibzugriff | Abrufen und Erstellen von Lead-Aktivitäten |
| Metadaten der Aktivität mit Lese-/Schreibzugriff | Abrufen und Erstellen von Metadaten der Lead-Aktivität |
| Assets mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Assets |
| Kampagne mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Kampagnen |
| Unternehmen mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Firmen |
| Objekt mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren benutzerdefinierter Objekte |
| Lead mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Lead-Details |
| Benanntes Konto mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von benannten Konten |
| Liste benannter Konten mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von benannten Kontolisten |
| Verkaufschance mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Opportunities |
| Verkaufsmitarbeiter mit Lese-/Schreibzugriff | Abrufen, Erstellen und Aktualisieren von Vertriebspersonen |
