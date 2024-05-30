---
title: "Standardfelder"
feature: REST API, Field Management
description: "Eine Tabelle mit standardmäßigen Marketo-Feldern."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 28%

---


# Standardfelder

Im Folgenden finden Sie eine Liste der in Marketo verfügbaren Standardfelder, auf die über die API zugegriffen werden kann.

Sie können die Liste aller unterstützten Feldnamen abrufen, die in Ihren Lead-Datensätzen verfügbar sind, indem Sie REST verwenden [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/) -Endpunkt.

| REST-API-Name | SOAP-API-Name | Angezeigte Bezeichnung | Beschreibung |
| --- | --- | --- | --- |
| Adresse | Adresse | Adresse | Adresse des federführenden Unternehmens |
| annualRevenue | AnnualRevenue | Jahresumsatz | Jahresumsatz des Lead-Unternehmens |
| anonymousIP | AnonymousIP | Anonyme IP | IP-Adresse des ersten aufgezeichneten Webbesuchs des Leads |
| billingCity | BillingCity | Abrechnungsort | Rechnungsadresse des Leads |
| billingCountry | BillingCountry | Abrechnungsland | Rechnungsadresse des Lead |
| billingPostalCode | BillingPostalCode | Postleitzahl für Abrechnung | Postleitzahl der Abrechnungsadresse des Leads |
| billingState | BillingState | Bundesland für Abrechnung | Rechnungsadresse des Lead-Landes oder -Provinz |
| billingStreet | BillingStreet | Rechnungsadresse | Abrechnung der Straßenadresse des Lead-Unternehmens |
| city | Ort | Ort | Lead-Stadt |
| Unternehmen | Unternehmen | Unternehmensname | Firmenname des Leads |
| country | Land | Land | Land des federführenden Unternehmens |
| dateOfBirth | DateofBirth | Geburtsdatum | Geburtsdatum des Leiters |
| Abteilung | Abteilung | Abteilung | Lead-Abteilung in ihrem Unternehmen |
| doNotCall | DoNotCall | Nicht aufrufen | Nicht-Call-Präferenz des Leads |
| doNotCallReason | DoNotCallReason | Nicht aufrufen – Ursache | Erläuterung der nicht aufrufenden Voreinstellung des Leads |
| E-Mail | E-Mail | E-Mail-Adresse | Die E-Mail-Adresse des Leads. Standard-Marketo-Schlüsselfeld für Lead-Datensätze |
| fax | Fax | Faxnummer | Faxnummer des Leiters |
| firstName | FirstName | Vorname | Vorname des Leads |
| Wirtschaftszweig | Branche | Branche | Lead-Branche |
| inferredCompany | InferredCompany | Abgeleitetes Unternehmen | Unternehmensname, abgeleitet durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads |
| inferredCountry | InferredCountry | Abgeleitetes Land | Land, das durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads abgeleitet wird |
| lastName | LastName | Nachname | Nachname des Leads |
| leadRole | LeadRole | Role | Rolle des Leads in seinem Unternehmen |
| leadScore | LeadScore | Lead-Bewertung | Ganzzahlwert, der dem Lead durch Scoring-Kampagnen und -Programme zugewiesen wird |
| leadSource | LeadSource | Lead-Quelle | Feld zur Angabe der Quelle, aus der der Lead stammt |
| leadStatus | LeadStatus | Lead-Status | Feld zur Aufzeichnung des aktuellen Marketing-/Verkaufsstatus des Leads |
| mainPhone | MainPhone | Haupttelefonnummer | Primäre Telefonnummer der Lead-Firma |
| jigsawContactId | Marketo Jigsaw – Kontakt-ID | Marketo Data.com – ID | Lead-ID Data.com , falls verfügbar |
| jigsawContactStatus | Marketo Jigsaw – Kontaktstatus | Marketo Data.com – Status | Status Data.com des Leads, falls verfügbar |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Marketo Social – Facebook-Anzeigename | Facebook-Anzeigename des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| facebookId | MarketoSocialFacebookId | Marketo Social Facebook-ID | Facebook ID des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| facebookFotoURL | MarketoSocialFacebookPhotoURL | Marketo Social – Facebook-Foto-URL | URL des Facebook-Profilfotos des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| facebookProfileURL | MarketoSocialFacebookProfileURL | Marketo Social – Facebook-Profil-URL | URL des Facebook-Profils des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| facebookReach | MarketoSocialFacebookReach | Marketo Social – Facebook-Reichweite | Die Facebook-Reichweite des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei Facebook | Anzahl der weitergeleiteten Registrierungen, die dem Lead über Facebook zugeordnet wurden. Systemverwaltetes System |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Marketo Social – Bezeichnete Besuche bei Facebook | Anzahl der Besuche, die dem Lead über Facebook zugeordnet wurden. Systemverwaltetes System |
| gender | MarketoSocialGender | Marketo Social – Geschlecht | Geschlecht des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Marketo Social – Letzte bezeichnete Registrierung | Datum der letzten abgeschlossenen Befassung. Systemverwaltetes System |
| lastReferredVisit | MarketoSocialLastReferredVisit | Marketo Social – Letzter bezeichneter Besuch | Datum des letzten Besuchs. Systemverwaltetes System |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Marketo Social – LinkedIn-Anzeigename | LinkedIn-Anzeigename des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| linkedInId | MarketoSocialLinkedInId | Marketo Social LinkedIn-ID | LinkedIn ID des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| linkedInFotoURL | MarketoSocialLinkedInPhotoURL | Marketo Social – LinkedIn-Foto-URL | LinkedIn-Foto-URL des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | Marketo Social – LinkedIn-Profil-URL | LinkedIn-Profil des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| linkedInReach | MarketoSocialLinkedInReach | Marketo Social – LinkedIn-Reichweite | LinkedIn-Reichweite des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei LinkedIn | Anzahl der weitergeleiteten Registrierungen, die dem Lead über LinkedIn zugeordnet wurden. Systemverwaltetes System |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Marketo Social – Bezeichnete Besuche bei LinkedIn | Anzahl der Besuche, die dem Lead über LinkedIn zugeordnet wurden. Systemverwaltetes System |
| syndicationId |  - | Marketo Social Syndication-ID | Interne Marketo Social-ID des Leads. Systemverwaltetes System |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Marketo Social – Bezeichnete Registrierungen insgesamt | Gesamtzahl der abgeschlossenen Bewerbungsanmeldungen, die dem Lead zugeordnet wurden |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Marketo Social – Bezeichnete Besuche insgesamt | Gesamtzahl der dem Lead zugeordneten Besuche |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Marketo Social – Twitter-Anzeigename | Twitter-Anzeigename des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| twitterId | MarketoSocialTwitterId | Marketo Social Twitter-ID | Twitter-ID des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| twitterFotoURL | MarketoSocialTwitterPhotoURL | Marketo Social – Twitter-Foto-URL | Twitter Foto-URL des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| twitterProfileURL | MarketoSocialTwitterProfileURL | Marketo Social – Twitter-Profil-URL | Twitter-Profil-URL des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| twitterReach | MarketoSocialTwitterReach | Marketo Social – Twitter-Reichweite | Die Twitter-Reichweite des Leads. System wird während der Anmeldung in sozialen Netzwerken ausgefüllt |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei Twitter | Anzahl der weitergeleiteten Registrierungen, die dem Lead über Twitter zugeordnet wurden. Systemverwaltetes System |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Marketo Social – Bezeichnete Besuche bei Twitter | Anzahl der Besuche, die dem Lead über Twitter zugeordnet wurden. Systemverwaltetes System |
| middleName | MiddleName | Zweiter Vorname | Mittlerer Name des Führers |
| mobilePhone | MobilePhone | Mobiltelefonnummer | Mobiltelefonnummer des Leiters |
| numberOfEmployees | NumberOfEmployees | Anzahl Mitarbeiter | Anzahl der Beschäftigten des Lead-Unternehmens |
| Telefon | Telefon | Telefonnummer | Telefonnummer des Interessenten |
| postalCode | PostalCode | Postleitzahl | Postleitzahl des Leiters |
| Bewertung | Bewertung | Lead-Bewertung | Marketing-/Verkaufsbewertung des Leads |
| salutation | Anrede | Anrede | Beliebte Anrede des Führers, d.h. Herr, Misses.. usw. |
| sicCode | SICCode | SIC-Code | Standard-Klassifizierungscode des Lead-Unternehmens |
| site | Seite | Seite |  |
| state | Bundesland | Bundesland | Bundesstaat des federführenden Unternehmens |
| Titel | Titel | Jobtitel | Berufsbezeichnung des Leiters |
| unsubscribed | Abbestellt | Abbestellt | Status der Abmeldung per E-Mail des Leads. Teilweise vom System verwaltet. Verhindert den Empfang nicht operativer E-Mails, wenn auf &quot;true&quot;gesetzt. |
| unsubscribedReason | UnsubscribedReason | Ursache für Abbestellung | Grund für den Abmeldestatus des Leads. Teilweise vom System verwaltet. Werden mit E-Mail-Informationen ausgefüllt, wenn ein Lead sich direkt von einer Marketo-E-Mail abgemeldet hat. |
| website | Website | Website | URL der Website des Lead-Unternehmens |
| createdAt |  - | Erstellt um | Der Zeitpunkt, zu dem der Lead-Datensatz ursprünglich erstellt wurde. Systemverwaltetes System |
| updatedAt |  - | Aktualisiert um | Das letzte Mal, dass der Lead-Datensatz aktualisiert wurde. Systemverwaltetes System |
| emailInvalid |  - | E-Mail-Adresse ungültig | Status &quot;E-Mail ungültig&quot;. Alle E-Mails an die Adresse werden blockiert, wenn auf &quot;true&quot;gesetzt. Bounces, die angeben, dass die E-Mail ungültig ist, setzen dieses Feld automatisch auf &quot;true&quot;. |
| emailInvalidUrsache |  - | Grund für ungültige E-Mail | Grund für den ungültigen E-Mail-Status. Die auslösende Bounce Message wird in diesem Feld aufgezeichnet, wenn &quot;email invalid&quot;auf &quot;true&quot;gesetzt ist. |
| inferredCity |  - | Abgeleiteter Ort | Die Stadt des Leads wird durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads abgeleitet. |
| inferredMetropolitanArea |  - | Abgeleiteter Stadtbereich | Die Metropolregion des Leads wird durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads abgeleitet. |
| inferredPhoneAreaCode |  - | Abgleitete Vorwahl | Der Telefongebietscode des Leads wird durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads abgeleitet. |
| inferredPostalCode |  - | Abgeleitete Postleitzahl | Postleitzahl des Leads, abgeleitet durch die umgekehrte IP-Suche des ersten aufgezeichneten Webbesuchs des Leads. |
| inferredStateRegion |  - | Abgeleitetes Bundesland/abgeleitete Region | Die Statusregion des Leads wird von der umgekehrten IP-Suche des ersten aufgezeichneten Webbesuchs des Leads abgeleitet. |
| isAnonymous |  - | Ist anonym | Anonymer Status des Lead-Datensatzes. System verwaltet. |
| Priorität |  - | Priorität | Die Priorität von Sales Insight von Lead. System verwaltet. |
| relativeScore |  - | Relative Bewertung | relatives Ergebnis von Lead Sales Insight. System verwaltet. |
| Dringlichkeit |  - | Dringlichkeit | Die Dringlichkeit von Sales Insight von Lead. System verwaltet. |
