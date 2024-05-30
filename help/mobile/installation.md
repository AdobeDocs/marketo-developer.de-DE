---
title: "Installation"
feature: "Mobile Marketing"
description: "Installieren von SDKs für Mobile Marketo"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---


# Installation

Installationsanweisungen für das Marketo Mobile SDK. Die folgenden Schritte sind erforderlich, um Push-Benachrichtigungen und/oder In-App-Nachrichten zu senden.

## Installieren des Marketo SDK in iOS

### Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Schlüssels für Ihre Anwendung und der Munchkin-ID)
1. [Push-Benachrichtigungen einrichten](push-notifications.md) (optional)

### Framework über CocoaPods installieren

1. Installieren Sie CocoaPods. `$ sudo gem install cocoapods`
1. Ändern Sie das Verzeichnis in Ihr Projektverzeichnis und erstellen Sie eine Podfile mit intelligenten Standardeinstellungen. `$ pod init`
1. Öffnen Sie Ihre Podfile. `$ open -a Xcode Podfile`
1. Fügen Sie Ihrer Podfile die folgende Zeile hinzu. `$ pod 'Marketo-iOS-SDK'`
1. Speichern und schließen Sie Ihre Podfile.
1. Herunterladen und Installieren des Marketo iOS SDK. `$ pod install`
1. Öffnen Sie Workspace in Xcode. `$ open App.xcworkspace`

### Installieren von Framework mithilfe von Swift Package Manager

1. Wählen Sie Ihr Projekt im Projektnavigator aus und klicken Sie unter &quot;Paketabhängigkeit hinzufügen&quot;auf &quot;+&quot;, wie unten dargestellt:

   ![Abhängigkeit hinzufügen](assets/dependency-manager-add.png)

1. Fügen Sie das Marketo-Paket aus diesem Repo hinzu. Fügen Sie diese URL für dieses Repository hinzu: https://github.com/Marketo/ios-sdk.

   ![Repo URL](assets/dependency-manager-url.png)

1. Fügen Sie nun das Ressourcen-Bundle wie folgt hinzu: Suchen Sie nach `MarketoFramework.XCframework` im Projektnavigator und öffnen Sie es in Finder. Drag &amp; Drop `MKTResources.bundle` zum Kopieren von Bundle-Ressourcen.

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu Datei > Neu > Datei und wählen Sie &quot;Header File&quot;.

   ![Wählen Sie &quot;Header File&quot;](assets/choose-header-file.png)

1. Benennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Gehen Sie zu Projekt > Ziel > Build-Phasen > Swift-Compiler > Codegenerierung. Fügen Sie den folgenden Pfad zur Objective-Bridging-Kopfzeile hinzu:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Build-Phasen](assets/build-phases.png)

## SDK initialisieren

Bevor Sie das Marketo iOS SDK verwenden können, müssen Sie es mit Ihrer Munchkin-Konto-ID und Ihrem App-Geheimnisschlüssel initialisieren. Sie finden diese im Marketo Admin-Bereich unter &quot;Apps und Geräte für Mobilgeräte&quot;.

1. Öffnen Sie die Datei AppDelegate.m (Objective-C) oder Bridging (Swift) und importieren Sie die Header-Datei Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Fügen Sie den folgenden Code in die `application:didFinishLaunchingWithOptions`: Funktion.

   Beachten Sie, dass &quot;nativ&quot;als Framework-Typ für native Apps übergeben werden muss.

>[!BEGINTABS]

>[!TAB Ziel C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Ersetzen `munkinAccountId` und `secretKey` oben unter Verwendung Ihrer &quot;Munchkin-Konto-ID&quot;und des &quot;Geheimschlüssels&quot;, die in der Marketo zu finden sind **[!UICONTROL Admin]** > **[!UICONTROL Mobile Apps und Geräte]** Abschnitt.

## iOS-Testgeräte

1. Wählen Sie Projekt > Ziel > Info > URL-Typen aus.
1. Kennung hinzufügen: ${PRODUCT_NAME}
1. Festlegen von URL-Schemata: `mkto-<Secret Key_>`
1. Anwendung einschließen:openURL:sourceApplication:annotation: in die Datei AppDelegate.m (Objective-C)

## Umgang mit benutzerdefiniertem URL-Typ in AppDelegate

>[!BEGINTABS]

>[!TAB Ziel C]

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{
   
    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];    
}
```

>[!TAB Swift]

```
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Installieren des Marketo SDK unter Android

### Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Schlüssels für Ihre Anwendung und der Munchkin-ID)
1. [Push-Benachrichtigungen einrichten](push-notifications.md#android_setup_push) (optional)
1. [Marketo SDK für Android herunterladen](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Android SDK-Einrichtung mit Gradle

1. Fügen Sie in der Datei &quot;build.gradle&quot;auf Anwendungsebene unter dem Abschnitt &quot;Abhängigkeiten&quot;hinzu

`implementation 'com.marketo:MarketoSDK:0.8.9'`

1. Der Stamm `build.gradle` sollte

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Projekt mit Gradle-Dateien synchronisieren

### Berechtigungen konfigurieren

Öffnen `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen &quot;INTERNET&quot;und &quot;ACCESS_NETWORK_STATE&quot;anfordern. Wenn Ihre App diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### SDK initialisieren

1. Öffnen Sie die Anwendungs- oder Aktivitätsklasse in Ihrer App und importieren Sie das Marketo SDK vor setContentView oder im Anwendungskontext in Ihre Aktivität.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. ProGuard-Konfiguration (optional)

   Wenn Sie ProGuard für Ihre App verwenden, fügen Sie die folgenden Zeilen in Ihre `proguard.cfg` -Datei. Die Datei befindet sich in Ihrem Projektordner. Durch Hinzufügen dieses Codes wird das Marketo SDK aus dem Verschleierungsprozess ausgeschlossen.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Android-Testgeräte

Hinzufügen von &quot;MarketoActivity&quot;zu `AndroidManifest.xml` Datei innerhalb des Anwendungs-Tags.

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

Das MME Software Development Kit (SDK) für Android wurde auf ein moderneres, stabileres und skalierbareres Framework aktualisiert, das mehr Flexibilität und neue technische Funktionen für Ihren Android-App-Entwickler enthält.

Entwickler von Android-Apps können jetzt Google direkt verwenden [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) mit diesem SDK.

### Hinzufügen von FCM zu Ihrer Anwendung

1. Integrieren Sie das neueste Marketo Android SDK in die Android-App.  Schritte finden Sie unter [GitHub](https://github.com/Marketo/android-sdk).
1. Konfigurieren Sie die Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts auf [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Firebase-Konsole.
      1. Im [Firebase-Konsole](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)auswählen `Add Project`.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und wählen Sie `Add Firebase`.
      1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option `Add Firebase to your Android App`.
      1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie `Add App`. Eine neue `google-services.json` -Datei für Ihre Firebase-App heruntergeladen wurde.
      1. Auswählen `Continue` und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

   1. Navigieren Sie in der Projektübersicht zu &quot;Projekteinstellungen&quot;.
      1. Klicken Sie auf die Registerkarte &quot;Allgemein&quot;. Laden Sie die Datei &quot;google-services.json&quot;herunter.
      1. Klicken Sie auf die Registerkarte &quot;Cloud Messaging&quot;. Kopieren Sie &quot;Server Key&quot; und &quot;Sender ID&quot;. Geben Sie diese &quot;Serverschlüssel&quot;und &quot;Sender-ID&quot;an Marketo an.
   1. Konfigurieren von FCM-Änderungen in der Android-App
      1. Wechseln Sie in Android Studio zur Projektansicht, um den Stammordner Ihres Projekts anzuzeigen.
         1. Verschieben Sie die heruntergeladene Datei &quot;google-services.json&quot;in den Stammordner des Android-Anwendungsmoduls .
         1. Fügen Sie auf Projektebene build.gradle Folgendes hinzu:

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

         1. Klicken Sie abschließend in der Leiste, die in der ID angezeigt wird, auf &quot;Jetzt synchronisieren&quot;
   1. Bearbeiten des Manifests Ihrer App Das FCM-SDK fügt automatisch alle erforderlichen Berechtigungen und die erforderlichen Empfängerfunktionen hinzu. Achten Sie darauf, die folgenden veralteten (und potenziell schädlichen) Elemente aus dem Manifest Ihrer App zu entfernen, da sie eine Nachrichtenduplizierung verursachen können:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter> 
      </receiver>
      ```
