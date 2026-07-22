---
title: Marketo-Objekte
feature: Email Programs
description: Anleitung zur Verwendung von Marketo Velocity mit Leads, Opportunities und benutzerdefinierten Objekten, Ladefeldern, Zugriff auf die zehn wichtigsten Listen, SFDC-Beziehungen und $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
TQID: https://experienceleague.adobe.com/PvLJb-AOk6DKaNINycpzk5ojZiL8UNcanRg3vXmsGCI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 452
ht-degree: 1%

---

# Marketo-Objekte

Die Velocity-Implementierung von Marketo kann Daten aus diesen Marketo-Quellen verwenden:

- Leads
- Opportunitys
- Benutzerdefinierte Objekte
- App
- Mobile-App-Installation

## Felder werden geladen

Um ein Feld in einem Skript zu verwenden, wählen Sie das Feld unter der entsprechenden Liste im Skript-Token-Editor aus.

Wenn ein Skript auf ein Feld verweist, das nicht geladen wird, schlägt das Skript zur Laufzeit fehl. Ziehen Sie ein Feld aus dem Menü Feld in das Skript, um es zu laden, und fügen Sie einen Verweis am Cursor hinzu.

## Opportunity- und benutzerdefinierte Objektlisten

Für Opportunities und benutzerdefinierte Objekte lädt Marketo nur die 10 zuletzt aktualisierten Objekte jedes Typs. Sie können die Anzahl der verfügbaren benutzerdefinierten Objekte erhöhen, indem Sie die hier beschriebenen Schritte ausführen.

Marketo stellt die Objekte in einer Liste mit dem Namen `<objectName>List` bereit, sortiert vom zuletzt aktualisierten Datensatz zum zuletzt aktualisierten Datensatz. Um auf das Feld Betrag der zuletzt aktualisierten Opportunity zuzugreifen, verwenden Sie:

`${OpportunityList.get(0).Amount}`

Dieses Beispiel verweist auf das OpportunityList-Objekt, verwendet die get-Methode, um auf den Datensatz mit Index 0 zuzugreifen, und ruft die Amount-Eigenschaft aus diesem Datensatz ab.

Wenn Sie eine Opportunity oder ein benutzerdefiniertes Objektfeld in den Editor ziehen, ruft Marketo das Feld automatisch aus dem Datensatz bei Index 0 ab.

## Benutzerdefinierte SFDC-Objektbeziehungen

Um ein benutzerdefiniertes SFDC-Objekt zu verwenden, darf das Objekt nur eine einzige Beziehung zum Marketo-Lead haben. Objekte werden oft sowohl über den Kontakt als auch über das Konto verknüpft. Nur Objekte synchronisieren, bei denen die Lead/Kontakt-Beziehung aktiviert ist.

## Trigger-Objekte

Wenn eine Kampagne die Variable Zu Opportunity hinzugefügt, Opportunity aktualisiert oder `<Custom Object Name>` Trigger hinzugefügt verwendet, steht die Variable `$TriggerObject` für Skript-Token zur Verfügung, die in der Trigger-Kampagne ausgeführt werden. Diese Variable wird für den Trigger `<Custom Object Name>` ist aktualisiert nicht unterstützt.

Diese Variable verweist auf das Objekt, das die Kampagne ausgelöst hat. Es enthält dieselben Datensatzdaten, die verfügbar sind, wenn Sie über einen anderen Variablennamen auf das Objekt zugreifen.

Verwenden Sie in einer Batch-Kampagne kein Token, das auf `$TriggerObject` verweist. Das -Objekt ist in Batch-Kampagnen nicht verfügbar und der E-Mail-Versand schlägt fehl.

Wenn beispielsweise ein benutzerdefiniertes Objekt für eine Produktbestellung eine Kampagne Trigger, gibt die `$TriggerObject` die Reihenfolge an, zu der der Lead hinzugefügt wurde.

Das folgende Beispiel zeigt ein Skript für eine E-Mail zur Nachverfolgung von Bestellungen:

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

Die auslösende Aktion bestimmt das Objekt. Sie benötigen keinen zusätzlichen Code, um zu bestimmen, welches verfügbare Objekt die lokalen Daten enthält. Verwenden Sie `$TriggerObject`, wenn es verfügbar und angemessen ist, da es das zu referenzierende Objekt explizit identifiziert.

Hinweis: Wenn Sie `$TriggerObject` verwenden, wählen Sie die Felder des -Objekts im Bearbeitungsbereich aus, um sie für das Skript verfügbar zu machen.

Hinweis 2: `$TriggerObject` funktioniert nur für „Hinzugefügte“ Trigger, nicht für „Aktualisierte“ Trigger.
