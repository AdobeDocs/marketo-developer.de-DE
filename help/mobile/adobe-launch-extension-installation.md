---
title: '''[!DNL Adobe Launch] Installation der Erweiterung'
feature: Mobile Marketing
description: '''[!DNL Adobe Launch] Übersicht über die Installation der Erweiterung'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 2b6fd22becb0eb692dbcf48686c23f7a878cd812
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# [!DNL Adobe Launch] Installation der Erweiterung

Installationsanweisungen für [!DNL Adobe Launch] Marketo-Erweiterung. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Schlüssels für Ihre Anwendung und der Munchkin-ID)
1. [Konfigurieren Sie die Eigenschaft in [!DNL Adobe Launch] portal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Konfigurieren Sie den geheimen Schlüssel der Anwendung und die Munchkin-ID für die Eigenschaft im [!DNL Adobe Launch] portal
1. [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

## Installieren der Marketo-Erweiterung auf iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu Datei > Neu > Datei und wählen Sie &quot;Header File&quot;.

1. Benennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Navigieren Sie zu Projekt > Ziel > Build-Einstellungen > Swift-Compiler > Codegenerierung. Fügen Sie den folgenden Pfad zur Kopfzeile &quot;Objective-Bridging&quot;hinzu:

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

1. Auswählen [!UICONTROL Projekt] > [!UICONTROL Target] > [!UICONTROL Info] > URL-Typen.
1. Kennung hinzufügen: ${PRODUCT_NAME}
1. Festlegen von URL-Schemata: mkto-&lt;s_ecret key_=&quot;&quot;>
1. Einschließen `application:openURL:sourceApplication:annotation:` nach `AppDelegate.m file` (Objective-C)

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

Befolgen Sie die Anweisungen unter [!DNL Adobe Launch] portal

### Berechtigungen konfigurieren

Öffnen `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen &quot;INTERNET&quot;und &quot;ACCESS_NETWORK_STATE&quot;anfordern. Wenn Ihre App diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Initialisieren der Erweiterung

ProGuard-Konfiguration (optional)

Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihre `proguard.cfg` -Datei. Die Datei befindet sich in der `project` Ordner. Durch Hinzufügen dieses Codes wird das Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android-Testgeräte

Hinzufügen von &quot;MarketoActivity&quot;zu `AndroidManifest.xml` im Anwendungs-Tag.

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

Entwickler von Android-Apps können jetzt Google direkt verwenden [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) mit diesem SDK.

### Hinzufügen von FCM zu Ihrer Anwendung

1. Integrieren Sie das neueste Marketo Android SDK in die Android App.  Schritte finden Sie unter [GitHub](https://github.com/Marketo/android-sdk).
1. Konfigurieren Sie die Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts auf [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Firebase-Konsole.
      1. Im [Firebase-Konsole](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)auswählen [!UICONTROL Projekt hinzufügen].
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und wählen Sie [!UICONTROL Firebase hinzufügen].
      1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option &quot;Firebase zu Ihrer Android-App hinzufügen&quot;.
      1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie [!UICONTROL App hinzufügen]. Eine neue `google-services.json` -Datei für Ihre Firebase-App heruntergeladen wurde.
      1. Auswählen [!UICONTROL Weiter] und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

   1. Navigieren Sie in der Projektübersicht zu &quot;Projekteinstellungen&quot;.
      1. Klicken Sie auf die Registerkarte &quot;Allgemein&quot;. Laden Sie die `google-services.json` -Datei.
      1. Klicken Sie auf die Registerkarte &quot;Cloud Messaging&quot;. Kopieren Sie &quot;Server Key&quot; &amp; &quot;Sender ID&quot;. Geben Sie diese &quot;Serverschlüssel&quot;und &quot;Sender-ID&quot;an Marketo an.
   1. Konfigurieren von FCM-Änderungen in der Android App
      1. Wechseln Sie in Android Studio zur Projektansicht, um den Stammordner Ihres Projekts anzuzeigen.
         1. Verschieben Sie die heruntergeladenen `google-services.json` Datei in das Stammverzeichnis des Android-Anwendungsmoduls
         1. Auf Projektebene `build.gradle` fügen Sie Folgendes hinzu:

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

         1. Klicken Sie abschließend auf &quot;[!UICONTROL Synchronisieren jetzt]&quot; in der Leiste, die in der ID angezeigt wird
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

**F: Wo finde ich Anweisungen zum Aktualisieren auf die neueste Version des MME-SDK?** Anweisungen finden Sie auf der Marketo Developer Site . [HIER](installation.md).

**F: Muss ich bei der Aktualisierung auf die neueste Version des SDK eine aktualisierte Version meiner Android-Anwendung für meine bestehenden Benutzer veröffentlichen?** Anzahl

**F: Wie wirkt sich dies auf bestehende MME-Kunden aus, die Android-Apps veröffentlicht haben, die mit dem Marketo Android SDK integriert sind?** Sie können eine vorhandene GCM-Client-App in Android wie folgt zu Firebase Cloud Messaging (FCM) migrieren:

1. Im [Firebase-Konsole](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)auswählen [!UICONTROL Projekt hinzufügen].
1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und wählen Sie **[!UICONTROL Firebase hinzufügen]**.
1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option **[!UICONTROL Hinzufügen von Firebase zu Ihrer Android App]**.
1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie **[!UICONTROL App hinzufügen]**. Eine neue Datei google-services.json für Ihre
1. Das Firebase-Programm wird heruntergeladen.
1. Auswählen **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

**F: Können wir die Leads, die mit dem alten Marketo SDK erstellt wurden, das die GCM App verwendet hat, als Ziel auswählen?** Ja. Alle Leads, die mit dem Marketo SDK erstellt wurden, können für den Versand der Push-Benachrichtigungen ausgewählt werden.
