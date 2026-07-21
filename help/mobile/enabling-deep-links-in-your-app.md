---
title: Aktivieren von Deep-Links
feature: Mobile Marketing
description: Erfahren Sie, wie Sie Deep-Links in Ihrer App für Marketo-Push-Nachrichten mithilfe von benutzerdefinierten URI-Schemata aktivieren, einschließlich Anleitungen zu iOS, Android und PhoneGap und Best Practices.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 5%

---

# Aktivieren von Deep-Links

Deep-Linking leitet Personen zu bestimmten Inhalten in Ihrer App. Wenn eine Person beispielsweise eine mobile Push-Nachricht auswählt, mit der ein violettes T-Shirt beworben wird, kann die App den Inhalt des violetten T-Shirts anstelle der Startseite öffnen.

Der Prozess funktioniert wie folgt:

1. Ein Marketo-Benutzer platziert in der Tipp-Aktion für eine Push-Nachricht einen benutzerdefinierten URI.
1. Wenn eine Person auf ihrem Gerät auf die Push-Nachricht tippt, sendet Marketo MME SDK ein Trigger mit dem benutzerdefinierten URI.
1. Ihre App verarbeitet das Ereignis und leitet die Person zum entsprechenden Inhalt weiter.

So aktivieren Sie diesen Prozess:

1. Definieren Sie eine benutzerdefinierte URI-Struktur für Ihre App.
1. Registrieren Sie das Schema in Ihrem App-Manifest.
1. Fügen Sie Code hinzu, der Deep-Link-Ereignisse verarbeitet und Personen zum entsprechenden Inhalt weiterleitet.

Informationen zu iOS finden Sie in der Apple-Dokumentation unter [ eines benutzerdefinierten URL-Schemas für Ihre App](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Informationen zu Android finden Sie in der Google-Dokumentation unter [Aktivieren von Deep-Links für App-Inhalte](https://developer.android.com/training/app-links/deep-linking).

Verwenden Sie für PhoneGap-Apps ein Plug-in, damit Ihre Hybrid-App auf benutzerdefinierte URL-Schemata und universelle/App-Links in iOS und Android reagieren kann. Siehe die verfügbaren [Deep-Link-Plug-ins](https://cordova.apache.org/plugins/?q=deeplink).

Wenn Sie Deep-Linking in Ihrer App aktiviert haben, geben Sie Ihre benutzerdefinierten URIs für Ihre Marketo-Benutzenden frei, damit diese sie in die Tipp-Aktion für Push-Nachrichten einfügen können.

Marketo verwendet beim Einrichten von Testgeräten eine vordefinierte URI-Struktur. Weitere Informationen finden Sie unter „Testgeräte“ im [Installationshandbuch](installation.md).

## Best Practices für die Definition einer URI-Struktur

Wenn Ihre Marke über eine mobile Site verfügt, folgen Sie ihrer URL-Struktur, wenn Sie den Deep-Link-URI definieren. Wenn die Produkt-URL beispielsweise `https://myappname.com/products/purple-shirt` ist, verwenden Sie `myappname://products/purple-shirt` als entsprechenden Deep-Link-URI.

Verwenden Sie ein für Ihre Marke einzigartiges Schema. Obwohl keine Verordnung erfordert, dass Schemata global eindeutig sind, können Sie dazu beitragen, ein eindeutiges Schema zu erstellen, indem Sie Ihren Domain-Namen umkehren, z. B. `org.companyname`.
