---
title: In-App-Nachrichten
feature: Mobile Marketing
description: Richten Sie In-App-Nachrichten von Marketo mit dem Mobile SDK ein, konfigurieren Sie benutzerdefinierte Ereignis-Trigger, verfolgen Sie die Tipp-Aktivität und beheben Sie die Initialisierungsprobleme beim ersten Öffnen der App.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 357
ht-degree: 1%

---

# In-App-Nachrichten

Um die In-App-Messaging-Funktionen von Marketo zu verwenden, müssen Sie die folgenden Schritte ausführen:

1. Installieren Sie Marketo Mobile SDK wie unter [Mobile-Installation](installation.md) beschrieben.
1. Fügen Sie Ihre Mobile App zu Marketo hinzu, wie in [Hinzufügen einer Mobile App](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) beschrieben.
1. Fügen Sie optional Code zu Ihrer Mobile App hinzu, um ([&#x200B; Aktionen) &#x200B;](custom-actions.md) erfassen.

Nachdem Sie Marketo Mobile SDK installiert und das Hinzufügen Ihrer App in Marketo abgeschlossen haben, können Sie In-App-Nachrichten senden, die angezeigt werden, wenn ein Benutzer Ihre App öffnet.

Standardmäßig werden In-App-Nachrichten beim Öffnen der App ausgelöst. Wenn Sie Trigger für In-App-Nachrichten für andere Ereignisse erstellen möchten, z. B. wenn eine bestimmte Seite aufgerufen oder eine bestimmte Schaltfläche gedrückt wird, müssen Sie dem Code benutzerdefinierte Aktionen hinzufügen. Code[Beispiele hierfür finden Sie &#x200B;](custom-actions.md) Abschnitt „Benutzerdefinierte Aktionen“.

## Fehlerbehebung

**In-App-Nachricht wird nicht angezeigt**

Marketo reagiert erst auf Trigger aus Apps, nachdem Marketo Mobile SDK mit der Marketo-Plattform initialisiert wurde. Der Initialisierungsprozess wird ausgeführt, wenn Sie die App zum ersten Mal installieren und öffnen. Da die Initialisierung nach dem ersten Öffnen der App erfolgt, wird das Ereignis „App Open“ erst ausgelöst, wenn die App ein zweites Mal geöffnet wird. Schließen Sie die App und öffnen Sie sie erneut. Daraufhin sollte auf Ihrem Gerät eine Meldung angezeigt werden, die durch die App Open ausgelöst wird.

Benutzerspezifische Ereignisse werden durch Benutzerinteraktion ausgelöst, nachdem die App geöffnet wurde. Benutzerspezifische Ereignisse werden von Marketo während der ersten Sitzung erkannt.

**In-App-Tracking von Tipp-Aktivitäten**

Stellen Sie sicher, dass Sie neben „Schließen“ eine Aktion zu einer der primären oder sekundären Schaltflächen zuweisen, um Tipp-Aktivitäten zu verfolgen und basierend auf der Anzahl der Tippen die grundlegenden Anzeigefrequenzen zu verwenden.

Weitere Informationen finden Sie [&#x200B; Abschnitt „In-App](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message)Nachrichten“ in unserer Produktdokumentation.
