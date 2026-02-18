---
title: Standardfelder
feature: REST API, Field Management
description: Durchsuchen Sie die vollständige Liste der standardmäßigen Marketo-Lead-Felder mit REST- und SOAP-Namen, -Kennzeichnungen und -Beschreibungen sowie Anleitungen zum Abrufen über die API zum Beschreiben von Leads.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: d674384b3ab979df2322ece3f02155259d05431a
workflow-type: tm+mt
source-wordcount: '727'
ht-degree: 25%

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
