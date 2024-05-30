---
title: "Benutzerdefinierte Aktionen"
feature: "Mobile Marketing"
description: "Übersicht über benutzerdefinierte Aktionen"
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Benutzerdefinierte Aktionen

Sie können Benutzerinteraktionen verfolgen, indem Sie benutzerdefinierte Aktionen senden. Wenn Ihre Mobile App das Marketo SDK aufruft, um eine benutzerdefinierte Aktion zu senden, wird die benutzerdefinierte Aktion zunächst auf dem Gerät gespeichert. Das Marketo SDK prüft dann, ob eine ausreichende Internetverbindung besteht, bevor die benutzerdefinierte Aktion gesendet wird. Daher kann es zu einer Verzögerung zwischen dem Versand der benutzerdefinierten Aktion und dem Empfang durch Marketo kommen.

Benutzerdefinierte Aktionen können in Smart-Kampagnen als Trigger und Filter verwendet werden. Weitere Informationen finden Sie unter [Aktivität &quot;Mobile App&quot;](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Senden benutzerdefinierter Aktionen auf iOS

Senden Sie eine benutzerdefinierte Aktion.

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

Senden Sie eine benutzerdefinierte Aktion mit Metadaten.

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

Melden Sie alle Aktionen sofort (senden Sie alle gespeicherten Aktionen).

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

1. Senden Sie eine benutzerdefinierte Aktion.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Senden Sie eine benutzerdefinierte Aktion mit Metadaten.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Melden Sie alle benutzerdefinierten Aktionen sofort (senden Sie alle gespeicherten Aktionen).

   ```
   Marketo.reportAll();
   ```

## Fehlerbehebung bei benutzerdefinierten Aktionen

Die Einrichtung benutzerdefinierter Aktionen für Mobilgeräte ist unkompliziert. Es gibt jedoch Einschränkungen hinsichtlich der Anzahl der Zeichen, die Sie vom Mobile SDK an Marketo senden können. Stellen Sie sicher, dass alle benutzerdefinierten Aktionen, die über das mobile SDK an Marketo gemeldet werden, weniger als 20 Zeichen lang sind.

**Hinweis zu Anwendungsfällen für mehrere Benutzer auf einem gemeinsam genutzten Gerät:** Wenn sich ein Benutzer bei einer Mobile App anmeldet, die mit dem Marketo SDK integriert ist, wird der erste Aufruf durchgeführt, um den Lead mit der App-Installation zu verknüpfen. Nach erfolgreichem Abschluss dieses Aufrufs können weitere Benutzeraktivitäten in der App im Aktivitätsprotokoll des Leads angezeigt werden. Beachten Sie Folgendes: Dies ist ein asynchroner Aufruf, wenn unmittelbar nach der Anmeldung irgendwelche benutzerdefinierten Aktionen protokolliert werden, die möglicherweise mit dem Benutzer verknüpft werden, der zuvor angemeldet war, bis der verknüpfte Aufruf erfolgreich war.
