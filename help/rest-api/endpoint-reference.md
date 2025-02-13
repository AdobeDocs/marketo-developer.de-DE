---
title: Endpunktverweis
feature: REST API
description: Marketo API-Endpunktverweise
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 3632d2b713d97a2c895c65f144c07e62e1d369cb
workflow-type: tm+mt
source-wordcount: '4676'
ht-degree: 27%

---

# Endpunktverweis

Im Folgenden finden Sie Links zu den Referenzen zur Marketo REST-API.

- [Asset](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identität](https://developer.adobe.com/marketo-apis/api/identity/)
- [Lead-Datenbank](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Benutzerverwaltung](https://developer.adobe.com/marketo-apis/api/user/)

## Endpunktliste {#endpoint_list}

Im Folgenden finden Sie eine umfassende Liste von REST-API-Endpunkten.

| Name | Gruppe | Methode | URI | Erforderliche Berechtigung |
|---|---|---|---|---|
| Hinzufügen benutzerdefinierter Aktivitäten | Aktivitäten | POST | /rest/v1/activities/external.json | Aktivität mit Lese-/Schreibzugriff |
| Benutzerdefinierten Aktivitätstyp genehmigen | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/approve.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Erstellen benutzerdefinierter Aktivitätstypattribute | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/attributes/create.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Erstellen benutzerdefinierter Aktivitätstypen | Aktivitäten | POST | /rest/v1/activities/external/type.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Benutzerdefinierten Aktivitätstyp löschen | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/delete.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Benutzerdefinierte Aktivitätstypattribute löschen | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/attributes/delete.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Beschreibung des benutzerdefinierten Aktivitätstyps | Aktivitäten | GET | /rest/v1/activities/external/type/{apiName}/describe.json | Metadaten der schreibgeschützten Aktivität |
| Entwurf für benutzerdefinierten Aktivitätstyp verwerfen | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/discardDraft.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Aktivitätstypen abrufen | Aktivitäten | GET | /rest/v1/activities/types.json | Schreibgeschützte Aktivität |
| Abrufen benutzerdefinierter Aktivitätstypen | Aktivitäten | GET | /rest/v1/activities/external/types.json | Metadaten der schreibgeschützten Aktivität |
| Gelöschte Leads abrufen | Aktivitäten | GET | /rest/v1/activities/deletedleads.json | Schreibgeschützte Aktivität |
| Lead-Aktivitäten abrufen | Aktivitäten | GET | /rest/v1/activities.json | Schreibgeschützte Aktivität |
| Lead-Änderungen abrufen | Aktivitäten | GET | /rest/v1/activities/leadchanges.json | Schreibgeschützte Aktivität |
| Paging-Token abrufen | Aktivitäten | GET | /rest/v1/activities/pagingtoken.json | Schreibgeschützte Aktivität |
| Benutzerdefinierten Aktivitätstyp aktualisieren | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Benutzerdefinierte Aktivitätstypattribute aktualisieren | Aktivitäten | POST | /rest/v1/activities/external/type/{apiName}/attributes/update.json | Metadaten der Aktivität mit Lese-/Schreibzugriff |
| Identität | Authentifizierung | GET oder POST | /identity/oauth/token | Keine |
| Exportaktivitätsauftrag abbrechen | Massenexport-Aktivitäten | POST | /bulk/v1/activities/export/{exportid}/cancel.json | Schreibgeschützte Aktivität |
| Exportvorgang erstellen | Massenexport-Aktivitäten | POST | /bulk/v1/activities/export/create.json | Schreibgeschützte Aktivität |
| Exportaktivitätsauftrag in die Warteschlange stellen | Massenexport-Aktivitäten | POST | /bulk/v1/activities/export/{exportid}/enqueue.json | Schreibgeschützte Aktivität |
| Exportaktivitätsdatei abrufen | Massenexport-Aktivitäten | GET | /bulk/v1/activities/export/{exportid}/file.json | Schreibgeschützte Aktivität |
| Abrufen des Status von Exportvorgängen | Massenexport-Aktivitäten | GET | /bulk/v1/activities/export/{exportid}/status.json | Schreibgeschützte Aktivität |
| Abrufen von Exportaktivitätsvorgängen | Massenexport-Aktivitäten | GET | /bulk/v1/activities/export.json | Schreibgeschützte Aktivität |
| Abbrechen des Exports eines benutzerdefinierten Objektvorgangs | Massenexport benutzerdefinierter Objekte | POST | /bulk/v1/customobjects/export/{exportid}/cancel.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Erstellen und Exportieren eines benutzerdefinierten Objektvorgangs | Massenexport benutzerdefinierter Objekte | POST | /bulk/v1/customobjects/export/create.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Einreihen eines benutzerdefinierten Objektvorgangs in die Warteschlange | Massenexport benutzerdefinierter Objekte | POST | /bulk/v1/customobjects/export/{exportid}/enqueue.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Benutzerdefinierte Objektdatei exportieren | Massenexport benutzerdefinierter Objekte | GET | /bulk/v1/customobjects/export/{exportid}/file.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Abrufen des Status eines benutzerdefinierten Objektvorgangs | Massenexport benutzerdefinierter Objekte | GET | /bulk/v1/customobjects/export/{exportid}/status.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Abrufen von benutzerdefinierten Objektaufträgen | Massenexport benutzerdefinierter Objekte | GET | /bulk/v1/customobjects/export.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Exportvorgang abbrechen | Massenexport-Leads | POST | /bulk/v1/lead/export/{exportid}/cancel.json | Schreibgeschützter Lead |
| Exportvorgang erstellen | Massenexport-Leads | POST | /bulk/v1/leads/export/create.json | Schreibgeschützter Lead |
| Lead-Exportvorgang in die Warteschlange stellen | Massenexport-Leads | POST | /bulk/v1/lead/export/{exportid}/enqueue.json | Schreibgeschützter Lead |
| Exportdatei abrufen | Massenexport-Leads | GET | /bulk/v1/lead/export/{exportid}/file.json | Schreibgeschützter Lead |
| Abrufen des Status von Exportvorgängen | Massenexport-Leads | GET | /bulk/v1/lead/export/{exportid}/status.json | Schreibgeschützter Lead |
| Abrufen von Exportvorgängen | Massenexport-Leads | GET | /bulk/v1/leads/export.json | Schreibgeschützter Lead |
| Abbrechen des Abonnentenauftrags für Exportprogramm | Massenexport-Programmmitglieder | POST | /bulk/v1/program/members/export/{exportid}/cancel.json | Schreibgeschützter Lead |
| Programmmitgliedsvorgang erstellen | Massenexport-Programmmitglieder | POST | /bulk/v1/program/members/export/create.json | Schreibgeschützter Lead |
| Programmmitgliedsauftrag in Warteschlange stellen | Massenexport-Programmmitglieder | POST | /bulk/v1/program/members/export/{exportid}/enqueue.json | Schreibgeschützter Lead |
| Abrufen der Mitgliederdatei des Exportprogramms | Massenexport-Programmmitglieder | GET | /bulk/v1/program/members/export/{exportid}/file.json | Schreibgeschützter Lead |
| Abrufen des Status des Exportprogrammmitglieds | Massenexport-Programmmitglieder | GET | /bulk/v1/program/members/export/{exportid}/status.json | Schreibgeschützter Lead |
| Abrufen von Exportprogrammmitgliedsvorgängen | Massenexport-Programmmitglieder | GET | /bulk/v1/program/members/export.json | Schreibgeschützter Lead |
| Fehler beim Importieren benutzerdefinierter Objekte abrufen | Massenimport von benutzerdefinierten Objekten | GET | /bulk/v1/customobjects/import/{id}/failures.json | Objekt mit Lese-/Schreibzugriff |
| Benutzerdefinierten Objektstatus importieren abrufen | Massenimport von benutzerdefinierten Objekten | GET | /bulk/v1/customobjects/import/{id}/status.json | Objekt mit Lese-/Schreibzugriff |
| Abrufen von Warnungen zu benutzerdefinierten Objekten | Massenimport von benutzerdefinierten Objekten | GET | /bulk/v1/customobjects/import/{id}/warnings.json | Objekt mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte importieren | Massenimport von benutzerdefinierten Objekten | POST | /bulk/v1/customobjects/{apiName}/import.json | Objekt mit Lese-/Schreibzugriff |
| Abrufen von Lead-Importfehlern | Massenimport von Leads | GET | /bulk/v1/lead/batch/{id}/failures.json | Lead mit Lese-/Schreibzugriff |
| Lead-Importstatus abrufen | Massenimport von Leads | GET | /bulk/v1/lead/batch/{id}.json | Lead mit Lese-/Schreibzugriff |
| Abrufen von Lead-Importwarnungen | Massenimport von Leads | GET | /bulk/v1/lead/batch/{id}/warnings.json | Lead mit Lese-/Schreibzugriff |
| Leads importieren | Massenimport von Leads | POST | /bulk/v1/leads.json | Lead mit Lese-/Schreibzugriff |
| Abrufen von Fehlern bei Programmmitgliedern | Massenimport-Programmmitglieder | GET | /bulk/v1/program/members/import/{id}/failures.json | Lead mit Lese-/Schreibzugriff |
| Abrufen des Status eines Programmmitglieds | Massenimport-Programmmitglieder | GET | /bulk/v1/program/members/import/{id}/status.json | Lead mit Lese-/Schreibzugriff |
| Abrufen von Warnhinweisen zu Mitgliedern des Importprogramms | Massenimport-Programmmitglieder | GET | /bulk/v1/program/members/import/{id}/warnings.json | Lead mit Lese-/Schreibzugriff |
| Programmmitglieder importieren | Massenimport-Programmmitglieder | POST | /bulk/v1/program/{programId}/members/import.json | Lead mit Lese-/Schreibzugriff |
| Abrufen einer Kampagne nach ID | Kampagnen | GET | /rest/v1/campaigns/{id}.json | Schreibgeschützte Kampagnen |
| Abrufen von Kampagnen | Kampagnen | GET | /rest/v1/campaigns.json | Schreibgeschützte Kampagnen |
| Kampagne anfordern | Kampagnen | POST | /rest/v1/campaigns/{id}/trigger.json | Kampagnen mit Lese- und Schreibzugriff |
| Kampagne planen | Kampagnen | POST | /rest/v1/campaigns/{id}/schedule.json | Kampagnen mit Lese- und Schreibzugriff |
| Kanal nach Namen abrufen | Kanäle | GET | /rest/asset/v1/channel/byName.json | Schreibgeschütztes Asset |
| Kanäle abrufen | Kanäle | GET | /rest/asset/v1/channels.json | Schreibgeschütztes Asset |
| Firmen löschen | Firmen | POST | /rest/v1/companies/delete.json | Unternehmen mit Lese-/Schreibzugriff |
| Firmen beschreiben | Firmen | GET | /rest/v1/companies/describe.json | Schreibgeschütztes Unternehmen |
| Firmen abrufen | Firmen | GET | /rest/v1/companies.json | Schreibgeschütztes Unternehmen |
| Firmen synchronisieren | Firmen | POST | /rest/v1/companies.json | Unternehmen mit Lese-/Schreibzugriff |
| Unternehmensfeld nach Namen abrufen | Firmen | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Firmenfelder abrufen | Firmen | GET | /rest/v1/companies/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Hinzufügen benutzerdefinierter Objekttypfelder | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/addField.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Benutzerdefinierten Objekttyp genehmigen | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/approve.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte löschen | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/{name}/delete.json | Objekt mit Lese-/Schreibzugriff |
| Benutzerdefinierten Objekttyp löschen | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/delete.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekttypfelder löschen | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/deleteField.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Beschreiben benutzerdefinierter Objekte | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/{name}/describe.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Beschreiben des benutzerdefinierten Objekttyps | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/schema/{apiName}/describe.json | Schreibgeschützter benutzerdefinierter Objekttyp |
| Entwurf für benutzerdefinierten Objekttyp verwerfen | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/discardDraft.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Benutzerdefinierte Objekte abrufen | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/{name}.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Abrufen benutzerdefinierter objektverknüpfbarer Objekte | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/schema/linkableObjects.json | Schreibgeschützter benutzerdefinierter Objekttyp |
| Abrufen benutzerdefinierter objektabhängiger Assets | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/schema/{apiName}/dependentAssets.json | Schreibgeschützter benutzerdefinierter Objekttyp |
| Abrufen benutzerdefinierter Felddatentypen für Objekttyp | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Schreibgeschützter benutzerdefinierter Objekttyp |
| Liste benutzerdefinierter Objekte | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects.json | Schreibgeschütztes benutzerdefiniertes Objekt |
| Liste benutzerdefinierter Objekttypen | Benutzerdefinierte Objekte | GET | /rest/v1/customobjects/schema.json | Schreibgeschützter benutzerdefinierter Objekttyp |
| Benutzerdefinierte Objekte synchronisieren | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/{name}.json | Objekt mit Lese-/Schreibzugriff |
| Benutzerdefinierten Objekttyp synchronisieren | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| Benutzerdefiniertes Feld für Objekttyp aktualisieren | Benutzerdefinierte Objekte | POST | /rest/v1/customobjects/schema/{apiName}/updateField.json | Benutzerdefinierter Objekttyp mit Lese-/Schreibzugriff |
| E-Mail-Vorlagenentwurf genehmigen | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Vorlage klonen | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Vorlage erstellen | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplates.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Vorlage löschen | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Vorlagenentwurf verwerfen | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Vorlage nach ID abrufen | E-Mail-Vorlagen | GET | /rest/asset/v1/emailTemplate/{id}.json | Schreibgeschütztes Asset |
| E-Mail-Vorlage nach Namen abrufen | E-Mail-Vorlagen | GET | /rest/asset/v1/emailTemplate/byName.json | Schreibgeschütztes Asset |
| Abrufen des Inhalts einer E-Mail-Vorlage nach ID | E-Mail-Vorlagen | GET | /rest/asset/v1/emailTemplate/{id}/content.json | Schreibgeschütztes Asset |
| E-Mail-Vorlage abrufen, die von verwendet wird | E-Mail-Vorlagen | GET | /rest/asset/v1/emailTemplates/{id}/usedBy.json | Schreibgeschütztes Asset |
| E-Mail-Vorlagen abrufen | E-Mail-Vorlagen | GET | /rest/asset/v1/emailTemplates.json | Schreibgeschütztes Asset |
| Genehmigung des E-Mail-Vorlagenentwurfs aufheben | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| Inhalt der E-Mail-Vorlage aktualisieren | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Aktualisieren von E-Mail-Vorlagen-Metadaten | E-Mail-Vorlagen | POST | /rest/asset/v1/emailTemplate/{id}.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Modul hinzufügen | E-Mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/add.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Entwurf genehmigen | E-Mails | POST | /rest/asset/v1/email/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| E-Mail klonen | E-Mails | POST | /rest/asset/v1/email/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| E-Mail erstellen | E-Mails | POST | /rest/asset/v1/emails.json | Asset mit Lese- und Schreibzugriff |
| E-Mail löschen | E-Mails | POST | /rest/asset/v1/email/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Modul löschen | E-Mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/delete.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Entwurf verwerfen | E-Mails | POST | /rest/asset/v1/email/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Modul duplizieren | E-Mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json | Asset mit Lese- und Schreibzugriff |
| E-Mail nach ID abrufen | E-Mails | GET | /rest/asset/v1/email/{id}.json | Schreibgeschütztes Asset |
| E-Mail nach Namen abrufen | E-Mails | GET | /rest/asset/v1/email/byName.json | Schreibgeschütztes Asset |
| E-Mail-Inhalt abrufen | E-Mails | GET | /rest/asset/v1/email/{id}/content.json | Schreibgeschütztes Asset |
| Dynamische E-Mail-Inhalte abrufen | E-Mails | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Schreibgeschütztes Asset |
| Vollständigen Inhalt der E-Mail abrufen | E-Mails | GET | /rest/asset/v1/email/{id}/fullContent.json | Schreibgeschütztes Asset |
| Abrufen von E-Mail-Variablen | E-Mails | GET | /rest/asset/v1/email/{id}/variables.json | Schreibgeschütztes Asset |
| Abrufen von E-Mail-CC-Feldern | E-Mails | GET | /rest/asset/v1/email/ccFields.json | Schreibgeschütztes Asset |
| E-Mails abrufen | E-Mails | GET | /rest/asset/v1/emails.json | Schreibgeschütztes Asset |
| E-Mail-Module neu anordnen | E-Mails | POST | /rest/asset/v1/email/{id}/content/rearrange.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Modul umbenennen | E-Mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/rename.json | Asset mit Lese- und Schreibzugriff |
| Beispiel-E-Mail senden | E-Mails | POST | /rest/asset/v1/email/{id}/sendSample.json | Asset mit Lese- und Schreibzugriff |
| Genehmigung für E-Mail aufheben | E-Mails | POST | /rest/asset/v1/email/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Inhalt aktualisieren | E-Mails | POST | /rest/asset/v1/email/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Abschnitt zum Aktualisieren des E-Mail-Inhalts | E-Mails | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Asset mit Lese- und Schreibzugriff |
| Abschnitt „Dynamischen Inhalt von E-Mails aktualisieren“ | E-Mails | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Asset mit Lese- und Schreibzugriff |
| Vollständigen Inhalt der E-Mail aktualisieren | E-Mails | POST | /rest/asset/v1/emails/{id}/fullContent.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Metadaten aktualisieren | E-Mails | POST | /rest/asset/v1/email/{id}.json | Asset mit Lese- und Schreibzugriff |
| E-Mail-Variable aktualisieren | E-Mails | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Asset mit Lese- und Schreibzugriff |
| Datei erstellen | Dateien | POST | /rest/asset/v1/files.json | Asset mit Lese- und Schreibzugriff |
| Datei nach ID abrufen | Dateien | GET | /rest/asset/v1/file/{id}.json | Schreibgeschütztes Asset |
| Datei nach Namen abrufen | Dateien | GET | /rest/asset/v1/file/byName.json | Schreibgeschütztes Asset |
| Dateien abrufen | Dateien | GET | /rest/asset/v1/files.json | Schreibgeschütztes Asset |
| Dateiinhalt aktualisieren | Dateien | POST | /rest/asset/v1/file/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Ordner erstellen | Ordner | POST | /rest/asset/v1/folders.json | Asset mit Lese- und Schreibzugriff |
| Ordner löschen | Ordner | POST | /rest/asset/v1/folder/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Ordner nach ID abrufen | Ordner | GET | /rest/asset/v1/folder/{id}.json | Schreibgeschütztes Asset |
| Ordner nach Namen abrufen | Ordner | GET | /rest/asset/v1/folder/byName.json | Schreibgeschütztes Asset |
| Abrufen von Ordnerinhalten | Ordner | GET | /rest/asset/v1/folder/{id}/content.json | Schreibgeschütztes Asset |
| Ordner abrufen | Ordner | GET | /rest/asset/v1/folders.json | Schreibgeschütztes Asset |
| Aktualisieren von Ordnermetadaten | Ordner | POST | /rest/asset/v1/folder/{id}.json | Asset mit Lese- und Schreibzugriff |
| Feld zum Formular hinzufügen | Formularfelder | POST | /rest/asset/v1/form/{id}/fields.json | Asset mit Lese- und Schreibzugriff |
| Feldsatz zum Formular hinzufügen | Formularfelder | POST | /rest/asset/v1/form/{id}/fieldSet.json | Asset mit Lese- und Schreibzugriff |
| Hinzufügen von Sichtbarkeitsregeln für Formularfelder | Formularfelder | POST | /rest/asset/v1/form/{formId}/field/{fieldId}/visibility.json | Asset mit Lese- und Schreibzugriff |
| Rich-Text-Feld hinzufügen | Formularfelder | POST | /rest/asset/v1/form/{id}/richText.json | Asset mit Lese- und Schreibzugriff |
| Feld aus Feldsatz löschen | Formularfelder | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId}/delete.json | Asset mit Lese- und Schreibzugriff |
| Formularfeld löschen | Formularfelder | POST | /rest/asset/v1/form/{id}/field/{fieldId}/delete.json | Asset mit Lese- und Schreibzugriff |
| Verfügbare Formularfelder abrufen | Formularfelder | GET | /rest/asset/v1/form/fields.json | Schreibgeschütztes Asset |
| Verfügbare Felder für Programmteilnehmer abrufen | Formularfelder | GET | /rest/asset/v1/form/programMemberFields.json | Schreibgeschütztes Asset |
| Abrufen von Feldern für das Formular | Formularfelder | GET | /rest/asset/v1/form/{id}/fields.json | Schreibgeschütztes Asset |
| Feldpositionen aktualisieren | Formularfelder | POST | /rest/asset/v1/form/{id}/reArrange.json | Asset mit Lese- und Schreibzugriff |
| Formularfeld aktualisieren | Formularfelder | POST | /rest/asset/v1/form/{id}/field/{fieldId}.json | Asset mit Lese- und Schreibzugriff |
| Formularentwurf genehmigen | Formulare | POST | /rest/asset/v1/form/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| Formular klonen | Formulare | POST | /rest/asset/v1/form/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Formular erstellen | Formulare | POST | /rest/asset/v1/forms.json | Asset mit Lese- und Schreibzugriff |
| Formular abrufen von | Formulare | GET | /rest/asset/v1/form/{id}/usedBy.json | Asset mit Lese- und Schreibzugriff |
| Formular löschen | Formulare | POST | /rest/asset/v1/form/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Formularentwurf verwerfen | Formulare | POST | /rest/asset/v1/form/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| Formular nach ID abrufen | Formulare | GET | /rest/asset/v1/form/{id}.json | Schreibgeschütztes Asset |
| Formular nach Namen abrufen | Formulare | GET | /rest/asset/v1/form/byName.json | Schreibgeschütztes Asset |
| Forms abrufen | Formulare | GET | /rest/asset/v1/forms.json | Schreibgeschütztes Asset |
| Dankesseite nach Formular-ID abrufen | Formulare | GET | /rest/asset/v1/form/{id}/thankYouPage.json | Schreibgeschütztes Asset |
| Aktualisieren von Formularmetadaten | Formulare | POST | /rest/asset/v1/form/{id}.json | Asset mit Lese- und Schreibzugriff |
| Schaltfläche „Senden aktualisieren“ | Formulare | POST | /rest/asset/v1/{id}/submitButton.json | Asset mit Lese- und Schreibzugriff |
| Dankesseite aktualisieren | Formulare | POST | /rest/asset/v1/form/{id}/thankYouPage.json | Asset mit Lese- und Schreibzugriff |
| Abschnitt zum Hinzufügen von Landingpage-Inhalten | Inhalt der Landingpage | POST | /rest/asset/v1/landingPage/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Abschnitt zum Löschen des Landingpage-Inhalts | Inhalt der Landingpage | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}/delete.json | Asset mit Lese- und Schreibzugriff |
| Abrufen von Landingpage-Inhalten | Inhalt der Landingpage | GET | /rest/asset/v1/landingPage/{id}/content.json | Schreibgeschütztes Asset |
| Abrufen dynamischer Landingpage-Inhalte | Inhalt der Landingpage | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Schreibgeschütztes Asset |
| Abschnitt zum Aktualisieren des Landingpage-Inhalts | Inhalt der Landingpage | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}.json | Asset mit Lese- und Schreibzugriff |
| Abschnitt zu dynamischen Inhalten für Landingpages aktualisieren | Inhalt der Landingpage | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Asset mit Lese- und Schreibzugriff |
| Entwurf der Landingpage-Vorlage genehmigen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| Landing Page-Vorlage klonen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Landing Page-Vorlage erstellen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate.json | Asset mit Lese- und Schreibzugriff |
| Landing Page-Vorlage löschen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Entwurf der Landingpage-Vorlage verwerfen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| Landingpage-Vorlage nach ID abrufen | Landing Page-Vorlagen | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Schreibgeschütztes Asset |
| Landingpage-Vorlage nach Namen abrufen | Landing Page-Vorlagen | GET | /rest/asset/v1/landingPageTemplates/byName.json | Schreibgeschütztes Asset |
| Abrufen des Inhalts einer Landingpage-Vorlage | Landing Page-Vorlagen | GET | /rest/asset/v1/landingPageTemplate/{id}/content.json | Schreibgeschütztes Asset |
| Abrufen von Landingpage-Vorlagen | Landing Page-Vorlagen | GET | /rest/asset/v1/landingPageTemplates.json | Schreibgeschütztes Asset |
| Genehmigung für Landing Page-Vorlage entziehen | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| Inhalt der Landingpage-Vorlage aktualisieren | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Aktualisieren der Metadaten der Landingpage-Vorlage | Landing Page-Vorlagen | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Asset mit Lese- und Schreibzugriff |
| Landingpage-Entwurf genehmigen | Landing Page | POST | /rest/asset/v1/landingPage/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| Landing Page klonen | Landing Page | POST | /rest/asset/v1/landingPage/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Landingpages erstellen | Landing Page | POST | /rest/asset/v1/landingPages.json | Asset mit Lese- und Schreibzugriff |
| Landing Page löschen | Landing Page | POST | /rest/asset/v1/landingPage/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Entwurf der Landingpage verwerfen | Landing Page | POST | /rest/asset/v1/landingPage/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| Landingpage nach ID abrufen | Landing Page | GET | /rest/asset/v1/landingPage/{id}.json | Schreibgeschütztes Asset |
| Landingpage nach Namen abrufen | Landing Page | GET | /rest/asset/v1/landingPage/byName.json | Schreibgeschütztes Asset |
| Abrufen von Landingpage-Variablen | Landing Page | GET | /rest/asset/v1/landingPage/{id}/variables.json | Schreibgeschütztes Asset |
| Landingpages abrufen | Landing Page | GET | /rest/asset/v1/landingPages.json | Schreibgeschütztes Asset |
| Vorschau der Landingpage | Landing Page | GET | /rest/asset/v1/landingPage/{id}/preview.json | Schreibgeschütztes Asset |
| Genehmigung der Landing Page aufheben | Landing Page | POST | /rest/asset/v1/landingPage/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| Aktualisieren von Landingpage-Metadaten | Landing Page | POST | /rest/asset/v1/{id}.json | Asset mit Lese- und Schreibzugriff |
| Aktualisieren von Landingpage-Variablen | Landing Page | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId}.json | Asset mit Lese- und Schreibzugriff |
| Erstellen von Umleitungsregeln für Landingpages | Landing Page | POST | /rest/asset/v1/redirectRules.json | Schreib-Lese-Umleitungsregeln |
| Umleitungsregel für Landingpage löschen | Landing Page | POST | /rest/asset/v1/redirectRule/{id}/delete.json | Schreib-Lese-Umleitungsregeln |
| Regeln für Landingpage-Umleitung abrufen | Landing Page | GET | /rest/asset/v1/redirectRules.json | Schreibgeschützte Umleitungsregeln |
| Abrufen der Umleitungsregel für Landingpages nach ID | Landing Page | GET | /rest/asset/v1/redirectRule/{id}.json | Schreibgeschützte Umleitungsregeln |
| Aktualisieren der Umleitungsregel für Landingpages | Landing Page | POST | /rest/asset/v1/redirectRule/{id}.json | Schreib-Lese-Umleitungsregeln |
| Landingpage-Domains abrufen | Landing Page | GET | /rest/asset/v1/landingPageDomains.json | Schreibgeschützte Umleitungsregeln |
| Lead verknüpfen | Leads | POST | /rest/v1/lead/{id}/associate.json | Lead mit Lese-/Schreibzugriff |
| Status des Lead-Programms ändern | Leads | POST | /rest/v1/lead/programs/{programId}/status.json | Lead mit Lese-/Schreibzugriff |
| Leads löschen | Leads | POST | /rest/v1/leads.json | Lead mit Lese-/Schreibzugriff |
| Lead beschreiben | Leads | GET | /rest/v1/leads/describe.json | Schreibgeschützter Lead |
| Lead2 beschreiben | Leads | GET | /rest/v1/leads/describe2.json | Schreibgeschützter Lead |
| Programmteilnehmer beschreiben | Leads | GET | /rest/v1/program/members/describe.json | Schreibgeschützter Lead |
| Lead nach ID abrufen | Leads | GET | /rest/v1/lead/{id}.json | Schreibgeschützter Lead |
| Lead-Partitionen abrufen | Leads | GET | /rest/v1/leads/partitions.json | Schreibgeschützter Lead |
| Leads nach Filtertyp abrufen | Leads | GET | /rest/v1/leads.json | Schreibgeschützter Lead |
| Leads nach Programm-ID abrufen | Leads | GET | /rest/v1/lead/programs/{programId}.json | Schreibgeschützter Lead |
| Leads zusammenführen | Leads | POST | /rest/v1/lead/{id}/merge.json | Lead mit Lese-/Schreibzugriff |
| Abrufen von Listen nach Lead-ID | Leads | GET | /rest/v1/lead/{leadId}.json | Schreibgeschütztes Asset |
| Abrufen von Programmen nach Lead-ID | Leads | GET | /rest/v1/lead/{leadId}programMembership.json | Schreibgeschütztes Asset |
| Smart-Kampagnen nach Lead-ID abrufen | Leads | GET | /rest/v1/lead/{leadId}/smartCampaignMembership.json | Schreibgeschütztes Asset |
| Lead zu Marketo pushen | Leads | POST | /rest/v1/leads/partitions.json | Lead mit Lese-/Schreibzugriff |
| Formular senden | Leads | POST | /rest/v1/leads/submitForm.json | Lead mit Lese-/Schreibzugriff |
| Leads synchronisieren | Leads | POST | /rest/v1/leads.json | Lead mit Lese-/Schreibzugriff |
| Lead-Partition aktualisieren | Leads | POST | /rest/v1/leads/partitions.json | Lead mit Lese-/Schreibzugriff |
| Lead-Feld nach Namen abrufen | Leads | GET | /rest/v1/lead/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Lead-Felder abrufen | Leads | GET | /rest/v1/leads/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Lead-Felder erstellen | Leads | POST | /rest/v1/leads/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Lead-Feld aktualisieren | Leads | POST | /rest/v1/lead/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Hinzufügen von benannten Mitgliedern der Kontoliste | Spezifische Kontolisten | POST | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Benanntes Konto mit Lese-/Schreibzugriff |
| Löschen von Listen benannter Konten | Spezifische Kontolisten | POST | /rest/v1/namedaccountlists/delete.json | Liste benannter Konten mit Lese-/Schreibzugriff |
| Abrufen benannter Mitglieder der Kontoliste | Spezifische Kontolisten | GET | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Schreibgeschütztes benanntes Konto |
| Abrufen von Listen benannter Konten | Spezifische Kontolisten | GET | /rest/v1/namedaccountlists.json | Schreibgeschützte Liste benannter Konten |
| Spezifische Mitglieder der Kontoliste entfernen | Spezifische Kontolisten | POST | /rest/v1/namedaccountlist/{id}/namedaccounts/remove.json | Benanntes Konto mit Lese-/Schreibzugriff |
| Listen benannter Konten synchronisieren | Spezifische Kontolisten | POST | /rest/v1/namedaccountlists.json | Liste benannter Konten mit Lese-/Schreibzugriff |
| Benannte Konten löschen | Benannte Konten | POST | /rest/v1/namedaccounts/delete.json | Benanntes Konto mit Lese-/Schreibzugriff |
| Spezifische Konten beschreiben | Benannte Konten | GET | /rest/v1/namedaccounts/describe.json | Schreibgeschütztes benanntes Konto |
| Benannte Konten abrufen | Benannte Konten | GET | /rest/v1/namedaccounts.json | Schreibgeschütztes benanntes Konto |
| Benannte Konten synchronisieren | Benannte Konten | POST | /rest/v1/namedaccounts.json | Benanntes Konto mit Lese-/Schreibzugriff |
| Benanntes Kontofeld nach Namen abrufen | Benannte Konten | GET | /rest/v1/namedaccounts/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Benannte Kontofelder abrufen | Benannte Konten | GET | /rest/v1/namedaccounts/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Opportunities löschen | Opportunitys | POST | /rest/v1/opportunities/delete.json | Verkaufschance mit Lese-/Schreibzugriff |
| Opportunity-Rollen löschen | Opportunitys | POST | /rest/v1/opportunities/roles/delete.json | Verkaufschance mit Lese-/Schreibzugriff |
| Opportunity beschreiben | Opportunitys | GET | /rest/v1/opportunities/describe.json | Schreibgeschützte Geschäftschance |
| Opportunity-Rolle beschreiben | Opportunitys | GET | /rest/v1/opportunities/roles/describe.json | Schreibgeschützte Geschäftschance |
| Opportunities abrufen | Opportunitys | GET | /rest/v1/opportunities.json | Schreibgeschützte Geschäftschance |
| Opportunity-Rollen abrufen | Opportunitys | GET | /rest/v1/opportunities/roles.json | Schreibgeschützte Geschäftschance |
| Opportunities synchronisieren | Opportunitys | POST | /rest/v1/opportunities.json | Verkaufschance mit Lese-/Schreibzugriff |
| Opportunity-Rollen synchronisieren | Opportunitys | POST | /rest/v1/opportunities/roles.json | Verkaufschance mit Lese-/Schreibzugriff |
| Opportunity-Feld nach Namen abrufen | Opportunitys | GET | /rest/v1/unities/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Opportunity-Felder abrufen | Opportunitys | GET | /rest/v1/opportunities/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Programmmitglieder löschen | Programm-Mitglieder | POST | /rest/v1/programs/{programId}/members/delete.json | Lead mit Lese-/Schreibzugriff |
| Programmteilnehmer beschreiben | Programm-Mitglieder | GET | /rest/v1/programs/members/describe.json | Schreibgeschützter Lead |
| Abrufen von Programmmitgliedern | Programm-Mitglieder | GET | /rest/v1/programs/{programId}/members.json | Schreibgeschützter Lead |
| Teilnehmerdaten des Programms synchronisieren | Programm-Mitglieder | POST | /rest/v1/programs/{programId}/members.json | Lead mit Lese-/Schreibzugriff |
| Status des Programmmitglieds synchronisieren | Programm-Mitglieder | POST | /rest/v1/programs/{programId}/members/status.json | Lead mit Lese-/Schreibzugriff |
| Feld für Programmteilnehmer nach Namen abrufen | Programm-Mitglieder | GET | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Abrufen der Felder für Programmteilnehmer | Programm-Mitglieder | GET | /rest/v1/programs/members/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Erstellen von Programmteilnehmerfeldern | Programm-Mitglieder | POST | /rest/v1/programs/members/schema/fields.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Feld für Programmteilnehmer aktualisieren | Programm-Mitglieder | POST | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Lese-Schreib-Schema – Benutzerdefiniertes Feld |
| Programm genehmigen | Programme | POST | /rest/asset/v1/program/{id}/approve.json | Asset mit Lese- und Schreibzugriff |
| Programm klonen | Programme | POST | /rest/asset/v1/program/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Erstellen von Programmen | Programme | POST | /rest/asset/v1/programs.json | Asset mit Lese- und Schreibzugriff |
| Programm löschen | Programme | POST | /rest/asset/v1/program/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Programm nach ID abrufen | Programme | GET | /rest/asset/v1/program/{id}.json | Schreibgeschütztes Asset |
| Programm nach Namen abrufen | Programme | GET | /rest/asset/v1/program/byName.json | Schreibgeschütztes Asset |
| Programme abrufen | Programme | GET | /rest/asset/v1/programs.json | Schreibgeschütztes Asset |
| Programme nach Tag abrufen | Programme | GET | /rest/asset/v1/program/byTag.json | Schreibgeschütztes Asset |
| Smart-Liste nach Programm-ID abrufen | Programme | GET | /rest/asset/v1/program/{id}/smartList.json | Schreibgeschütztes Asset |
| Genehmigung für Programm entziehen | Programme | POST | /rest/asset/v1/program/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| Programmmetadaten aktualisieren | Programme | POST | /rest/asset/v1/program/{id}.json | Asset mit Lese- und Schreibzugriff |
| Programm-Tag aktualisieren | Programme | POST | /rest/asset/v1/program/{id}/tag/{tagType}.json | Asset mit Lese- und Schreibzugriff |
| Programm-Tag löschen | Programme | POST | /rest/asset/v1/program/{id}/tag/{tagType}/delete.json | Asset mit Lese- und Schreibzugriff |
| Vertriebspersonen löschen | Vertriebspersonal | POST | /rest/v1/salespersons/delete.json | Verkaufsmitarbeiter mit Lese-/Schreibzugriff |
| Vertriebsmitarbeiter beschreiben | Vertriebspersonal | GET | /rest/v1/salespersons/describe.json | Schreibgeschützter Verkaufsmitarbeiter |
| Vertriebspersonen abrufen | Vertriebspersonal | GET | /rest/v1/salespersons.json | Schreibgeschützter Verkaufsmitarbeiter |
| Vertriebspersonen synchronisieren | Vertriebspersonal | POST | /rest/v1/salespersons.json | Verkaufsmitarbeiter mit Lese-/Schreibzugriff |
| Abrufen von Segmentierungen | Segmente | GET | /rest/asset/v1/segmentation.json | Schreibgeschütztes Asset |
| Abrufen von Segmenten für Segmente | Segmente | GET | /rest/asset/v1/segmentation/{id}/segments.json | Schreibgeschütztes Asset |
| Intelligente Kampagne aktivieren | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaign/{id}/activate.json | Kampagne aktivieren |
| Intelligente Kampagne klonen | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaign/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Intelligente Kampagne erstellen | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaigns.json | Asset mit Lese- und Schreibzugriff |
| Smart Campaign deaktivieren | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaign/{id}/deactivate.json | Kampagne deaktivieren |
| Intelligente Kampagne löschen | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaign/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Intelligente Kampagnen | Intelligente Kampagnen | GET | /rest/asset/v1/smartCampaigns.json | Schreibgeschütztes Asset |
| Smart Campaign abrufen nach ID | Intelligente Kampagnen | GET | /rest/asset/v1/smartCampaign/{id}.json | Schreibgeschütztes Asset |
| Smart-Kampagne nach Namen abrufen | Intelligente Kampagnen | GET | /rest/asset/v1/smartCampaign/byName.json | Schreibgeschütztes Asset |
| Smart-Liste nach Smart-Kampagnen-ID abrufen | Intelligente Kampagnen | GET | /rest/asset/v1/smartCampaign/{id}/smartList.json | Schreibgeschütztes Asset |
| Intelligente Kampagne aktualisieren | Intelligente Kampagnen | POST | /rest/asset/v1/smartCampaign/{id}.json | Asset mit Lese- und Schreibzugriff |
| Intelligente Liste klonen | Intelligente Listen | POST | /rest/asset/v1/smartList/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Intelligente Liste löschen | Intelligente Listen | POST | /rest/asset/v1/smartList/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Smart-Liste nach ID abrufen | Intelligente Listen | GET | /rest/asset/v1/smartList/{id}.json | Schreibgeschütztes Asset |
| Smart-Liste nach Namen abrufen | Intelligente Listen | GET | /rest/asset/v1/smartList/byName.json | Schreibgeschütztes Asset |
| Abrufen von Smart-Listen | Intelligente Listen | GET | /rest/asset/v1/smartLists.json | Schreibgeschütztes Asset |
| Snippet-Entwurf genehmigen | Snippets | POST | /rest/asset/v1/snippet/{id}/approveDraft.json | Asset mit Lese- und Schreibzugriff |
| Ausschnitt klonen | Snippets | POST | /rest/asset/v1/snippet/{id}/clone.json | Asset mit Lese- und Schreibzugriff |
| Snippet erstellen | Snippets | POST | /rest/asset/v1/snippets.json | Asset mit Lese- und Schreibzugriff |
| Ausschnitt löschen | Snippets | POST | /rest/asset/v1/snippet/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Entwurf für Ausschnitt verwerfen | Snippets | POST | /rest/asset/v1/snippet/{id}/discardDraft.json | Asset mit Lese- und Schreibzugriff |
| Dynamische Inhalte abrufen | Snippets | GET | /rest/asset/v1/snippet/{id}/dynamicContent.json | Schreibgeschütztes Asset |
| Ausschnitt nach ID abrufen | Snippets | GET | /rest/asset/v1/snippet/{id}.json | Schreibgeschütztes Asset |
| Snippet-Inhalt abrufen | Snippets | GET | /rest/asset/v1/snippet/{id}/content.json | Schreibgeschütztes Asset |
| Snippets abrufen | Snippets | GET | /rest/asset/v1/snippets.json | Schreibgeschütztes Asset |
| Genehmigung für Ausschnitt aufheben | Snippets | POST | /rest/asset/v1/snippet/{id}/unapprove.json | Asset mit Lese- und Schreibzugriff |
| Snippet-Inhalt aktualisieren | Snippets | POST | /rest/asset/v1/snippet/{id}/content.json | Asset mit Lese- und Schreibzugriff |
| Dynamischen Snippet-Inhalt aktualisieren | Snippets | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId}.json | Asset mit Lese- und Schreibzugriff |
| Snippet-Metadaten aktualisieren | Snippets | POST | /rest/asset/v1/snippet/{id}.json | Asset mit Lese- und Schreibzugriff |
| Zu Liste hinzufügen | Statische Listen | POST | /rest/v1/lists/{listId}/leads.json | Lead mit Lese-/Schreibzugriff |
| Erstellen einer statischen Liste | Statische Listen | POST | /asset/v1/staticLists.json | Asset mit Lese- und Schreibzugriff |
| Statische Liste löschen | Statische Listen | POST | /asset/v1/staticList/{id}/delete.json | Asset mit Lese- und Schreibzugriff |
| Leads nach Listen-ID abrufen | Statische Listen | GET | /rest/v1/lists/{listId}/leads.json | Schreibgeschützter Lead |
| Liste nach ID abrufen | Statische Listen | GET | /rest/v1/lists/{id}.json | Schreibgeschützter Lead |
| Abrufen von Listen | Statische Listen | GET | /rest/v1/lists.json | Schreibgeschützter Lead |
| Abrufen der statischen Liste nach ID | Statische Listen | GET | /asset/v1/staticList/{id}.json | Schreibgeschütztes Asset |
| Abrufen einer statischen Liste nach Namen | Statische Listen | GET | /asset/v1/staticList/byName.json | Schreibgeschütztes Asset |
| Abrufen von statischen Listen | Statische Listen | GET | /asset/v1/staticLists.json | Schreibgeschütztes Asset |
| Listenmitglied | Statische Listen | GET | /rest/v1/lists/{listId}/leads/ismember.json | Schreibgeschützter Lead |
| Aus Liste entfernen | Statische Listen | DELETE | /rest/v1/lists/{listId}/leads.json | Lead mit Lese-/Schreibzugriff |
| Aktualisieren von Metadaten der statischen Liste | Statische Listen | POST | /asset/v1/staticList/{id}.json | Asset mit Lese- und Schreibzugriff |
| Tag nach Namen abrufen | Tags | GET | /rest/asset/v1/tagType/byName.json | Schreibgeschütztes Asset |
| Tag-Typen abrufen | Tags | GET | /rest/asset/v1/tagTypes.json | Schreibgeschütztes Asset |
| Token erstellen | Token | POST | /rest/asset/v1/folder/{id}/tokens.json | Asset mit Lese- und Schreibzugriff |
| Token nach Namen löschen | Token | POST | /rest/asset/v1/folder/{id}/tokens/delete.json | Asset mit Lese- und Schreibzugriff |
| Token nach Ordner-ID abrufen | Token | GET | /rest/asset/v1/folder/{id}/tokens.json | Schreibgeschütztes Asset |
| Funktionen hinzufügen | Benutzerverwaltung | POST | /userservice/management/v1/users/{userid}/roles/create.json | Auf Benutzer-Management-Api zugreifen |
| Eingeladenen Benutzer löschen | Benutzerverwaltung | POST | /userservice/management/v1/users/{userId}/invite/delete.json | Auf Benutzer-Management-Api zugreifen |
| Rollen löschen | Benutzerverwaltung | POST | /userservice/management/v1/users/{userid}/roles/delete.json | Auf Benutzer-Management-Api zugreifen |
| Benutzer löschen | Benutzerverwaltung | POST | /userservice/management/v1/users/{userId}/delete.json | Auf Benutzer-Management-Api zugreifen |
| Eingeladenen Benutzer nach ID abrufen | Benutzerverwaltung | GET | /userservice/management/v1/users/{userid}/invite.json | Auf Benutzer-Management-Api zugreifen |
| Rollen abrufen | Benutzerverwaltung | GET | /userservice/management/v1/users/roles.json | Auf Benutzer-Management-Api zugreifen |
| Abrufen von Rollen und Arbeitsbereichen nach ID | Benutzerverwaltung | GET | /userservice/management/v1/users/{userid}/roles.json | Auf Benutzer-Management-Api zugreifen |
| Abrufen von Benutzern | Benutzerverwaltung | GET | /userservice/management/v1/users/allusers.json | Auf Benutzer-Management-Api zugreifen |
| Benutzer nach ID abrufen | Benutzerverwaltung | GET | /userservice/management/v1/users/{userid}/user.json | Auf Benutzer-Management-Api zugreifen |
| Arbeitsbereiche abrufen | Benutzerverwaltung | GET | /userservice/management/v1/users/workspaces.json | Auf Benutzer-Management-Api zugreifen |
| Benutzer einladen | Benutzerverwaltung | POST | /userservice/management/v1/users/invite.json | Auf Benutzer-Management-Api zugreifen |
| Benutzerattribute aktualisieren | Benutzerverwaltung | POST | /userservice/management/v1/users/{userId}/update.json | Auf Benutzer-Management-Api zugreifen |