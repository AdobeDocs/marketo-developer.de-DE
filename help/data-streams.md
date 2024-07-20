---
title: Daten-Streams
description: Data Steams - Übersicht
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1594'
ht-degree: 2%

---

# Daten-Streams

Die Marketingorganisationen unserer Kunden setzen auf zeitnahe und zielgerichtete Marketing-Kampagnen, um ihr Geschäft zu übertreffen und wettbewerbsfähig zu sein. Um schnelle Entscheidungen zu unterstützen und einen schnellen strategischen Wandel zu ermöglichen, ist es wichtig, über Daten zu verfügen, die diese wichtigen Entscheidungen unterstützen und vorantreiben, die gezielte und zielgerichtete Kampagnen ermöglichen. Es gibt auch einige Kunden, die Marketingbemühungen auf Ebene ihrer Kundensegmente sowohl innerhalb als auch außerhalb von Marketo Engage durchführen. Um diese unterschiedlichen Bemühungen zu unterstützen, hat Marketo die Möglichkeit geschaffen, große Datenmengen nahezu in Echtzeit über Data Streams zu erfassen.

Neben den Vorteilen von nahezu Echtzeit-Daten gibt es produktbezogene Vorteile:

- Beseitigt den Engpass bei API-Beschränkungen, da stattdessen Streaming verwendet wird.
- Reduziert das Szenario der API-Beschränkungen und generiert weniger Warnhinweis-Nachrichten.
- Aufgrund der Data Streaming-Funktion muss kein Massenexport durchgeführt werden, um Daten zu extrahieren.

Daten-Streams sind für diejenigen verfügbar, die ein [Marketo Engage Performance Level Package](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835) erworben haben.

## Lead-Aktivitäts-Stream-Übersicht

Lead-Aktivitäts-Daten-Stream bietet nahezu Echtzeit-Streaming von Audit-Tracking-Lead-Aktivitäten, bei dem große Mengen von Lead-Aktivitäten an das externe System eines Kunden gesendet werden können. Streams ermöglichen es Kunden, Lead-bezogene Ereignisse und Nutzungsmuster effektiv zu prüfen, Ansichten zu Lead-Änderungen bereitzustellen sowie Trigger- und Workflows basierend auf den verschiedenen Arten von Lead-Ereignissen zu bearbeiten. Es gibt über 144 Aktivitätstypen, die für den Stream abonniert und über diesen gesendet werden können.

Arten von Lead-Daten gestreamt:

1. Lead-Änderungen - alle Änderungen in allen Feldern und neue Leads
1. Lead-Aktivitäten - alle im Dokument beschriebenen Lead-Aktivitätstypen
1. Gelöschte Leads
1. Alle benutzerdefinierten Objekte auf dem Lead (falls angefordert). Es ist jetzt alles oder nichts.

Indem Sie Einblicke in Lead-Änderungen bieten, können Kunden raschere Entscheidungen über ihre gesamten Marketing-Strategien treffen und zielgerichtetere Kampagnen erstellen. Einige gängige Anwendungsfälle wären:

- Benutzerdefinierte Warnhinweise: Wenn bestimmte Leads mit inkonsistenten Bedingungen gefunden werden, können sie der Liste hinzugefügt werden. Aktivitäts-Streams können diese aufrufen und die Aktivität &quot;Zu Liste hinzufügen&quot;an Kunden weitergeben, damit diese Aktionen ausführen können.
- Powering ML Models: Manche Kunden planen, Scoring-Modelle zu erstellen, die Aktivitätseinblicke verwenden und sie an Marketo zurückgeben oder nach Bedarf in ihren eigenen internen Scoring-Modellen verwenden. Durch die Auswertung eines Leads können Kunden Marketo dann informieren, Kunden zu Kurskampagnen hinzuzufügen, um ihre Bewertung zu verbessern.

Liste der gestreamten Aktivitäten:

| AchieveGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | ReferToSocialApp |
| AddToOpportunity | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| CallWebhook | DeleteLead | RequestCampaign |
| ChangeDataValue | DisqualifizierenPreisausschreiben | SalesEmailBounced |
| ChangeLeadPartition | EarnEntryInSocialApp | SendAlert |
| ChangeNurtureCadence | EmailBounced | SendEmail |
| ChangeNurtureTrack | EmailBouncedSoft | SendSalesEmail |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | SFDCActivity |
| ChangeProgramMemberData | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | SignUpForReferralOffer |
| ChangeRevenueStageManuell | FillOutForm | SyncLeadToMicrosoft |
| ChangeScore | InteressantMoment | SyncLeadToSFDC |
| ChangeSegment | MergeLeads | UnsubscribeEmail |
| ChangeStatusInProgression | NewLead | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Beachten Sie, dass bei benutzerdefinierten Objekten, die gestreamt werden sollen, alle Lead-bezogenen benutzerdefinierten Objekte sein müssen. Es gibt derzeit keine Möglichkeit, auszuwählen, welche gewünscht werden.

## Übersicht über Daten-Stream-Daten für Benutzerprüfung

Der Daten-Stream zur Benutzerprüfung bietet eine nahezu Echtzeit-Verfolgung von Asset-Änderungen durch Benutzer-&#x200B;. Dies ermöglicht es dem Kunden, Asset-Ereignisse effektiv zu prüfen, eine Ansicht der Benutzeränderungen bereitzustellen und Trigger-Prozesse oder -Workflows basierend auf verschiedenen Arten von Prüfereignissen bereitzustellen. Fast Asset-Änderungen in Echtzeit werden über Adobe I/O-Ereignisse an einen konfigurierbaren Endpunkt gesendet. Audit-Ereignisse werden nach Asset-Typ aufgeschlüsselt und können Prüfereignisse abonnieren, die für sie wichtig sind.

Ein guter Anwendungsfall für das Abonnieren dieses Streams wäre:

- Verfolgen von Änderungen bei der Verwendung mehrerer Marketing-Systeme: Es gibt einige Kunden, die auch Marketing-Aktivitäten in einem anderen System wie z. B. einem CRM-System wie Salesforce durchführen und dann den Lead an Marketo weiterleiten. Das Lead wird manchmal aktualisiert und hin und her synchronisiert, sodass es wichtig ist zu verfolgen, welches System kürzlich Änderungen vorgenommen hat.

Liste der gestreamten Benutzerprüfungsereignisse:

| KOMPONENTE | LISTE DES EREIGNISTYPS |
|--- |--- |
| Standardprogramm | Klonen, erstellen, löschen, Kanal bearbeiten, exportieren, Programmsetup ändern, Programm-Token ändern, umbenennen |
| E-Mail | Genehmigen, klonen, erstellen, löschen, bearbeiten, verschieben, umbenennen, nicht genehmigen |
| E-Mail-Stapelprogramm | approve, childUpdate, clone, create, delete, edit, edit channel, modify program schedule, modify program setup, modify program token, rename, unapprove |
| E-Mail-Vorlage | Genehmigen, klonen, erstellen, löschen, draftCreate, draftDiscard, bearbeiten, umbenennen, nicht genehmigen |
| Engagementprogramm | Klonen, erstellen, löschen, Kanal bearbeiten, Programmeinrichtung ändern, Programmstream ändern, Programm-Token ändern, umbenennen |
| Veranstaltungsprogramm | Klonen, erstellen, löschen, Kanal bearbeiten, Programmzeitplan ändern, Programmsetup ändern, Programm-Token ändern, umbenennen |
| Ordner | Erstellen, Löschen, Bearbeiten, Umbenennen |
| Formular | Genehmigen, klonen, erstellen, löschen, draftCreate, bearbeiten, verschieben, umbenennen |
| Formular -> Landingpage-Formular | Erstellen, klonen, bearbeiten, löschen, genehmigen, umbenennen |
| Landingpage | Genehmigen, klonen, erstellen, löschen, draftDiscard, bearbeiten, umbenennen, nicht genehmigen |
| Landing Page-Vorlage | Genehmigen, klonen, erstellen, löschen, draftCreate, draftDiscard, bearbeiten, umbenennen, nicht genehmigen |
| Smart List | Klonen, erstellen, löschen, bearbeiten, exportieren, Einstellungen für Smart-Listen ändern, umbenennen |
| Marketingordner | erstellen, bearbeiten, löschen |
| Nurturing-Programm | Klonen, erstellen, löschen, Kanal bearbeiten, Programmeinrichtung ändern, Programmstream ändern, Programm-Token ändern, umbenennen |
| Segment | Erstellen, Löschen, Bearbeiten, Umbenennen |
| Segmentierung | approve, create, delete, draftCreated, draftDiscarded, rename, unapprove |
| Intelligente Kampagne | Abbrechen, Aktivieren, Klonen, Erstellen, Deaktivieren, Löschen, Bearbeiten, Ändern des Kampagnenkalenplans, Ändern der Flussschritt-Aktion, Ändern der Einrichtung der intelligenten Liste, Verschieben, Umbenennen |
| Ausschnitt | genehmigen, mit Entwurf genehmigen, klonen, erstellen, löschen, bearbeiten, umbenennen, nicht genehmigen |
| Admin-Benutzeroberfläche -> LaunchPoint -> Integration | erstellen, löschen, bearbeiten |
| Admin-Benutzeroberfläche -> Benutzer | Erstellen, Bearbeiten, Löschen (nur für API-Benutzer identisch) |
| Admin Login -> User | Anmeldeerfolg, Anmeldefehler |
| Programm -> E-Mail-Batch-Programm | Bearbeiten (zum Ändern der ausgewählten E-Mail-Adresse) Asset-API |
| Programm -> Marketingprogramm | Erstellen, Klonen |

