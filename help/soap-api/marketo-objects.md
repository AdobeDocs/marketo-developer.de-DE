---
title: Marketo-Objekte
feature: SOAP
description: Übersicht über Marketo-Objekte
exl-id: 99b9aed4-94e8-46e8-84d9-2cc5215b0c13
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Marketo-Objekte

Marketo verwendet Marketo-Objekte (MObjects) zur Darstellung verschiedener Klassen wie Program, Opportunity und OpportunityPersonRole.

## Struktur von MObjects

MO-Objekte können einen von drei Typen aufweisen: Standard, Benutzerdefiniert oder Virtuell. Standard- und benutzerdefinierte MO-Objekte stellen verschiedene Entitäten dar, z. B. Lead oder Unternehmen, während virtuelle Objekte wie LeadRecord aus Feldern aus einem oder mehreren Objekten bestehen. Virtuelle Objekte sind Komfortobjekte, die in der -API verwendet werden, aber nicht in der Marketo-Anwendung vorhanden sind.

MO-Objekte bestehen aus:

- Ein kleiner Satz fester Attribute, die allen MObjects gemeinsam sind
   - Erforderlicher Typ
   - Optionaler externer Schlüssel
   - Schreibgeschützte ID, createdAt, updatedAt
- Eine Liste eines oder mehrerer objektspezifischer Attribute, z. B. Name/Wert-Paare, von denen einige erforderlich sein können. Beispiel: Name auf Opportunity.
- Eine Liste der zugehörigen Objektverweise, wie z. B. Objektname plus
   - Marketo-ID oder
   - Externer Schlüssel als Attribut-Name/Attribut-Wert-Paar.

### external-keys

Externe Schlüssel sind benutzerdefinierte Felder, die für Marketo-Objekte definiert werden, wie Lead oder Opportunity. Der Name ist der Feldname und der Wert ist der Feldwert, der in einem externen System generiert wird. **Marketo erzwingt keine Eindeutigkeitsbeschränkung für diese Werte.** Der API-Benutzer muss sicherstellen, dass die Werte eindeutig sind. Sollte ein Duplikat auftreten, verwendet Marketo das zuletzt hinzugefügte Objekt . Dies ähnelt dem Verhalten für das Standardfeld E-Mail-Adresse .

### Verfügbare APIs

| API | Kann ausgeführt werden am |
|---|---|
| describeMObject | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
| getMObjects | Opportunity, OpportunityPersonRole, Programm |
| syncMObjects | Opportunity, OpportunityPersonRole, Programm |
| deleteMObjects | Gelegenheit, OpportunityPersonRole |
| listMObjects | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
