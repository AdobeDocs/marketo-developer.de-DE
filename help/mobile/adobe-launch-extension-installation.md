---
title: Installation der [!DNL Adobe Launch]
feature: Mobile Marketing
description: '[!DNL Adobe Launch] - Installationsübersicht'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---

# Installation der [!DNL Adobe Launch]

Installationsanweisungen für [!DNL Adobe Launch] Marketo-Erweiterung. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Anwendungsschlüssels und der Munchkin-ID)
1. [Konfigurieren Sie die Eigenschaft in [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Konfigurieren Sie den geheimen Anwendungsschlüssel und die Munchkin-ID für die Eigenschaft im [!DNL Adobe Launch]
1. [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

## Installieren der Marketo-Erweiterung auf iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu [!UICONTROL Datei] > [!UICONTROL Neu] > [!UICONTROL Datei] und wählen Sie **[!UICONTROL Header-Datei]**.

1. Nennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header“.

1. Navigieren Sie [!UICONTROL Projekt] > [!UICONTROL Target] > [!UICONTROL Build-Einstellungen] > [!UICONTROL Swift-] > [!UICONTROL Code-Generierung]. Fügen Sie den folgenden Pfad zum „Objective-Bridging“-Header hinzu:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Erweiterung initialisieren

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` wie unten beschrieben

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` wie unten beschrieben

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## iOS-Testgeräte

1. Wählen Sie **[!UICONTROL Projekt]** > **[!UICONTROL Target]** > **[!UICONTROL Info]** > **[!UICONTROL URL-Typen]**.
1. Kennung hinzufügen: ${PRODUCT_NAME}
1. Festlegen von URL-Schemata: &lt;s_secret_key>
1. `application:openURL:sourceApplication:annotation:` in `AppDelegate.m file` einschließen (Objective-C)

### Verarbeiten eines benutzerdefinierten URL-Typs in AppDelegate

>[!BEGINTABS]

>[!TAB Ziel C]

```
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Installieren von Marketo SDK auf Android

### Einrichtung der Android-Erweiterung

Folgen Sie den Anweisungen [!DNL Adobe Launch] Portal

### Konfigurieren von Berechtigungen

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen „INTERNET“ und „ACCESS_NETWORK_STATE“ anfordern. Wenn Ihre Anwendung diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Erweiterung initialisieren

ProGuard Konfiguration (optional)

Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihrer `proguard.cfg`-Datei hinzu. Die Datei befindet sich in Ihrem `project`. Durch Hinzufügen dieses Codes wird die Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Test  Geräte

Fügen Sie „MarketoActivity“ zu `AndroidManifest.xml` im Anwendungs-Tag hinzu.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Firebase Cloud Messaging-Unterstützung

Das MME Software Development Kit (SDK) für Android wurde auf ein moderneres, stabileres und skalierbareres Framework aktualisiert, das mehr Flexibilität und neue Engineering-Funktionen für Ihre Android-App-Entwicklerinnen und -Entwickler enthält.

Entwickler von Android-Apps können jetzt Googles [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) direkt mit diesem SDK verwenden.

### Hinzufügen von FCM zu Ihrer Anwendung

1. Integrieren Sie die neueste Marketo Android SDK in die Android-App.  Die Schritte sind unter [GitHub“ ](https://github.com/Marketo/android-sdk).
1. Konfigurieren der Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts in [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase Console.
      1. Wählen Sie in [Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
      1. Wählen Sie im Firebase-Begrüßungsbildschirm die Option **[!UICONTROL Firebase zu Ihrer Android-App hinzufügen]**.
      1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` für Ihre Firebase-App wird heruntergeladen.
      1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

   1. Navigieren Sie **[!UICONTROL Projekteinstellungen]** in [!UICONTROL Projektübersicht]
      1. Klicken Sie auf **[!UICONTROL Allgemein]** Registerkarte. Laden Sie die Datei `google-services.json` herunter.
      1. Klicken Sie auf **[!UICONTROL Registerkarte]** Cloud Messaging“. Kopieren [!UICONTROL Server-Schlüssel] und [!UICONTROL Absender-ID]. Stellen Sie diese [!UICONTROL Serverschlüssel] und [!UICONTROL Absender-ID] für Marketo bereit.
   1. Konfigurieren von FCM-Änderungen in der Android-App
      1. Wechseln Sie zur Projektansicht in Android Studio, um den Projektstammordner anzuzeigen
         1. Verschieben Sie die heruntergeladene `google-services.json` in das Stammverzeichnis des Android-App-Moduls.
         1. Fügen Sie in `build.gradle` auf Projektebene Folgendes hinzu:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Fügen Sie in build.gradle auf App-Ebene Folgendes hinzu:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            }
            // Add to the bottom of the file
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Klicken Sie abschließend in der Leiste **[!UICONTROL die in der ID angezeigt wird,]** auf „Jetzt synchronisieren“
   1. Manifest der App bearbeiten Die FCM-SDK fügt automatisch alle erforderlichen Berechtigungen und die erforderliche Empfängerfunktion hinzu. Entfernen Sie die folgenden veralteten (und potenziell schädlichen, da sie zu einer Duplizierung von Nachrichten führen können) Elemente aus dem Manifest Ihrer App:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```

### Häufig gestellte Fragen zu FCM

Häufig gestellte Fragen zur Unterstützung von Firebase Cloud Messaging.

**F: Wo finde ich Anleitungen zum Aktualisieren auf die neueste Version des MME SDK?** Anweisungen finden Sie auf der Marketo Developer Site [HIER](installation.md).

**F: Ist es bei der Aktualisierung auf die neueste Version von SDK erforderlich, dass ich eine aktualisierte Version meiner Android-Anwendung für meine bestehenden Anwender veröffentliche?**

**F: Wie wirkt sich dies auf die bestehenden MME-Kunden aus, die Android-Apps veröffentlicht haben, die in Marketo Android SDK integriert sind?** Sie können eine bestehende GCM-Client-App in Android wie folgt zu Firebase Cloud Messaging (FCM) migrieren:

1. Wählen Sie in [Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
1. Wählen Sie im Firebase-Begrüßungsbildschirm die Option **[!UICONTROL Firebase zu Ihrer Android-App hinzufügen]**.
1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue Datei „google-services.json“ für Ihre
1. Die Firebase App wird heruntergeladen.
1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

**F: Können wir die Leads auswählen, die mit dem alten Marketo SDK erstellt wurden, das die GCM-App verwendet?** Ja. Alle mit Marketo SDK erstellten Leads können für den Versand der Push-Benachrichtigungen ausgewählt werden.
