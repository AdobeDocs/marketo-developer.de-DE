---
title: Aktivieren von Deep-Links
feature: Mobile Marketing
description: Erfahren Sie, wie Sie Deep-Links in Ihrer App für Marketo-Push-Nachrichten mithilfe von benutzerdefinierten URI-Schemata aktivieren, einschließlich Anleitungen zu iOS, Android und PhoneGap und Best Practices.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Aktivieren von Deep-Links

Deep-Linking ermöglicht es Ihnen, Personen zu bestimmten Inhalten (Ressourcen) in Ihrer App umzuleiten. Wenn eine Person beispielsweise auf eine mobile Push-Nachricht klickt, mit der ein lilafarbenes T-Shirt beworben wird, können Sie die App direkt öffnen, um den Inhalt des lilafarbenen T-Shirts (anstatt der Startseite) anzuzeigen.

Der Prozess funktioniert wie folgt:

1. Der Marketo-Benutzer platziert in der Tipp-Aktion einen benutzerdefinierten URI für seine Push-Nachricht.
1. Wenn eine Person auf ihrem Gerät auf die Push-Nachricht tippt, sendet Marketo MME SDK ein Trigger mit dem benutzerdefinierten URI.
1. Ihre App verarbeitet dann das Ereignis und leitet die Person zum richtigen Inhalt innerhalb Ihrer App weiter.

Dazu müssen Sie eine benutzerdefinierte URI-Struktur für Ihre App definieren, das Schema im Manifest Ihrer App registrieren und dann Code hinzufügen, um Deep-Link-Ereignisse zu verarbeiten und zum richtigen Speicherort in Ihrer App zu routen.

Informationen zu iOS finden Sie in der Apple-Dokumentation unter [ eines benutzerdefinierten URL-Schemas für Ihre App](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Informationen zu Android finden Sie in der Google-Dokumentation unter [Aktivieren von Deep-Links für App-Inhalte](https://developer.android.com/training/app-links/deep-linking).

Bei PhoneGap-Apps ist Deep-Linking nicht so einfach wie bei nativen iOS- oder Android-Apps. Es gibt jedoch Plug-ins, mit denen Ihre Hybrid-App auf benutzerdefinierte Deep-Link-URL-Schemata und universelle/App-Links sowohl in iOS als auch in Android reagieren kann. Betrachten Sie [diese Plug-ins](https://cordova.apache.org/plugins/?q=deeplink).

Wenn Sie Deep-Linking in Ihrer App aktiviert haben, geben Sie Ihre benutzerdefinierten URIs für Ihre Marketo-Benutzenden frei, damit diese sie in die Tipp-Aktion für Push-Nachrichten einfügen können.

Marketo verwendet beim Einrichten von Testgeräten eine vordefinierte URI-Struktur. Weitere Informationen finden Sie im Abschnitt „Testgeräte[ des ](installation.md).

## Best Practices für die Definition einer URI-Struktur

Wenn Ihre Marke über eine vorhandene mobile Site verfügt, empfiehlt es sich, auch für den Deep-Link-URI der URL-Struktur zu folgen. Wenn `https://myappname.com/products/purple-shirt` beispielsweise Ihre Website-Adresse für das betreffende Produkt ist, wäre `myappname://products/purple-shirt` eine gute Deep-Link-URI-Struktur, die in Ihrer App verwendet werden sollte.

Im Allgemeinen sollten Ihre Schemata eindeutig für Ihre Marke sein. Es gibt derzeit zwar keine Vorschriften, um Schemata weltweit einzigartig zu machen, aber eine Möglichkeit, um sicherzustellen, dass Ihre Schemata eindeutig sind, besteht darin, Ihren Domain-Namen umzukehren (z. B. `org.companyname`).
