---
title: Standardfelder
feature: REST API, Field Management
description: Durchsuchen Sie die vollständige Liste der standardmäßigen Marketo-Lead-Felder mit REST- und SOAP-Namen, -Kennzeichnungen und -Beschreibungen sowie Anleitungen zum Abrufen über die API zum Beschreiben von Leads.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 28%

---

# Standardfelder

Im Folgenden finden Sie eine Liste der in Marketo verfügbaren Standardfelder, auf die über die API zugegriffen werden kann.

Sie können die Liste aller unterstützten Feldnamen in Ihren Lead-Datensätzen abrufen, indem Sie den REST-Endpunkt [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/) verwenden.

| REST-API-Name | SOAP-API-Name | Angezeigte Bezeichnung | Beschreibung |
| --- | --- | --- | --- |
| Adresse | Adresse | Adresse | Lead-Adresse |
| Jahreseinnahmen | AnnualRevenue | Jahresumsatz | Jahresumsatz des Lead-Unternehmens |
| anonyme IP | AnonymousIP | Anonyme IP | IP-Adresse des ersten aufgezeichneten Web-Besuchs des Leads |
| billingCity | BillingCity | Abrechnungsort | Ort, an dem die Rechnungsadresse des Leads liegt |
| Fakturierungsland | BillingCountry | Abrechnungsland | Land der Rechnungsadresse des Leads |
| billingPostalCode | BillingPostalCode | Postleitzahl für Abrechnung | Postleitzahl der Rechnungsadresse des Leads |
| BillingState | BillingState | Bundesland für Abrechnung | Bundesland oder Provinz der Rechnungsadresse des Leads |
| BillingStreet | BillingStreet | Rechnungsadresse | Straße der Rechnungsadresse des Unternehmens des Leads |
| city | Stadt | Stadt | Lead&#39;s Stadt |
| Unternehmen | Unternehmen | Firmenname | Firmenname des Leads |
| country | Land | Land | Land des Leads |
| Geburtsdatum | DateofBirth | Geburtsdatum | Geburtsdatum des Leads |
| Abteilung | Abteilung | Abteilung | Leads Abteilung in ihrem Unternehmen |
| doNotCall | DoNotCall | Nicht aufrufen | Voreinstellung für Nicht-Anrufe des Leads |
| doNotCallReason | DoNotCallReason | Nicht aufrufen – Ursache | Erklärung für die „Do-Not-Call“-Präferenz des Leads |
| E-Mail | E-Mail | E-Mail-Adresse | E-Mail-Adresse des Leads. Marketo-Standardschlüsselfeld für Lead-Datensätze |
| Fax | Fax | Faxnummer | Faxnummer des Leads |
| firstName | FirstName | Vorname | Vorname des Leads |
| Wirtschaftszweig | Branche | Branche | Lead-Branche |
| Abgeleitetes Unternehmen | InferredCompany | Abgeleitetes Unternehmen | Firmenname, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads |
| abgeleitetes Land | InferredCountry | Abgeleitetes Land | Land, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads |
| lastName | LastName | Nachname | Nachname des Leads |
| leadRole | LeadRole | Rolle | Die Rolle des Leads in seinem Unternehmen |
| LeadScore | LeadScore | Lead-Bewertung | Ganze Zahl, die dem Lead durch Bewertung von Kampagnen und Programmen zugewiesen wird |
| leadSource | LeadSource | Lead-Quelle | Feldaufzeichnung, aus welcher Quelle der Lead stammt |
| leadStatus | LeadStatus | Lead-Status | Feld, das den aktuellen Marketing-/Verkaufsstatus des Leads aufzeichnet |
| mainPhone | MainPhone | Haupttelefonnummer | Primäre Telefonnummer des Unternehmens des Leads |
| jigsawContactId | Marketo Jigsaw – Kontakt-ID | Marketo Data.com – ID | Data.com ID des Leads, falls verfügbar |
| PuzzleContactStatus | Marketo Jigsaw – Kontaktstatus | Marketo Data.com – Status | Status Data.com des Leads, falls verfügbar |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Marketo Social – Facebook-Anzeigename | Facebook-Anzeigename des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| facebookId | MarketoSocialFacebookId | Marketo Social Facebook-ID | Facebook-ID des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| facebookFotoURL | MarketoSocialFacebookPhotoURL | Marketo Social – Facebook-Foto-URL | URL des Facebook-Profilbilds des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| facebookProfileURL | MarketoSocialFacebookProfileURL | Marketo Social – Facebook-Profil-URL | URL des Facebook-Profils des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| facebookReach | MarketoSocialFacebookReach | Marketo Social – Facebook-Reichweite | Die Facebook-Reichweite des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei Facebook | Anzahl der weitergeleiteten Registrierungen, die dem Lead über Facebook zugeordnet wurden. Vom System verwaltet |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Marketo Social – Bezeichnete Besuche bei Facebook | Anzahl der verwiesenen Besuche, die dem Lead über Facebook zugeordnet wurden. Vom System verwaltet |
| Geschlecht | MarketoSocialGender | Marketo Social – Geschlecht | Geschlecht des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Marketo Social – Letzte bezeichnete Registrierung | Datum der letzten abgeschlossenen Empfehlung. Vom System verwaltet |
| lastReferredVisit | MarketoSocialLastReferredVisit | Marketo Social – Letzter bezeichneter Besuch | Datum des letzten Besuchs. Vom System verwaltet |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Marketo Social – LinkedIn-Anzeigename | LinkedIn-Anzeigename des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| linkinId | MarketoSocialLinkedInId | Marketo Social LinkedIn-ID | LinkedIn-ID des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | Marketo Social – LinkedIn-Foto-URL | LinkedIn-Foto-URL des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | Marketo Social – LinkedIn-Profil-URL | LinkedIn-Profil des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| linkedInReach | MarketoSocialLinkedInReach | Marketo Social – LinkedIn-Reichweite | LinkedIn-Reichweite des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei LinkedIn | Anzahl der verwiesenen Registrierungen, die dem Lead über LinkedIn zugewiesen wurden. Vom System verwaltet |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Marketo Social – Bezeichnete Besuche bei LinkedIn | Anzahl der dem Lead über LinkedIn zugewiesenen verwiesenen Besuche. Vom System verwaltet |
| syndicationId |  - | Marketo Social Syndication-ID | Interne Marketo Social-ID des Leads. Vom System verwaltet |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Marketo Social – Bezeichnete Registrierungen insgesamt | Gesamtzahl der abgeschlossenen Empfehlungsregistrierungen, die dem Lead zugeordnet wurden |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Marketo Social – Bezeichnete Besuche insgesamt | Gesamtzahl der dem Lead zugewiesenen verwiesenen Besuche |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Marketo Social – Twitter-Anzeigename | Twitter-Anzeigename des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| twitterId | MarketoSocialTwitterId | Marketo Social Twitter-ID | Twitter-ID des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| twitterFotoURL | MarketoSocialTwitterPhotoURL | Marketo Social – Twitter-Foto-URL | Twitter-Foto-URL des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| twitterProfileURL | MarketoSocialTwitterProfileURL | Marketo Social – Twitter-Profil-URL | Twitter-Profil-URL des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| twitterReach | MarketoSocialTwitterReach | Marketo Social – Twitter-Reichweite | Twitter-Reichweite des Leads. System wird während der Anmeldung bei Social Media ausgefüllt |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Marketo Social – Bezeichnete Registrierungen bei Twitter | Anzahl der weitergeleiteten Registrierungen, die dem Lead über Twitter zugeordnet wurden. Vom System verwaltet |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Marketo Social – Bezeichnete Besuche bei Twitter | Anzahl der verwiesenen Besuche, die dem Lead über Twitter zugeordnet wurden. Vom System verwaltet |
| MiddleName | MiddleName | Zweiter Vorname | Zweiter Vorname des Leads |
| Mobiltelefon | MobilePhone | Mobiltelefonnummer | Mobiltelefonnummer des Leads |
| numberOfEmployees | NumberOfEmployees | Anzahl Mitarbeiter | Anzahl der Mitarbeiter des Lead-Unternehmens |
| Telefon | Telefon | Telefonnummer | Telefonnummer des Leads |
| Postleitzahl | PostalCode | Postleitzahl | Postleitzahl des Leads |
| Bewertung | Rating | Lead-Bewertung | Marketing-/Verkaufsbewertung des Leads |
| Begrüßung | Anrede | Anrede | Die bevorzugte Begrüßung des Leads, d.h. Mister, Misses…etc. |
| sicCode | SICCode | SIC-Code | Standard-Industrieklassifizierungscode des Unternehmens des Leads |
| Baustelle | Seite | Seite |  |
| state | Land | Land | Lead-Status |
| Titel | Titel | Jobtitel | Stellenbezeichnung des Leads |
| Abgemeldet | Abbestellt | Abbestellt | Der E-Mail-Abmeldestatus des Leads. Teilweise vom System verwaltet. Verhindert den Empfang nicht funktionierender E-Mails, wenn er auf „true“ gesetzt ist. |
| unsubscribedReason | UnsubscribedReason | Ursache für Abbestellung | Grund für die Abmeldung des Leads. Teilweise vom System verwaltet. Mit E-Mail-Informationen gefüllt, wenn sich der Lead direkt von einer Marketo-E-Mail abgemeldet hat. |
| Website | Website | Website | URL der Website des Unternehmens des Leads |
| createdAt |  - | Erstellt um | Der Zeitpunkt, zu dem der Lead-Datensatz anfänglich erstellt wurde. Vom System verwaltet |
| updatedAt |  - | Aktualisiert um | Letztes Mal, als der Lead-Datensatz aktualisiert wurde. Vom System verwaltet |
| emailInvalid |  - | E-Mail-Adresse ungültig | Ungültiger E-Mail-Status. Alle E-Mails an diese Adresse werden blockiert, wenn sie auf „true“ gesetzt sind. Durch Bounces, die darauf hinweisen, dass die E-Mail ungültig ist, wird dieses Feld automatisch auf „true“ gesetzt. |
| emailInvalidCause |  - | Grund für ungültige E-Mail | Ursache des ungültigen E-Mail-Status. Die Bounce-Nachricht wird in diesem Feld aufgezeichnet, wenn die Einstellung Ungültige E-Mail auf „true“ gesetzt ist. |
| abgeleitete Stadt |  - | Abgeleiteter Ort | Stadt des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredMetropolitanArea |  - | Abgeleiteter Stadtbereich | Der Großraum Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredPhoneAreaCode |  - | Abgleitete Vorwahl | Telefonvorwahl des Leads, abgeleitet durch Reverse-IP-Suche des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredPostalCode |  - | Abgeleitete Postleitzahl | Postleitzahl des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredStateRegion |  - | Abgeleitetes Bundesland/abgeleitete Region | Region des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| isAnonymous |  - | Ist anonym | Anonymer Status des Lead-Eintrags. Vom System verwaltet. |
| Priorität |  - | Priorität | Vertriebs-Insight-Priorität des Leads. Vom System verwaltet. |
| relativer Wert |  - | Relative Bewertung | Relative Bewertung des Leads zur Vertriebs-Insight. Vom System verwaltet. |
| Dringlichkeit |  - | Dringlichkeit | Insight-Dringlichkeit des Leads. Vom System verwaltet. |
