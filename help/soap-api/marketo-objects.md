---
title: "Marketo-Objekte"
feature: SOAP
description: "Marketo-Objekte - Übersicht"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---


# Marketo-Objekte

Marketo verwendet Marketo-Objekte (MObjects) zur Darstellung verschiedener Klassen wie Programm, Chancen und OpportunityPersonRole.

## Struktur von Objekten

Bei MObjects kann es sich um einen von drei Typen handeln: Standard, Benutzerdefiniert oder Virtual. Standard- und benutzerdefinierte MObjects stellen verschiedene Entitäten dar, wie z. B. Lead oder Unternehmen, während virtuelle Objekte wie LeadRecord aus Feldern eines oder mehrerer Objekte bestehen. Virtuelle Objekte sind praktische Objekte, die innerhalb der API verwendet werden, aber nicht in der Marketo-Anwendung vorhanden sind.

Objekte bestehen aus:

- Ein kleiner Satz fester Attribute, die für alle MObjects gelten
   - Erforderlicher Typ
   - Optionaler externalKey
   - schreibgeschützte ID, createdAt, updatedAt
- Eine Liste mit einem oder mehreren objektspezifischen Attributen, als Name/Wert-Paare, von denen einige möglicherweise erforderlich sind. Beispiel: Name für Chancen.
- Eine Liste der zugehörigen Objektverweise, als Objektname plus
   - Marketo ID oder
   - Externer Schlüssel als Attribut-Name/Attribut-Wert-Paar.

### ExternalKeys

Externe Schlüssel sind benutzerdefinierte Felder, die für Marketo-Objekte definiert sind, z. B. &quot;Lead&quot;oder &quot;Chancen&quot;. Der Name ist der Feldname und der Wert ist der Feldwert, der in einem externen System generiert wird. **Marketo erzwingt keine eindeutige Einschränkung für diese Werte.** Es liegt in der Verantwortung des API-Benutzers sicherzustellen, dass die Werte eindeutig sind. Sollte es zu einem Duplikat kommen, verwendet Marketo das zuletzt hinzugefügte Objekt. Dies ähnelt dem Verhalten für das Standardfeld E-Mail-Adresse .

### Verfügbare APIs

| API | Kann aktiviert werden |
|---|---|
| descriptionMObject | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
| getMObjects | Chancen, ChancenPersonRolle, Programm |
| syncMObjects | Chancen, ChancenPersonRolle, Programm |
| deleteMObjects | Chancen, ChancenPersonRolle |
| listMObjects | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
