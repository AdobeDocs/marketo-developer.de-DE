---
title: Benutzerdefinierte Aktionen
feature: Mobile Marketing
description: Erfahren Sie, wie Sie benutzerdefinierte Aktionen mit der Marketo Mobile SDK für iOS und Android senden und melden, offline in die Warteschlange stellen, Trigger Smart-Kampagnen durchführen und die 20 Zeichen…
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
TQID: https://experienceleague.adobe.com/yZKzdm-dH0cYPGGKE-Z-4KcbhGIwyFl0Z9vEqcv1QXI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 2%

---

# Benutzerdefinierte Aktionen

Benutzerdefinierte Aktionen verfolgen Benutzerinteraktionen in Ihrer Mobile App. Wenn die App die Marketo SDK aufruft, um eine benutzerdefinierte Aktion zu senden, speichert die SDK die Aktion zuerst auf dem Gerät. Der SDK sendet die Aktion, nachdem er eine ausreichende Internetverbindung erkannt hat, sodass Marketo die Aktion möglicherweise verzögert erhält.

Benutzerdefinierte Aktionen können als Trigger und Filter in Smart-Kampagnen verwendet werden. Weitere Informationen finden Sie unter [Aktivität von Mobile Apps](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Senden benutzerdefinierter Aktionen auf iOS

Eine benutzerdefinierte Aktion senden.

>[!BEGINTABS]

>[!TAB Ziel C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```swift
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Senden einer benutzerdefinierten Aktion mit Metadaten.

>[!BEGINTABS]

>[!TAB Ziel C]

```objectivec
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```swift
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Alle gespeicherten Aktionen sofort melden.

>[!BEGINTABS]

>[!TAB Ziel C]

```objectivec
[sharedInstance reportAll];
```

>[!TAB Swift]

```swift
sharedInstance.reportAll();
```

>[!ENDTABS]

## Senden benutzerdefinierter Aktionen auf Android

1. Eine benutzerdefinierte Aktion senden.

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

1. Alle gespeicherten benutzerdefinierten Aktionen sofort melden.

   ```
   Marketo.reportAll();
   ```

## Fehlerbehebung bei benutzerdefinierten Aktionen

Benutzerdefinierte Aktionsnamen, die von der mobilen SDK an Marketo gesendet werden, müssen weniger als 20 Zeichen lang sein.

**Anwendungsfälle mit mehreren Benutzern auf einem gemeinsam genutzten Gerät:** Wenn sich ein Benutzer bei einer Mobile App anmeldet, die die Marketo SDK verwendet, wird der Lead beim ersten Aufruf mit der App-Installation verknüpft. Nachdem der Aufruf erfolgreich war, werden nachfolgende Benutzeraktivitäten im Aktivitätsprotokoll des Leads angezeigt.

Der Verknüpfungsaufruf ist asynchron. Benutzerdefinierte Aktionen, die unmittelbar nach der Anmeldung protokolliert werden, werden möglicherweise mit dem zuvor angemeldeten Benutzer verknüpft, bis der Aufruf erfolgreich ist.
