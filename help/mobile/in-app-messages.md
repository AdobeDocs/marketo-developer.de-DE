---
title: In-App-Nachrichten
feature: Mobile Marketing
description: Richten Sie In-App-Nachrichten von Marketo mit dem Mobile SDK ein, konfigurieren Sie benutzerdefinierte Ereignis-Trigger, verfolgen Sie die Tipp-Aktivität und beheben Sie die Initialisierungsprobleme beim ersten Öffnen der App.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 2%

---

# In-App-Nachrichten

Führen Sie die folgenden Schritte aus, um In-App-Nachrichten von Marketo zu verwenden:

1. Installieren Sie Marketo Mobile SDK wie unter [Mobile-Installation](installation.md) beschrieben.
1. Fügen Sie Ihre Mobile App zu Marketo hinzu, wie in [Hinzufügen einer Mobile App](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) beschrieben.
1. Optional: Fügen Sie Code zu Ihrer Mobile App hinzu, um ([ Aktionen) ](custom-actions.md) erfassen.

Nachdem Sie Marketo Mobile SDK installiert und Ihre App zu Marketo hinzugefügt haben, können Sie In-App-Nachrichten senden, die angezeigt werden, wenn ein Benutzer Ihre App öffnet.

Standardmäßig werden In-App-Nachrichten beim Öffnen der App ausgelöst. Um eine Nachricht für ein anderes Ereignis Trigger, z. B. eine bestimmte Seite anzuzeigen oder eine bestimmte Schaltfläche auszuwählen, fügen Sie dem Code eine benutzerdefinierte Aktion hinzu. Code[Beispiele finden Sie unter ](custom-actions.md)Benutzerdefinierte Aktionen“.

## Fehlerbehebung

**In-App-Nachricht wird nicht angezeigt**

Marketo reagiert erst auf App-Trigger, nachdem Marketo Mobile SDK mit der Marketo-Plattform initialisiert wurde. Die Initialisierung erfolgt, wenn Sie die App zum ersten Mal installieren und öffnen.

Da die Initialisierung nach dem ersten Öffnen der App erfolgt, wird das Ereignis „App Open“ erst ausgelöst, wenn die App ein zweites Mal geöffnet wird. Schließen Sie die App und öffnen Sie sie erneut. Eine Nachricht, die durch Öffnen der App ausgelöst wird, sollte dann auf Ihrem Gerät angezeigt werden.

Benutzerspezifische Ereignisse werden durch Benutzerinteraktion ausgelöst, nachdem die App geöffnet wurde. Benutzerspezifische Ereignisse werden von Marketo während der ersten Sitzung erkannt.

**In-App-Tracking von Tipp-Aktivitäten**

Um Tipp-Aktivitäten zu verfolgen und die Anzeigefrequenz auf der Anzahl der Tipper zu basieren, weisen Sie einer primären oder sekundären Schaltfläche eine andere Aktion als „Schließen“ zu.

Weitere Informationen finden Sie unter [In-App-Nachrichten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) in der Produktdokumentation.
