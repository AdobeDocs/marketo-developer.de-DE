---
title: Standardfelder
feature: REST API, Field Management
description: Durchsuchen Sie die vollständige Liste der standardmäßigen Marketo-Lead-Felder mit REST-Namen, Kennzeichnungen und Beschreibungen sowie Anleitungen zum Abrufen über die API zum Beschreiben von Leads.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
TQID: https://experienceleague.adobe.com/vu2wGk36XJ243vwavhfLE7Vc9vMIJKGx6vmVqMRgEDA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 688
ht-degree: 19%

---

# Standardfelder

In der folgenden Tabelle sind die über die API verfügbaren Marketo-Standardfelder aufgeführt. Sie enthält den Namen, die Bezeichnung und die Beschreibung der REST-API jedes Felds.

Verwenden Sie den REST-Endpunkt [Lead beschreiben](https://developer.adobe.com/marketo-apis/api/mapi) um alle Feldnamen abzurufen, die von Ihren Lead-Datensätzen unterstützt werden.

| REST-API-Name | Angezeigte Bezeichnung | Beschreibung |
| --- | --- | --- |
| Adresse | Adresse | Lead-Adresse |
| Jahreseinnahmen | Jahresumsatz | Jahresumsatz des Lead-Unternehmens |
| anonyme IP | Anonyme IP | IP-Adresse des ersten aufgezeichneten Web-Besuchs des Leads |
| billingCity | Abrechnungsort | Ort, an dem die Rechnungsadresse des Leads liegt |
| Fakturierungsland | Abrechnungsland | Land der Rechnungsadresse des Leads |
| billingPostalCode | Postleitzahl für Abrechnung | Postleitzahl der Rechnungsadresse des Leads |
| BillingState | Bundesland für Abrechnung | Bundesland oder Provinz der Rechnungsadresse des Leads |
| BillingStreet | Rechnungsadresse | Straße der Rechnungsadresse des Unternehmens des Leads |
| city | Stadt | Lead&#39;s Stadt |
| Unternehmen | Firmenname | Firmenname des Leads |
| country | Land | Land des Leads |
| Geburtsdatum | Geburtsdatum | Geburtsdatum des Leads |
| Abteilung | Abteilung | Leads Abteilung in ihrem Unternehmen |
| doNotCall | Nicht aufrufen | Voreinstellung für Nicht-Anrufe des Leads |
| doNotCallReason | Nicht aufrufen – Ursache | Erklärung für die „Do-Not-Call“-Präferenz des Leads |
| E-Mail | E-Mail-Adresse | E-Mail-Adresse des Leads. Marketo-Standardschlüsselfeld für Lead-Datensätze |
| Fax | Faxnummer | Faxnummer des Leads |
| firstName | Vorname | Vorname des Leads |
| Wirtschaftszweig | Branche | Lead-Branche |
| Abgeleitetes Unternehmen | Abgeleitetes Unternehmen | Firmenname, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads |
| abgeleitetes Land | Abgeleitetes Land | Land, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads |
| lastName | Nachname | Nachname des Leads |
| leadRole | Rolle | Die Rolle des Leads in seinem Unternehmen |
| LeadScore | Lead-Bewertung | Ganze Zahl, die dem Lead durch Bewertung von Kampagnen und Programmen zugewiesen wird |
| leadSource | Lead-Quelle | Feldaufzeichnung, aus welcher Quelle der Lead stammt |
| leadStatus | Lead-Status | Feld, das den aktuellen Marketing-/Verkaufsstatus des Leads aufzeichnet |
| mainPhone | Haupttelefonnummer | Primäre Telefonnummer des Unternehmens des Leads |
| jigsawContactId | Marketo Data.com – ID | Data.com ID des Leads, falls verfügbar |
| PuzzleContactStatus | Marketo Data.com – Status | Status Data.com des Leads, falls verfügbar |
| MiddleName | Zweiter Vorname | Zweiter Vorname des Leads |
| Mobiltelefon | Mobiltelefonnummer | Mobiltelefonnummer des Leads |
| numberOfEmployees | Anzahl Mitarbeiter | Anzahl der Mitarbeiter des Lead-Unternehmens |
| Telefon | Telefonnummer | Telefonnummer des Leads |
| Postleitzahl | Postleitzahl | Postleitzahl des Leads |
| Bewertung | Lead-Bewertung | Marketing-/Verkaufsbewertung des Leads |
| Begrüßung | Anrede | Leads bevorzugte Begrüßung, das heißt Mister, Miss…und so weiter |
| sicCode | SIC-Code | Standard-Industrieklassifizierungscode des Unternehmens des Leads |
| Site | Seite |  |
| state | Land | Lead-Status |
| Titel | Jobtitel | Stellenbezeichnung des Leads |
| Abgemeldet | Abbestellt | Der E-Mail-Abmeldestatus des Leads. Teilweise vom System verwaltet. Verhindert den Empfang nicht funktionierender E-Mails, wenn er auf „true“ gesetzt ist. |
| unsubscribedReason | Ursache für Abbestellung | Grund für die Abmeldung des Leads. Teilweise vom System verwaltet. Mit E-Mail-Informationen gefüllt, wenn sich der Lead direkt von einer Marketo-E-Mail abgemeldet hat. |
| Website | Website | URL der Website des Unternehmens des Leads |
| createdAt | Erstellt um | Der Zeitpunkt, zu dem der Lead-Datensatz anfänglich erstellt wurde. Vom System verwaltet |
| updatedAt | Aktualisiert um | Letztes Mal, als der Lead-Datensatz aktualisiert wurde. Vom System verwaltet |
| emailInvalid | E-Mail-Adresse ungültig | Ungültiger E-Mail-Status. Alle E-Mails an diese Adresse werden blockiert, wenn sie auf „true“ gesetzt sind. Durch Bounces, die darauf hinweisen, dass die E-Mail ungültig ist, wird dieses Feld automatisch auf „true“ gesetzt. |
| emailInvalidCause | Grund für ungültige E-Mail | Ursache des ungültigen E-Mail-Status. Die Bounce-Nachricht wird in diesem Feld aufgezeichnet, wenn die Einstellung Ungültige E-Mail auf „true“ gesetzt ist. |
| abgeleitete Stadt | Abgeleiteter Ort | Stadt des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredMetropolitanArea | Abgeleiteter Stadtbereich | Der Großraum Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredPhoneAreaCode | Abgleitete Vorwahl | Telefonvorwahl des Leads, abgeleitet durch Reverse-IP-Suche des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredPostalCode | Abgeleitete Postleitzahl | Postleitzahl des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| inferredStateRegion | Abgeleitetes Bundesland/abgeleitete Region | Region des Leads, abgeleitet durch Reverse-IP-Lookup des ersten aufgezeichneten Web-Besuchs des Leads. |
| isAnonymous | Ist anonym | Anonymer Status des Lead-Eintrags. Vom System verwaltet. |
| Priorität | Priorität | Vertriebs-Insight-Priorität des Leads. Vom System verwaltet. |
| relativer Wert | Relative Bewertung | Relative Bewertung des Leads zur Vertriebs-Insight. Vom System verwaltet. |
| Dringlichkeit | Dringlichkeit | Insight-Dringlichkeit des Leads. Vom System verwaltet. |
