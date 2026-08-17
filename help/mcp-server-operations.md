---
title: Marketo Engage MCP-Vorgänge
description: Erfahren Sie, welche Marketo Engage-MCP-Vorgänge für die Verwendung mit KI-Assistenten verfügbar sind.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: dca84292-69e9-4116-a575-667d31fa060did: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: 066dff918cae70ccf4284b626ccb44d47a31c386
workflow-type: tm+mt
source-wordcount: 1249
ht-degree: 49%

---


# [!DNL Marketo Engage] MCP-Vorgänge

Die folgenden Vorgänge sind über den [!DNL Marketo Engage] MCP-Server verfügbar. Der -Server stellt schreibgeschützte oder zerstörungsfreie Endpunkte bereit. Das KI-System kann keine `Delete` oder andere zerstörerische Vorgänge verwenden.

>[!NOTE]
>
>Das MCP Server-Team arbeitet daran, die Smart List- und Smart Campaign Asset-APIs für die Zusammenarbeit mit dem MCP Server zu aktivieren. Diese Arbeiten, einschließlich der Zulassungsauflistung von Elementen, werden voraussichtlich im 3. Quartal 2026 abgeschlossen sein.

Informationen zum Umgang mit Daten mit Marketo AI und dem Marketo Engage MCP-Server finden Sie auf der Seite [Dateninformationen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

## Massenexport

[API-Referenz für Massenexport](https://developer.adobe.com/marketo-apis/api/mapi){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Kanäle und Tags

[Channels API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels){target="_blank"} | [Tags-API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## E-Mails

[E-Mail-API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Ordner

[API-Referenz für Ordner](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Formulare

[Forms-API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms){target="_blank"}

- `add_field_set`
- `add_field_to_form`
- `add_field_visibility_rule`
- `add_rich_text_field`
- `approve_form`
- `browse_forms`
- `clone_form`
- `create_form`
- `delete_field_from_fieldset`
- `delete_form`
- `delete_form_field`
- `discard_form_draft`
- `get_form_by_id`
- `get_form_by_name`
- `get_form_field_metadata`
- `get_form_fields`
- `get_forms_used_by`
- `get_program_member_fields`
- `get_thank_you_page`
- `set_field_autofill`
- `update_field_positions`
- `update_form`
- `update_form_field`

## Leads

[Leads-API-Referenz](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programme

[API-Referenz für Programme](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs){target="_blank"}

- `approve_program`
- `browse_email_batch_programs`
- `browse_nurture_programs`
- `browse_program_details`
- `browse_program_events`
- `browse_programs`
- `browse_scheduled_programs`
- `clone_program`
- `create_program`
- `delete_program_tag`
- `get_program_by_id`
- `get_program_by_name`
- `get_program_creation_options`
- `get_program_smart_list`
- `get_programs_by_tag`
- `unapprove_program`
- `update_program`
- `update_program_tag`

## Intelligente Kampagnen

[API-Referenz für Smart-Kampagnen](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns){target="_blank"}

- `activate_smart_campaign`
- `add_flow_step`
- `browse_smart_campaigns`
- `create_smart_campaign`
- `facet_smart_campaigns`
- `get_smart_campaign_auto_suggest`
- `get_smart_campaign_by_id`
- `get_smart_campaign_by_name`
- `get_smart_campaign_flow_step_by_name`
- `get_smart_campaign_flow_step_type_by_name`
- `get_smart_campaign_flow_step_types`
- `get_smart_campaign_flow_steps`
- `get_smart_campaign_rule_by_name`
- `get_smart_campaign_rules`
- `get_smart_campaign_scheduled_runs`
- `get_smart_campaign_used_by`
- `get_smart_list_by_campaign_id`
- `schedule_campaign`
- `trigger_campaign`
- `update_flow_step_choice`
- `update_smart_campaign`

## Intelligente Listen

[API-Referenz für Smart Lists](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists){target="_blank"}

- `add_smart_list_rule`
- `browse_smart_lists`
- `clone_smart_list`
- `create_smart_list`
- `delete_all_smart_list_rules`
- `get_smart_list_auto_suggest`
- `get_smart_list_by_id`
- `get_smart_list_by_name`
- `get_smart_list_rule_by_name`
- `get_smart_list_rules`
- `get_smart_list_used_by`
- `remove_smart_list_rule_constraint`
- `reorder_smart_list_rules`
- `update_smart_list_filter_logic`
- `update_smart_list_rule`

## Ausschnitte

[Snippets-API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets){target="_blank"}

- `approve_snippet`
- `browse_snippets`
- `clone_snippet`
- `create_snippet`
- `delete_snippet`
- `discard_snippet_draft`
- `facet_snippets`
- `get_snippet_by_id`
- `get_snippet_content`
- `get_snippet_dynamic_content`
- `unapprove_snippet`
- `update_snippet`
- `update_snippet_content`
- `update_snippet_dynamic_content`

## Statische Listen

[API-Referenz für statische Listen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Token

[Token-API-Referenz](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`

## MCP Flow Steps-Tools aktiviert

| Flussschritte | Trigger | Filter (Aktivität) | Filter (Attribut) |
| --- | --- | --- | --- |
| <ul><li>Zu Feldsatz hinzufügen</li><li>Hinzufügen zur Liste</li><li>Zu Microsoft-Kampagne hinzufügen</li><li>Zu Nurture hinzufügen</li><li>Hinzufügen zu SFDC-Kampagne</li><li>Webhook aufrufen</li><li>Datenwert ändern</li><li>Lead-Partition ändern</li><li>Nurturing-Kadenz ändern</li><li>Nurturing-Verfolgung ändern</li><li>Ändern von Eigentümerin bzw. Eigentümer</li><li>Ändern von Inhabenden in Microsoft</li><li>Programmdaten ändern</li><li>Ändern von Mitgliederdaten in Programmen</li><li>Umsatzstadium ändern</li><li>Ändern von Bewertung</li><li>Segment ändern</li><li>Ändern des Status im Verlauf</li><li>Ändern des Status in SFDC-Kampagne</li><li>Lead konvertieren</li><li>Erstellen von Aufgaben</li><li>Erstellen einer Aufgabe in Microsoft</li><li>Lead löschen</li><li>Lead aus Microsoft löschen</li><li>Lead aus SFDC löschen</li><li>Ausführen von Kampagne</li><li>Interessanter Moment</li><li>Aus Feldsatz entfernen</li><li>Entfernen aus Fluss</li><li>Entfernen aus Liste</li><li>Aus Microsoft-Kampagne entfernen</li><li>Entfernen aus SFDC-Kampagne</li><li>Anfordern von Kampagne</li><li>Senden von Alarm</li><li>Senden von E-Mail</li><li>Lead mit Microsoft synchronisieren</li><li>Lead in SFDC synchronisieren</li><li>Warten</li></ul> | <ul><li>Aktivität wird protokolliert</li><li>Aktivität wird aktualisiert</li><li>Der Liste hinzugefügt</li><li>Zu Microsoft Campaign hinzugefügt</li><li>Zu „Nurture“ hinzugefügt</li><li>Zu Opportunity hinzugefügt</li><li>Zu Opportunity (Konto) hinzugefügt</li><li>Zu Opportunity (Kontakt) hinzugefügt</li><li>Zu SFDC-Kampagne hinzugefügt</li><li>Stellt Fragen während des Ereignisses</li><li>Teilnimmt an Veranstaltung teil</li><li>Kampagne wird angefordert</li><li>Klickt auf Link</li><li>Klickt auf Link in E-Mail&#x200B;</li><li>Klickt auf Link in Verkaufs-E-Mail</li><li>Klicks auf Link in SMS-Nachricht</li><li>Klicks auf einen Link</li><li>Datenwertänderungen</li><li>Lädt ein Asset herunter</li><li>E-Mail nicht zugestellt (Bounce)</li><li>E-Mail nicht zugestellt (Soft Bounce)</li><li>E-Mail wird übermittelt</li><li>Interagiert mit einem Gesprächsfluss</li><li>Interagiert mit einem Dialogfeld</li><li>Interagiert mit einem Agenten im Gesprächsfluss</li><li>Interagiert mit einem Agenten im Dialogfeld</li><li>Füllt Formular aus</li><li>Hat interessanten Moment</li><li>Interagiert mit Dokumenten im Konversationsfluss</li><li>Interagiert mit einem Dokument im Dialogfeld</li><li>Erhält Vertriebs-E-Mail</li><li>Lead wird konvertiert</li><li>Lead wird erstellt</li><li>Lead wird aus Microsoft gelöscht</li><li>Lead wird aus SFDC gelöscht</li><li>Lead wird an Marketo gesendet</li><li>Lead wird mit Microsoft synchronisiert</li><li>Lead wird mit SFDC synchronisiert</li><li>Änderungen der Lead-Partition</li><li>Manuelle Stadiumsänderung</li><li>Kadenzänderungen pflegen</li><li>Änderungen pflegen</li><li>Öffnet E-Mail</li><li>Öffnet Verkaufs-E-Mail</li><li>Opportunity (Konto) wird aktualisiert</li><li>Opportunity (Kontakt) wird aktualisiert</li><li>Opportunity wird aktualisiert</li><li>Eigentümer wird geändert</li><li>Änderungen an Inhabern in Microsoft</li><li>Daten der Programmteilnehmer werden geändert</li><li>Der Fortschrittsstatus wurde geändert</li><li>Erreicht das Dialogziel</li><li>Erreicht Ziel im Gesprächsfluss</li><li>Hat E-Mail „An einen Freund weiterleiten“ erhalten</li><li>Aus Liste entfernt</li><li>Aus Microsoft-Kampagne entfernt</li><li>Aus Opportunity entfernt</li><li>Aus Opportunity entfernt (Konto)</li><li>Aus Opportunity entfernt (Kontakt)</li><li>Aus SFDC-Kampagne entfernt</li><li>Antworten auf Verkaufs-E-Mail</li><li>Antwortet auf eine Abfrage</li><li>Antwortet auf eine Umfrage</li><li>Umsatzstadium wird geändert</li><li>Verkaufs-E-Mail wird aufgrund eines Bounce-Ereignisses nicht zugestellt</li><li>Verkaufs-E-Mail wird empfangen</li><li>Termine für Besprechungen im Gesprächsfluss</li><li>Besprechungszeitpläne im Dialogfeld</li><li>Bewertung wird geändert</li><li>Segment wird geändert</li><li>Alarm senden</li><li>Hat E-Mail „An einen Freund weiterleiten“ gesendet</li><li>Bounces bei SMS-Nachrichten</li><li>SMS-Nachricht wird zugestellt</li><li>Status wird in SFDC-Kampagne geändert</li><li>Bestellt E-Mail ab</li><li>Besucht Web-Seite</li><li>Webhook wird aufgerufen</li></ul> | <ul><li>Aktivität wurde protokolliert</li><li>Aktivität wurde aktualisiert</li><li>Alarm wurde gesendet</li><li>Kampagne wurde ausgeführt</li><li>Kampagne wurde angefordert</li><li>Auf Link klicken</li><li>Link in E-Mail angeklickt</li><li>Link in Verkaufs-E-Mail angeklickt</li><li>Auf Link in SMS-Nachricht geklickt</li><li>Auf einen Link geklickt</li><li>Datenwert geändert</li><li>Hat ein Asset heruntergeladen</li><li>E-Mail war aufgrund eines Bounce-Ereignisses unzustellbar</li><li>E-Mail war aufgrund eines Soft Bounce-Ereignisses unzustellbar</li><li>Hat mit einem Konversationsfluss interagiert</li><li>Hatte eine Interaktion mit einem Dialog</li><li>Interagiert mit einem Agenten im Gesprächsfluss</li><li>Hatte eine Interaktion mit einem Agent per Dialog</li><li>Ausgefülltes Formular</li><li>Hatte interessanten Moment</li><li>Hat während des Ereignisses Fragen gestellt</li><li>Hat am Ereignis teilgenommen</li><li>Interagiert mit einem Dokument im Gesprächsfluss</li><li>Hat per Dialog mit Dokument interagiert</li><li>Lead-Partition geändert</li><li>Lead wurde konvertiert</li><li>Lead wurde erstellt</li><li>Lead wurde aus Microsoft gelöscht</li><li>Lead wurde aus SFDC gelöscht</li><li>Lead wurde nach Marketo verschoben</li><li>Lead wurde mit Microsoft synchronisiert</li><li>Lead wurde mit SFDC synchronisiert</li><li>Nurture-Kadenz geändert</li><li>Pflegespur geändert</li><li>Hat E-Mail geöffnet</li><li>Hat Verkaufs-E-Mail geöffnet</li><li>Gelegenheit (Konto) wurde aktualisiert</li><li>Gelegenheit (Kontakt) wurde aktualisiert</li><li>Opportunity wurde aktualisiert</li><li>Eigentümer wurde geändert</li><li>Besitzer wurde in Microsoft geändert</li><li>Teilnehmerdaten des Programms wurden geändert</li><li>Fortschrittsstatus wurde geändert</li><li>Dialogziel erreicht</li><li>Ziel im Gesprächsfluss erreicht</li><li>Hat E-Mail „An einen Freund weiterleiten“ erhalten</li><li>Hat auf Vertriebs-E-Mail geantwortet</li><li>Hat auf eine Umfrage geantwortet</li><li>Beantwortet einer Umfrage</li><li>Umsatzstadium wurde geändert</li><li>Verkaufs-E-Mail war aufgrund eines Bounce-Ereignisses unzustellbar</li><li>Verkaufs-E-Mail wurde empfangen</li><li>Geplante Besprechung im Gesprächsfluss</li><li>Hat ein Meeting per Dialog geplant</li><li>Bewertung wurde geändert</li><li>Segment geändert</li><li>Hat E-Mail „An einen Freund weiterleiten“ gesendet</li><li>SMS-Nachricht gebounct</li><li>Von E-Mail abgemeldet</li><li>Besuchte Webseite</li><li>Wurde der Liste hinzugefügt</li><li>wurde „Nurture“ hinzugefügt</li><li>Wurde der Opportunity hinzugefügt</li><li>wurde zu Opportunity (Konto) hinzugefügt</li><li>wurde zu Opportunity (Kontakt) hinzugefügt</li><li>War übermittelte E-Mail</li><li>SMS-Nachricht wurde zugestellt</li><li>Wurde aus Liste entfernt</li><li>Wurde aus Opportunity entfernt</li><li>wurde aus Opportunity (Konto) entfernt</li><li>wurde von der Opportunity entfernt (Kontakt)</li><li>War gesendete E-Mail</li><li>War gesendete Verkaufs-E-Mail</li><li>Webhook wird aufgerufen</li></ul> | <ul><li>E-Mail-Adresse des Account-Inhabers</li><li>Vorname des Account-Inhabers</li><li>Nachname des Account-Inhabers</li><li>Akquisitionsdatum</li><li>Akquirierungsprogramm</li><li>Akquirierungsprogramm-Name</li><li>Adresse</li><li>Jahresumsatz</li><li>Anonyme IP</li><li>Rechnungsadresse</li><li>Abrechnungsort</li><li>Abrechnungsland</li><li>Postleitzahl für Abrechnung</li><li>Bundesland für Abrechnung</li><li>Auf der schwarzen Liste</li><li>Stadt</li><li>Microsoft-Typ für Unternehmen</li><li>Firmenname</li><li>Land</li><li>Erstellt um</li><li>Geburtsdatum</li><li>Abteilung</li><li>Nicht aufrufen</li><li>Nicht aufrufen – Ursache</li><li>Doppelte Felder</li><li>E-Mail-Adresse</li><li>E-Mail-Adresse ungültig</li><li>Grund für ungültige E-Mail</li><li>E-Mail angehalten</li><li>E-Mail angehalten am</li><li>Ursache für angehaltene E-Mail</li><li>Faxnummer</li><li>Vorname</li><li>Vollständiger Name</li><li>Hat Opportunity</li><li>Branche</li><li>Abgeleiteter Ort</li><li>Abgeleitetes Unternehmen</li><li>Abgeleitetes Land</li><li>Abgeleiteter Stadtbereich</li><li>Abgeleitete Telefonvorwahl</li><li>Abgeleitete Postleitzahl</li><li>Abgeleitetes Bundesland/abgeleitete Region</li><li>Ist Kunde</li><li>Ist Partner</li><li>Jobtitel</li><li>Nachname</li><li>E-Mail-Adresse der bzw. des Lead-Inhabenden</li><li>Vorname des Lead-Inhabers</li><li>Position der bzw. des Lead-Inhabenden</li><li>Nachname des Lead-Inhabers</li><li>Telefonnummer des Lead-Inhabers</li><li>Lead-Partitionsname</li><li>Lead-Bewertung</li><li>Lead-Bewertung</li><li>Lead-Quelle</li><li>Lead-Status</li><li>Haupttelefonnummer</li><li>Marketing eingestellt</li><li>Mitglied der Feldgruppe</li><li>Listenmitglied</li><li>Mitglied von Nurture</li><li>Mitglied des Programms</li><li>Mitglied des Umsatzmodells</li><li>Mitglied der Umsatzstufe</li><li>Mitglied der SFDC-Kampagne</li><li>Mitglied einer intelligenten Kampagne</li><li>Mitglied einer intelligenten Liste</li><li>Microsoft-Kontonummer</li><li>Microsoft – Erstellungsdatum</li><li>Microsoft wird gelöscht</li><li>Microsoft-Typ</li><li>Zweiter Vorname</li><li>Mobiltelefonnummer</li><li>Hinweise</li><li>Anzahl Mitarbeiter</li><li>Anzahl Gelegenheiten</li><li>Ursprünglicher Verweis</li><li>Ursprüngliche Such-Engine</li><li>Ursprünglicher Suchausdruck</li><li>Ursprüngliche Quelleninfo</li><li>Ursprünglicher Quellentyp</li><li>Name des übergeordneten Unternehmens</li><li>Zeitzone der Person</li><li>Telefonnummer</li><li>Postleitzahl</li><li>Zufälliges Beispiel</li><li>Registrierungsquelleninfo</li><li>Registrierungsquellentyp</li><li>Rolle</li><li>Anrede</li><li>SFDC-Kontonummer</li><li>SFDC Created Date</li><li>SFDC wurde gelöscht</li><li>SFDC-Typ</li><li>SIC-Code</li><li>Seite</li><li>Land</li><li>Opportunity-Gesamtbetrag</li><li>Erwarteter Opportunity-Gesamtumsatz</li><li>Abbestellt</li><li>Ursache für Abbestellung</li><li>Aktualisiert um</li><li>Website</li></ul> |

{style="table-layout:auto"}
