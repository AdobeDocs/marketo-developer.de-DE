---
title: Benutzerdefinierte Aktionen
feature: Mobile Marketing
description: Übersicht über benutzerdefinierte Aktionen
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Benutzerdefinierte Aktionen

Sie können Benutzerinteraktionen verfolgen, indem Sie benutzerdefinierte Aktionen senden. Wenn Ihre Mobile App zum Senden einer benutzerdefinierten Aktion in Marketo SDK aufruft, wird die benutzerdefinierte Aktion zunächst auf dem Gerät gespeichert. Marketo SDK prüft dann, ob eine ausreichende Internetverbindung besteht, bevor die benutzerdefinierte Aktion gesendet wird. Daher kann es zu einer Verzögerung zwischen dem Zeitpunkt kommen, zu dem die benutzerdefinierte Aktion gesendet wird, und dem Zeitpunkt, zu dem sie von Marketo empfangen wird.

Benutzerdefinierte Aktionen können als Trigger und Filter in Smart-Kampagnen verwendet werden. Weitere Informationen finden Sie unter [Aktivität von Mobile Apps](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Senden benutzerdefinierter Aktionen auf iOS

Benutzerdefinierte Aktion senden.

>[!BEGINTABS]

>[!TAB Ziel C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Senden einer benutzerdefinierten Aktion mit Metadaten.

>[!BEGINTABS]

>[!TAB Ziel C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Alle Aktionen sofort melden (alle gespeicherten Aktionen senden).

>[!BEGINTABS]

>[!TAB Ziel C]

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
sharedInstance.reportAll();
```

>[!ENDTABS]

## Senden benutzerdefinierter Aktionen auf Android

1. Benutzerdefinierte Aktion senden.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Senden einer benutzerdefinierten Aktion mit Metadaten.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Alle benutzerdefinierten Aktionen sofort melden (alle gespeicherten Aktionen senden).

   ```
   Marketo.reportAll();
   ```

## Fehlerbehebung bei benutzerdefinierten Aktionen

Das Einrichten benutzerdefinierter Aktionen für Mobilgeräte ist einfach, aber es gibt Einschränkungen hinsichtlich der Anzahl der Zeichen, die Sie von der mobilen SDK an Marketo senden können. Stellen Sie sicher, dass alle benutzerdefinierten Aktionen, die über Mobile SDK an Marketo zurückgemeldet werden, weniger als 20 Zeichen lang sind.

**Hinweis zu Anwendungsfällen für mehrere Benutzende auf einem gemeinsam genutzten Gerät:** Wenn sich ein(e) Benutzende(r) bei einer in Marketo SDK integrierten App anmeldet, erfolgt der erste Aufruf, um den Lead mit der App-Installation zu verknüpfen. Nach erfolgreichem Abschluss dieses Aufrufs können weitere Benutzeraktivitäten in der App im Aktivitätsprotokoll des Leads eingesehen werden. Hinweis: Da es sich um einen asynchronen Aufruf handelt, werden benutzerdefinierte Aktionen, die unmittelbar nach der Anmeldung protokolliert werden, möglicherweise mit dem Benutzer verknüpft, der zuvor angemeldet war, bis der zugehörige Aufruf erfolgreich ist.
