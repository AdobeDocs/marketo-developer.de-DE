---
title: Marketo Mobile-Erweiterung für [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installieren und konfigurieren Sie die Marketo Mobile SDK-Erweiterung in Adobe Launch für iOS und Android, einschließlich der Einrichtung für Push-Benachrichtigungen und In-App-Nachrichten.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
TQID: https://experienceleague.adobe.com/Bk5GTnQjm6NDosl5Iw6TS-NRjH8owNRUKoE0mZ-H3pY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# Marketo Mobile-Erweiterung für [!DNL Adobe Launch]

Installieren Sie die Marketo Mobile SDK-Erweiterung in [!DNL Adobe Launch], um Push-Benachrichtigungen, In-App-Nachrichten oder beides zu senden.

## Voraussetzungen

- [Fügen Sie eine Anwendung in Marketo Admin hinzu](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) und rufen Sie den geheimen Anwendungsschlüssel und die Munchkin-ID ab.
- Folgen Sie den Installationsanweisungen im [!DNL Adobe Launch].
- Optional: [Einrichten von Push-Benachrichtigungen](push-notifications.md).

## iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu Datei > Neu > Datei und wählen Sie „Header-Datei“.
1. Nennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header“.
1. Gehen Sie zu Projekt > Target > Build-Phasen > Swift-Compiler > Codegenerierung.
1. Fügen Sie den folgenden Pfad zur Kopfzeile „Objective-Bridging“ hinzu:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Entfernen Sie für Swift die folgende Importanweisung, da in den vorherigen Schritten die Überbrückungskopfzeile hinzugefügt wurde.

`import Marketo/ALMarketo`

### iOS-Testgeräte

Befolgen Sie die Anweisungen unter [Hinzufügen von iOS-Testgeräten](installation.md#ios_test_devices).

### Verarbeiten eines benutzerdefinierten URL-Typs in AppDelegate

Befolgen Sie die [Anweisungen für benutzerdefinierte URL](installation.md#ios_test_devices).

### Einrichten von Push-Benachrichtigungen auf iOS

Befolgen Sie die [Push-Benachrichtigungsanweisungen](push-notifications.md). Verwenden Sie den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;.

## Android

### Konfigurieren von Berechtigungen

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen „INTERNET“ und „ACCESS_NETWORK_STATE“ anfordern. Überspringen Sie diesen Schritt, wenn die App sie bereits anfordert.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### ProGuard Konfiguration (optional)

Wenn Ihr Programm ProGuard verwendet, fügen Sie die folgenden Zeilen zur `proguard.cfg` Datei in Ihrem Projektordner hinzu. Diese Konfiguration schließt die Marketo SDK von der Verschleierung aus.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Android-Testgeräte

Befolgen Sie die Anweisungen unter [Android-Testgeräte](installation.md#android_test_devices).

## Einrichten von Push-Benachrichtigungen auf Android

Befolgen Sie die [Android Firebase Cloud Messaging-Anweisungen](installation.md#android_firebase_cloud_messaging_support). Verwenden Sie den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;.

Um Benutzerprofile einzurichten, befolgen Sie die [Anweisungen für Benutzerprofile](user-profiles.md). Um benutzerdefinierte Aktionen einzurichten, befolgen Sie die [Anweisungen für benutzerdefinierte Aktionen](custom-actions.md#android_custom_action). Verwenden Sie in beiden Anweisungen den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;.
