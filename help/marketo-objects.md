---
title: Marketo-Objekte
feature: Email Programs
description: Anleitung zur Verwendung von Marketo Velocity mit Leads, Opportunities und benutzerdefinierten Objekten, Ladefeldern, Zugriff auf die zehn wichtigsten Listen, SFDC-Beziehungen und $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Marketo-Objekte

Die Velocity-Implementierung von Marketo kann Daten aus verschiedenen Quellen in Marketo nutzen: Leads, Opportunities, benutzerdefinierte Objekte, Mobile App, Mobile-App-Installation.

## Felder werden geladen

Um ein Feld zur Verwendung in einem Skript zu laden, muss dieses Feld unter der entsprechenden Liste im Skript-Token-Editor aktiviert werden.

Wenn Sie ein Feld nicht laden und es im Skript referenziert wird, schlägt die Skriptausführung zur Laufzeit fehl. Sie können Felder aus dem Menü Feld per Drag-and-Drop in das Skript ziehen. Dadurch können sie geladen werden, und es wird ein Verweis auf das Feld am Cursor hinzugefügt.

## Opportunity- und benutzerdefinierte Objektlisten

Beim Abrufen von Opportunities oder benutzerdefinierten Objekten werden nur die 10 zuletzt aktualisierten Objekte eines Typs geladen. Die Anzahl der verfügbaren benutzerdefinierten Objekte kann durch Befolgen der hier beschriebenen Schritte erhöht werden. Diese werden als Liste mit dem Namen `<objectName>List` angegeben und sind aus dem zuletzt aktualisierten Datensatz sortiert. Um auf das Feld Betrag in der Opportunity zuzugreifen, die zuletzt aktualisiert wurde, würden Sie also Folgendes verwenden:

`${OpportunityList.get(0).Amount}`

In diesem Beispiel verweisen Sie auf das OpportunityList-Objekt, verwenden die get-Methode, um auf den mit 0 indizierten Datensatz zuzugreifen, und rufen dann die Amount-Eigenschaft aus dem zurückgegebenen Objekt ab. Wenn Sie ein Feld aus einer Opportunity oder einem benutzerdefinierten Objekt in den Editor ziehen, wird das Feld automatisch aus dem mit 0 indizierten Datensatz abgerufen.

## Benutzerdefinierte SFDC-Objektbeziehungen

Um zur Verwendung verfügbar zu sein, darf ein benutzerdefiniertes SFDC-Objekt nur eine einzige Beziehung zum Marketo-Lead haben. Objekte sind häufig sowohl über den Kontakt als auch das Konto verknüpft. Daher ist es wichtig, nur Objekte mit Marketo zu synchronisieren, für die die Lead/Kontakt-Beziehung aktiviert ist.

## Trigger-Objekte

Wenn eine Kampagne über die Option Zu Opportunity hinzugefügt, Opportunity aktualisiert oder `<Custom Object Name>` Triggern hinzugefügt wird, wird eine spezielle Variable in den Skript-Token verfügbar, die im Kontext der Trigger-Kampagne ausgeführt werden: `$TriggerObject `(wird für `<Custom Object Name>` Trigger nicht unterstützt).  Wenn in einer Batch-Kampagne ein Token mit einer `$TriggerObject` verwendet wird, schlägt der E-Mail-Versand fehl, da dieses Objekt in Batch-Kampagnen aller Art nicht verfügbar ist.  Dies ist ein Verweis auf das Objekt, das die Kampagne ausgelöst hat. Das -Objekt enthält alle Daten, die der Datensatz enthält, wenn über einen anderen Variablennamen auf ihn zugegriffen wird.

Wenn beispielsweise eine Kampagne über ein benutzerdefiniertes Objekt für eine Produktbestellung ausgelöst wurde, wird die Reihenfolge, zu der der Lead hinzugefügt wurde, in der `$TriggerObject`-Variablen angezeigt.

Hier ist ein Beispielskript für eine E-Mail zur Nachverfolgung von Bestellungen:

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

Der Vorteil der Verwendung der `$TriggerObject`-Variablen besteht darin, dass Sie keinen Code dedizieren müssen, um zu bestimmen, aus welchen der verfügbaren Objekte Sie Ihre lokalen Daten abrufen möchten.  Das Objekt wird durch die Auslöseaktion bestimmt. Dies ist die explizitste Methode zur Auswahl eines Objekts, auf das verwiesen werden soll, und sollte verwendet werden, wann immer verfügbar und angemessen.

Hinweis: Bei Verwendung der `$TriggerObject` müssen Felder im Bearbeitungsbereich markiert werden, damit das Objekt dem Skript zur Verfügung gestellt werden kann.

Hinweis 2: Die `$TriggerObject` funktioniert nur für „hinzugefügte“ Trigger und nicht für „aktualisierte“ Trigger.
