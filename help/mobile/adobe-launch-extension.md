---
title: Marketo Mobile-Erweiterung für [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installieren und konfigurieren Sie die Marketo Mobile SDK-Erweiterung in Adobe Launch für iOS und Android, einschließlich der Einrichtung für Push-Benachrichtigungen und In-App-Nachrichten.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Marketo Mobile-Erweiterung für [!DNL Adobe Launch]

Installationsanweisungen für die Marketo Mobile SDK-Erweiterung in [!DNL Adobe Launch]. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Voraussetzungen

- [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Anwendungsschlüssels und der Munchkin-ID)
- Befolgen Sie bei der Installation die Anweisungen im [!DNL Adobe Launch] Portal
- [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

## iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu Datei > Neu > Datei und wählen Sie „Header-Datei“.
1. Nennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header“.
1. Gehen Sie zu Projekt > Target > Build-Phasen > Swift-Compiler > Codegenerierung. Fügen Sie den folgenden Pfad zur Kopfzeile „Objective-Bridging“ hinzu:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Für Swift-Benutzer: Entfernen Sie die folgende Importanweisung, da die Überbrückungskopfzeile in den obigen Schritten hinzugefügt wird.

`import Marketo/ALMarketo`

### iOS-Testgeräte

Befolgen Sie die Anweisungen unter [Hinzufügen von iOS-Testgeräten](installation.md#ios_test_devices)

### Verarbeiten eines benutzerdefinierten URL-Typs in AppDelegate

Folgen Sie den Anweisungen [hier](installation.md#ios_test_devices)

### Einrichten von Push-Benachrichtigungen auf iOS

Befolgen Sie [hier](push-notifications.md) und verwenden Sie den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;

## Android

### Konfigurieren von Berechtigungen

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen „INTERNET“ und „ACCESS_NETWORK_STATE“ anfordern. Wenn Ihre Anwendung diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### ProGuard Konfiguration (optional)

Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihrer `proguard.cfg`-Datei hinzu. Die Datei befindet sich im Projektordner. Durch Hinzufügen dieses Codes wird die Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Android-Testgeräte

Folgen Sie den Anweisungen [hier](installation.md#android_test_devices)

## Einrichten von Push-Benachrichtigungen auf Android

Befolgen Sie [hier](installation.md#android_firebase_cloud_messaging_support) und verwenden Sie den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;

Befolgen Sie beim Einrichten von Benutzerprofilen [ Anweisungen ](user-profiles.md)hier) und bei benutzerdefinierten Aktionen die Anweisungen [hier](custom-actions.md#android_custom_action). Verwenden Sie in den folgenden Anweisungen den Klassennamen „ALMarketo“ anstelle von &quot;Marketo&quot;
