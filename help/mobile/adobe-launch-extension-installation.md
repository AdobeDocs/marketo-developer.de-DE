---
title: '[!DNL Adobe Launch] Installation der Erweiterung'
feature: Mobile Marketing
description: '[!DNL Adobe Launch] Installationsübersicht für Erweiterungen'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# [!DNL Adobe Launch] Installation der Erweiterung

Installationsanweisungen für die Marketo-Erweiterung [!DNL Adobe Launch]. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Voraussetzungen

1. [Hinzufügen einer Anwendung in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Schlüssels für die Anwendung und der Munchkin-ID)
1. [Konfigurieren Sie die Eigenschaft in [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Konfigurieren des Geheimschlüssels der Anwendung und der Munchkin-ID für die Eigenschaft im Portal [!DNL Adobe Launch]
1. [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

## Installieren der Marketo-Erweiterung auf iOS

### Swift Bridging-Kopfzeile einrichten

1. Wechseln Sie zu [!UICONTROL Datei] > [!UICONTROL Neu] > [!UICONTROL Datei] und wählen Sie **[!UICONTROL Header-Datei]** aus.

1. Nennen Sie die Datei &quot;&lt;_Projektname_>-Bridging-Header&quot;.

1. Wechseln Sie zu [!UICONTROL Projekt] > [!UICONTROL Ziel] > [!UICONTROL Build-Einstellungen] > [!UICONTROL Swift-Compiler] > [!UICONTROL Codegenerierung]. Fügen Sie den folgenden Pfad zur Kopfzeile &quot;Objective-Bridging&quot;hinzu:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Initialisieren der Erweiterung

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` -Methode wie unten

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` -Methode wie unten

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## iOS-Testgeräte

1. Wählen Sie **[!UICONTROL Projekt]** > **[!UICONTROL Ziel]** > **[!UICONTROL Info]** > **[!UICONTROL URL-Typen]** aus.
1. Kennung hinzufügen: ${PRODUCT_NAME}
1. Festlegen von URL-Schemata: mkto-&lt;S_ecret Key_>
1. `application:openURL:sourceApplication:annotation:` in `AppDelegate.m file` einschließen (Objective-C)

### Umgang mit benutzerdefiniertem URL-Typ in AppDelegate

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

## Installieren des Marketo SDK in Android

### Einrichten der Android-Erweiterung

Befolgen Sie die Anweisungen in [!DNL Adobe Launch] Portal

### Berechtigungen konfigurieren

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen &quot;INTERNET&quot;und &quot;ACCESS_NETWORK_STATE&quot;anfordern. Wenn Ihre App diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Initialisieren der Erweiterung

ProGuard-Konfiguration (optional)

Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihrer `proguard.cfg` -Datei hinzu. Die Datei befindet sich im Ordner &quot;`project`&quot;. Durch Hinzufügen dieses Codes wird das Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Test  Geräte

Fügen Sie &quot;MarketoActivity&quot;zu `AndroidManifest.xml` im Anwendungs-Tag hinzu.

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

Das MME Software Development Kit (SDK) für Android wurde auf ein moderneres, stabileres und skalierbareres Framework aktualisiert, das mehr Flexibilität und neue Engineering-Funktionen für Ihren Android-App-Entwickler bietet.

Entwickler von Android-Apps können jetzt mit diesem SDK Googles [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) direkt verwenden.

### Hinzufügen von FCM zu Ihrer Anwendung

1. Integrieren Sie das neueste Marketo Android SDK in die Android App.  Schritte sind unter [GitHub](https://github.com/Marketo/android-sdk) verfügbar.
1. Konfigurieren Sie die Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts in der Firebase-Konsole [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/).
      1. Wählen Sie in der [Firebase-Konsole](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/) die Option **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte und dann **[!UICONTROL Firebase hinzufügen]** aus.
      1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option **[!UICONTROL Firebase zu Ihrer Android App hinzufügen]**.
      1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` -Datei für Ihre Firebase-App wird heruntergeladen.
      1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

   1. Navigieren Sie in [!UICONTROL Project Overview] zu **[!UICONTROL Projekteinstellungen]** .
      1. Klicken Sie auf die Registerkarte **[!UICONTROL Allgemein]**. Laden Sie die Datei `google-services.json` herunter.
      1. Klicken Sie auf die Registerkarte **[!UICONTROL Cloud Messaging]** . Kopieren Sie [!UICONTROL Server-Schlüssel] und [!UICONTROL Sender-ID]. Stellen Sie diese [!UICONTROL Server-Schlüssel] und die [!UICONTROL Sender-ID] für Marketo bereit.
   1. Konfigurieren von FCM-Änderungen in der Android App
      1. Wechseln Sie in Android Studio zur Projektansicht, um den Stammordner Ihres Projekts anzuzeigen.
         1. Verschieben Sie die heruntergeladene `google-services.json` -Datei in den Stammordner des Android-Anwendungsmoduls.
         1. Fügen Sie auf Projektebene `build.gradle` Folgendes hinzu:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Fügen Sie in &quot;build.gradle&quot;auf Anwendungsebene Folgendes hinzu:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            } 
            // Add to the bottom of the file 
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Klicken Sie abschließend in der in der ID angezeigten Leiste auf **[!UICONTROL Jetzt synchronisieren]** .
   1. Bearbeiten des Manifests Ihrer App Das FCM-SDK fügt automatisch alle erforderlichen Berechtigungen und die erforderlichen Empfängerfunktionen hinzu. Achten Sie darauf, die folgenden veralteten (und potenziell schädlichen) Elemente aus dem Manifest Ihrer App zu entfernen, da sie eine Nachrichtenduplizierung verursachen können:

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

**Q: Wo finde ich Anweisungen zum Aktualisieren auf die neueste Version des MME-SDK?** Anweisungen finden Sie auf der Marketo Developer Site [HIER](installation.md).

**F: Muss ich bei der Aktualisierung auf die neueste Version des SDK eine aktualisierte Version meiner Android-Anwendung für meine bestehenden Benutzer veröffentlichen?** Nein.

**F: Wie wirkt sich dies auf bestehende MME-Kunden aus, die Android-Apps veröffentlicht haben, die mit dem Marketo Android SDK integriert sind?** Sie können eine vorhandene GCM-Client-App in Android wie folgt zu Firebase Cloud Messaging (FCM) migrieren:

1. Wählen Sie in der [Firebase-Konsole](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/) die Option **[!UICONTROL Projekt hinzufügen]** aus.
1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte und dann **[!UICONTROL Firebase hinzufügen]** aus.
1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option **[!UICONTROL Firebase zu Ihrer Android App hinzufügen]**.
1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue Datei google-services.json für Ihre
1. Das Firebase-Programm wird heruntergeladen.
1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

**Q: Können wir die Leads, die mit dem alten Marketo SDK erstellt wurden, das die GCM App verwendet hat, als Ziel auswählen?** Ja. Alle Leads, die mit dem Marketo SDK erstellt wurden, können für den Versand der Push-Benachrichtigungen ausgewählt werden.
