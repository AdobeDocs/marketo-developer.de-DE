---
title: Aktivieren von Deep-Links
feature: Mobile Marketing
description: Anweisungen zum Aktivieren von Deep-Links
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# Aktivieren von Deep-Links

Über Deep-Linking können Sie Personen zu bestimmten Inhalten (Ressourcen) innerhalb Ihrer App weiterleiten. Wenn beispielsweise eine Person auf eine Push-Nachricht für Mobilgeräte klickt, für die ein violettes T-Shirt angezeigt wird, können Sie die App direkt für den lilafarbenen T-Shirt-Inhalt öffnen (und nicht für die Startseite).

Der Prozess funktioniert wie folgt:

1. Der Marketo-Benutzer legt einen benutzerdefinierten URI in der Tippen-Aktion für seine Push-Nachricht ab.
1. Wenn eine Person auf ihr Gerät auf die Push-Nachricht tippt, wird vom Marketo MME SDK ein Ereignis mit dem benutzerdefinierten URI Trigger.
1. Ihre App verarbeitet dann das Ereignis und leitet die Person zum entsprechenden Inhalt in Ihrer App weiter.

Dazu müssen Sie eine benutzerdefinierte URI-Struktur für Ihre App definieren, das Schema im Manifest Ihrer App registrieren und dann Code hinzufügen, um Deep-Link-Ereignisse zu verarbeiten und zum richtigen Speicherort in Ihrer App weiterzuleiten.

Informationen zu iOS finden Sie in der Apple-Dokumentation unter [Definieren eines benutzerdefinierten URL-Schemas für Ihre App](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Informationen zu Android finden Sie in der Google-Dokumentation unter [Aktivieren von Deep-Links für App-Inhalte](https://developer.android.com/training/app-links/deep-linking).

Bei PhoneGap-Apps ist die Deep-Linking-Funktion nicht so einfach wie bei nativen iOS- oder Android-Apps. Es gibt jedoch Plugins, mit denen Ihre Hybrid-App auf benutzerdefinierte Deep-Link-URL-Schemata und universelle/App-Links in iOS und Android reagieren kann. Beachten Sie [diese Plug-ins](https://cordova.apache.org/plugins/?q=deeplink).

Wenn Sie Deep-Linking in Ihrer App aktiviert haben, geben Sie Ihre benutzerdefinierten URIs für Ihre Marketo-Benutzer frei, damit sie sie in die Tippen-Aktion für Push-Nachrichten einfügen können.

Marketo verwendet beim Einrichten von Testgeräten eine vordefinierte URI-Struktur. Weitere Informationen finden Sie im Abschnitt &quot;Testgeräte&quot;des [Installationshandbuchs](installation.md).

## Best Practices zum Definieren einer URI-Struktur

Wenn Ihre Marke über eine vorhandene mobile Site verfügt, empfiehlt es sich, auch für den Deep-Link-URI die URL-Struktur zu befolgen. Wenn beispielsweise `https://myappname.com/products/purple-shirt` Ihre Website-Adresse für das betreffende Produkt ist, wäre `myappname://products/purple-shirt` eine gute Deep-Link-URI-Struktur, die in Ihrer App verwendet werden kann.

Im Allgemeinen sollten Ihre Schemas für Ihre Marke eindeutig sein. Es gibt zwar derzeit keine Vorschriften, um Schemas weltweit eindeutig zu machen, aber eine Möglichkeit, um sicherzustellen, dass Ihre Schemas eindeutig sind, besteht darin, Ihren Domänennamen umzukehren (z. B. `org.companyname`).
