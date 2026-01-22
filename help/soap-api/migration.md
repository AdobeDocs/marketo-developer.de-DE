---
title: Migration zur REST-API
feature: SOAP
description: Schrittweise Anleitung zur Migration von Marketo Engage von SOAP zu REST bis zum 31. Januar 2026 mit Endpunktzuordnungen, OAuth, Lead-Synchronisierungsmethoden und Referenzarchitekturen.
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 2%

---

# Migration zur REST-API

Die Marketo Engage SOAP-API wird nach dem 31. März 2026 eingestellt. Alle bestehenden Integrationen, die die SOAP-API verwenden, sollten bis zu diesem Datum eingestellt oder zur [Marketo Engage REST](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/rest-api)API migriert werden, um Service-Unterbrechungen zu vermeiden.

## Migration

Die SOAP-API unterstützt im Vergleich zur [REST-API](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/rest-api)I eine begrenzte Anzahl von Anwendungsfällen. Bei der Bestimmung, welche Endpunkte Sie Ihren Anwendungsfällen zuordnen sollen, sollten Sie die Best Practices für die [Marketo-Integration befolgen](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/marketo-integration-best-practices)

[Referenzarchitekturen](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/reference-architectures) sind für die Anwendungsfälle [CRM-Synchronisation](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=de) und [Data Warehouse-](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=de) verfügbar.

## Authentifizierung

[Authentifizierungsdokumentation](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/authentication)

Die Marketo REST-API verwendet OAuth 2.0-basierte Authentifizierung mit dem Grant-Typ „Client-Anmeldeinformationen“. Zugriffstoken sind eine Stunde nach der Erstellung gültig.

## Leads

[Lead-API-Dokumentation](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/leads)

Die SOAP-API unterstützt Lead-Datensynchronisation, [Munchkin-Cookie](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking)Verknüpfung und Lead-Zusammenführung. Wenn Ihre Anwendung die SyncLead-Methode von SOAP aufruft und den `marketoCookie` festlegt, können Sie wie folgt migrieren:

1. mithilfe der [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) REST-Methode, gefolgt von [Zugeordneter Lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. Sie können [Formular senden](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/leads) aufrufen. Dies erfordert jedoch die Konfiguration einiger Marketing-Assets und eine Interaktion mit der [Forms-API](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/assets/forms)

Anwendungen, die den `foreignSysPersonId` Schlüsseltyp verwenden, sollten mithilfe eines benutzerdefinierten Lead-Felds zur Darstellung dieser externen Kennung nach migrieren und entweder [Leads synchronisieren](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) oder [Massenimport von Leads](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) REST-Methoden verwenden.

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [getLead](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/getlead) | [Lead nach ID abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Leads nach Filtertyp abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [getMultipleLeads](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [Lead abrufen nach ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Leads abrufen nach Filtertyp](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [Leads abrufen nach Programm-ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [Leads abrufen nach Listen-ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [Massenexport von Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [mergeLeads](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/mergeleads) | [Zusammenführen von Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [syncLead](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/synclead) | [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Formular senden](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [Lead verknüpfen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [syncMultipleLeads](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [Leads synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Massenimport](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## Meine Objekte

M Objects war ein Sammelkonzept zur Unterstützung des Exports von Opportunity-Attributionsdaten für die externe Analyse und arbeitete mit drei Objekttypen: Opportunities, Opportunity-Rollen und Programme.

REST-Dokumentation:

- [Opportunity](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [Rollen](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [Programme](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/assets/programs)

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [deleteMObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [Opportunities löschen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [Opportunity-Rollen löschen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [describeMObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [Opportunity beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [Opportunity-Rolle beschreiben](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [getMObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [Opportunities erhalten](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [Opportunity-Rollen erhalten](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [listMObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | Nicht zutreffend |
| [syncMObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [Opportunities synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [Opportunity-Rollen synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [getChannels](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/programs/getchannels) | [Kanäle abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [getTags](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/programs/gettags) | [Tag-Typen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [Tag nach Namen abrufen](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Statische Listen

Anwendungsfälle für statische Listen in der SOAP-API sind auf die Aufnahme von Mitgliedschafts- und Lead-Daten sowie das Entfernen der Mitgliedschaft beschränkt, was mit den REST-Methoden [Zu Liste hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST), [Massenimport von Leads](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) oder [Aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) erreicht werden kann.

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [getImportToListStatus](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [Massenimport-Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [importToList](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [Zur Liste hinzufügen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) [Leads für den Massenimport](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [listOperation](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [Aus Liste entfernen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Aktivitäten

Die SOAP-API unterstützt nur das Abrufen von Aktivitäten.

REST-Dokumentation:

- [Synchrone Aktivitäten](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/activities)
- [Massenaktivität-Extrakt](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [getLeadActivity](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [Massenexport-Aktivitäten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [getLeadChanges](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [Massenexport-Aktivitäten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Kampagnen

REST-Dokumentation:

- [Intelligente Kampagnen](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

Die SOAP-API unterstützt nur drei Anwendungsfälle für intelligente Kampagnen: [Auslösen von Leads, um sich für eine anforderbare intelligente Kampagne zu qualifizieren](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger) Abrufen dieser anforderbaren Kampagnen und [Planen einer zukünftigen Ausführung einer intelligenten Kampagne](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [getCampaignForSource](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [Intelligente Kampagnen &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [requestCampaign](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [Kampagne anfordern](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [scheduleCampaign](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [Kampagne planen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Benutzerdefinierte Objekte

REST-Dokumentation:

- [Benutzerdefinierte Objekte](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

Die SOAP-API unterstützt nur CRUD-Vorgänge für benutzerdefinierte Objekte.

| SOAP-Methode | REST-Methode(n) |
| --- | --- |
| [deleteCustomObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [Benutzerdefinierte Objekte löschen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [getCustomObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [Benutzerdefinierte Objekte abrufen](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [syncCustomObjects](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [Benutzerdefinierte Objekte synchronisieren](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [Benutzerdefiniertes Massenimportobjekt importieren](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
