---
title: Marketo Mobile-Erweiterung für  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Übersicht über die Marketo Mobile-Erweiterung für  [!DNL Adobe Launch] für
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---

# Marketo Mobile-Erweiterung für [!DNL Adobe Launch]

Installationsanweisungen für die Marketo Mobile SDK-Erweiterung in [!DNL Adobe Launch]. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Voraussetzungen

- [Hinzufügen einer Anwendung in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Schlüssels für die Anwendung und der Munchkin-ID)
- Befolgen Sie die Anweisungen, die im Portal [!DNL Adobe Launch] für die Installation bereitgestellt werden.
- [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

## iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu Datei > Neu > Datei und wählen Sie &quot;Header File&quot;.
1. Nennen Sie die Datei &quot;&lt;_Projektname_>-Bridging-Header&quot;.
1. Gehen Sie zu Projekt > Ziel > Build-Phasen > Swift-Compiler > Codegenerierung. Fügen Sie den folgenden Pfad zur Objective-Bridging-Kopfzeile hinzu:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Für Swift-Benutzer: Entfernen Sie die folgende Importanweisung, da die Überbrückungskopfzeile in den oben genannten Schritten hinzugefügt wird.

`import Marketo/ALMarketo`

### iOS-Testgeräte

Befolgen Sie die Anweisungen unter [Hinzufügen von iOS-Testgeräten](installation.md#ios_test_devices)

### Umgang mit benutzerdefiniertem URL-Typ in AppDelegate

Befolgen Sie die Anweisungen [hier](installation.md#ios_test_devices)

### Push-Benachrichtigungen in iOS einrichten

Befolgen Sie die Anweisungen [hier](push-notifications.md) und verwenden Sie den Klassennamen &quot;ALMarketo&quot;anstelle von &quot;Marketo&quot;.

## Android

### Berechtigungen konfigurieren

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen &quot;INTERNET&quot;und &quot;ACCESS_NETWORK_STATE&quot;anfordern. Wenn Ihre App diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### ProGuard-Konfiguration (optional)

Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihrer `proguard.cfg` -Datei hinzu. Die Datei befindet sich in Ihrem Projektordner. Durch Hinzufügen dieses Codes wird das Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Android-Testgeräte

Befolgen Sie die Anweisungen [hier](installation.md#android_test_devices)

## Push-Benachrichtigungen in Android einrichten

Befolgen Sie die Anweisungen [hier](installation.md#android_firebase_cloud_messaging_support) und verwenden Sie den Klassennamen &quot;ALMarketo&quot;anstelle von &quot;Marketo&quot;.

Befolgen Sie zum Einrichten von Benutzerprofilen die Anweisungen [hier](user-profiles.md) und für benutzerdefinierte Aktionen die Anweisungen [hier](custom-actions.md#android_custom_action). Verwenden Sie in den folgenden Anweisungen den Klassennamen &quot;ALMarketo&quot;anstelle von &quot;Marketo&quot;.