Beispiel eines User Audit-Ereignisses:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"          
        }
    }
}
```

## Übersicht über Benachrichtigungsdaten-Stream

Benachrichtigungsdaten-Stream ist als Teil der Leistungsebenenangebote von Marketo Engage verfügbar.

Derzeit kann das Benachrichtigungszentrum in Marketo so konfiguriert werden, dass Benachrichtigungen an eine E-Mail-Adresse gesendet werden. Benachrichtigungsdaten-Stream ermöglicht das direkte Senden der Benachrichtigungen an einen konfigurierbaren Endpunkt über Adobe I/O-Ereignisse. Benachrichtigungen werden heute über die Benutzeroberfläche bereitgestellt und können über die orangefarbene Glocke oben rechts im Bildschirm referenziert werden. Dieser Stream nimmt diese Benachrichtigungen und sendet sie über einen Stream.

Liste der Benachrichtigungsereignisse:

| KOMPONENTE | LISTE DES EREIGNISTYPS |
|--- |--- |
| Benachrichtigung | Kampagnenabbruch, Kampagnenfehler, Pflege (Programm erschöpft), Fehler bei Salesforce-Synchronisation, Testgruppe (A/B-Testergebnis), Webdienste (tägliches Kontingent) |

Beispiel für ein Benachrichtigungsereignis:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Technische Details

Dieser Abschnitt enthält Richtlinien zu den erforderlichen Elementen, zum Verbinden und Empfangen von Streaming-Daten für jeden Stream. Für jede Komponente ist ein gewisser Grad an Kodierung und Einrichtung erforderlich.

### Lead-Aktivitätsdaten-Stream

Der Lead-Aktivitäts-Stream bietet nahezu Echtzeit-Streaming von Marketo-Lead-Aktivitätsereignissen und sendet abonnierte Aktivitätstypänderungen mit konfigurierbaren Attributen:

- Die Häufigkeit der Datenübermittlungen wird standardmäßig alle 2 Sekunden übertragen.
- Batches von 100 bis 500 pro Abonnement.
- Die Zeitüberschreitung für den REST-Dienst des Kunden beträgt 20 Sekunden, wobei alle 3 Minuten 3 erneute Versuche durchgeführt werden und die automatische Aktivierung bei Erfolg erfolgt. Andernfalls werden sie angehalten. Nach der Pause versucht der Dienst alle 3 Minuten erneut, die Aktivierung vorzunehmen, es sei denn, die Bereitstellung wurde manuell aufgehoben.
- Datenbeibehaltung in einer Warteschlange für bis zu 7 Tage.

Gehen Sie wie folgt vor, um den Lead-Aktivitäts-Datenstream zu implementieren:

1. Stellen Sie einen HTTP-Endpunkt bereit, der POST-Anfragen mit einem JSON-Hauptteil aus dem öffentlichen Internet empfangen kann. Der Aktivitäts-Push-Daten-Stream sendet Anforderungen an:
1. Stellen Sie Adobe wie folgt bereit:
   1. Marketo Munchkin-ID für ihre Anmeldung
   1. Die URL des Endpunkts in Schritt 1
   1. Die Aktivitätstypen, die sie empfangen möchten (vollständige Liste oben)
   1. Eine Authentifizierungsmethode, mit der der Kunde überprüfen kann, ob die Anfragen korrekt sind. Entweder:
      1. Eine Identitäts-Provider-URL, Client-ID und Client Secret für OAuth [Client Credentials Authentication](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Ein API-Token, das in vom Lead-Aktivitäts-Datastream gesendeten Anfragen entweder in Abfrageparametern oder in einem Autorisierungs-Header (Kundenwahl) enthalten sein kann

Adobe aktiviert dann den Datenstrom, an dem Kunden Daten empfangen.

UML-Diagramm eines typischen Data Stream-Aufrufs für die Lead-Aktivität:

![Lead-Aktivitäts-Stream-Diagramm ](assets/lead-activity-data-stream.png)

Beispiel für die Erstellung eines URL-Endpunkts:

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Ein Codebeispiel für eine Anwendung, die den Data Stream der Lead-Aktivität von Marketo nutzt, finden Sie [hier](https://github.com/ihgrant/activity-stream-consumer-example).

### Datenstream und Benachrichtigungsdatenstream für Benutzeraudit

Benutzerprüfungsereignisse werden an Adobe IO gesendet und können durch Anmeldung bei einer Adobe ID genutzt werden. Gehen Sie wie folgt vor:

1. Kunden bieten Adobe mit dem folgenden Angebot:
   1. Adobe-ID
   1. Marketo Munchkin-ID für ihre Anmeldung
1. Der Kunde stellt einen REST-Endpunkt bereit, um Ereignisse normal in Form eines Webhooks zu nutzen.
1. Sobald dies bereitgestellt ist, ermöglicht Adobe den Stream für das Abonnement des Kunden.
1. Der Kunde richtet den Stream dann in Adobe IO ein (Anweisungen sind bereitzustellen).
   1. Für diesen Schritt ist eine Adobe-Organisation erforderlich.
   1. Erfordert, dass Adobe Org User Entwickler- oder Systemadministratorrolle erhält.

Informationen zum Einrichten von Adobe IO finden Sie unter [Einrichten von Marketo User Audit Data Streams mit Adobe IO](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) im Abschnitt Öffentliche Dokumentation .

### Einrichten des Daten-Streams für die Benutzerprüfung in Marketo

Der Datenstream &quot;Benutzerprüfung&quot;ist derzeit als Teil der Leistungspakete zusammen mit den anderen 3 Datenströmen verfügbar. Weitere Informationen zu den Paketen finden Sie auf der Seite [Produktbeschreibung](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) für Produktbeschränkungen und -funktionen.

### Einrichten von Adobe I/O

[Siehe Erste Schritte mit Adobe I/O-Ereignissen](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Grundlegende Anweisungen für diesen Anwendungsfall, beginnend mit [console.adobe.io](https://developer.adobe.com/console):

Wählen Sie bei Aufforderung entweder **[!UICONTROL Neues Projekt erstellen]** oder **[!UICONTROL Ereignis hinzufügen]** aus.

### Erste Schritte mit Ihrem neuen Projekt

Um mit der Verwendung von Adobe-Diensten zu beginnen, fügen Sie eine API, Ereignisse oder Laufzeit hinzu, sehen Sie sich unsere [Dokumentation](https://developer.adobe.com/runtime/docs/) an.

## Öffentliche Dokumentation

- [Marketo-Daten-Streams](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Einführung in Adobe IO-Ereignisse und Webhooks](https://developer.adobe.com/events/docs/guides/)
- [Daten-Stream-Blog](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
