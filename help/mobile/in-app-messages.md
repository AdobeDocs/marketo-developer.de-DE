---
title: In-App-Nachrichten
feature: Mobile Marketing
description: Überblick über In-App-Nachrichten
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 1%

---

# In-App-Nachrichten

Um die In-App-Messaging-Funktionen von Marketo nutzen zu können, müssen Sie die folgenden Schritte ausführen:

1. Installieren Sie das Marketo Mobile SDK wie in der [Mobilinstallation](installation.md) beschrieben.
1. Fügen Sie Ihre mobile App zu Marketo hinzu, wie in [Eine mobile App hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) beschrieben.
1. Fügen Sie optional Code zu Ihrer Mobile App hinzu, um [benutzerdefinierte Aktionen](custom-actions.md) zu erfassen.

Sobald Sie das Marketo Mobile SDK installiert und Ihre App in Marketo hinzugefügt haben, können Sie In-App-Nachrichten senden, die angezeigt werden, wenn ein Benutzer Ihre App öffnet.

Standardmäßig werden In-App-Nachrichten beim Öffnen der App ausgelöst. Wenn Sie In-App-Nachrichten für andere Ereignisse (z. B. wenn eine bestimmte Seite angezeigt wird oder eine bestimmte Schaltfläche gedrückt wird) Trigger haben möchten, müssen Sie dem Code benutzerdefinierte Aktionen hinzufügen. Codebeispiele dazu finden Sie im Abschnitt [Benutzerdefinierte Aktionen](custom-actions.md) .

## Fehlerbehebung

**In-App-Nachricht wird nicht angezeigt**

Marketo antwortet nur dann auf Trigger aus Apps, wenn das Marketo Mobile SDK mit der Marketo-Plattform initialisiert wurde. Der Initialisierungsprozess erfolgt beim erstmaligen Installieren und Öffnen der App. Da die Initialisierung nach dem ersten Öffnen der App erfolgt, wird das Ereignis &quot;App öffnen&quot;erst ausgelöst, nachdem die App ein zweites Mal geöffnet wurde. Schließen Sie die App und öffnen Sie sie erneut. Auf Ihrem Gerät sollte eine durch App-Öffnung ausgelöste Meldung angezeigt werden.

Benutzerdefinierte Ereignisse werden durch Benutzerinteraktionen ausgelöst, nachdem die App geöffnet wurde. Benutzerspezifische Ereignisse werden von Marketo während der ersten Sitzung erkannt.

**In-App-Tippen auf Aktivitäts-Tracking**

Stellen Sie sicher, dass Sie einer der primären oder sekundären Schaltflächen eine Aktion zuweisen, um Tippen-Aktivitäten zu verfolgen und basierend auf der Anzahl der Tippen die grundlegenden Anzeigefrequenzen zu verwenden.

Weitere Informationen finden Sie im Abschnitt [In-App-Nachrichten](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) in unserer Produktdokumentation.
