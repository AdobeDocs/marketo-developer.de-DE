---
title: Installation der [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installieren Sie die Adobe Launch Marketo-Erweiterung für Mobilgeräte. Befolgen Sie die Schritte zur Einrichtung von iOS und Android, Testgeräte, Berechtigungen und FCM für Push- und In-App-Nachrichten.
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
TQID: https://experienceleague.adobe.com/UZRHaRBISIZsE6E25Ee7CnnYwyZwi6w2YgOQJ-JL00U
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 753
ht-degree: 0%

---

# Installation der [!DNL Adobe Launch]

Installieren Sie die [!DNL Adobe Launch] Marketo-Erweiterung, um Push-Benachrichtigungen, In-App-Nachrichten oder beides zu senden.

## Voraussetzungen

1. [Fügen Sie eine Anwendung in Marketo Admin hinzu](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) und rufen Sie den geheimen Anwendungsschlüssel und die Munchkin-ID ab.
1. [Konfigurieren Sie die Eigenschaft im  [!DNL Adobe Launch] Portal](https://experience.adobe.com/#/@amc/data-collection/home).
1. Konfigurieren Sie den Geheimschlüssel der Anwendung und die Munchkin-ID für die Eigenschaft im [!DNL Adobe Launch].
1. Optional: [Einrichten von Push-Benachrichtigungen](push-notifications.md).

## Installieren der Marketo-Erweiterung auf iOS

### Swift Bridging-Kopfzeile einrichten

1. Gehen Sie zu [!UICONTROL Datei] > [!UICONTROL Neu] > [!UICONTROL Datei] und wählen Sie **[!UICONTROL Header-Datei]**.

1. Nennen Sie die Datei &quot;&lt;_ProjectName_>-Bridging-Header“.

1. Navigieren Sie [!UICONTROL Projekt] > [!UICONTROL Target] > [!UICONTROL Build-Einstellungen] > [!UICONTROL Swift-] > [!UICONTROL Code-Generierung].
1. Fügen Sie den folgenden Pfad zum „Objective-Bridging“-Header hinzu:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Erweiterung initialisieren

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` wie folgt.

```objectivec
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` wie folgt.

```objectivec
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## iOS-Testgeräte

1. Wählen Sie **[!UICONTROL Projekt]** > **[!UICONTROL Target]** > **[!UICONTROL Info]** > **[!UICONTROL URL-Typen]**.
1. Kennung ${PRODUCT_NAME} hinzufügen.
1. Setzen Sie URL-Schemata auf mkto-&lt;s_secret_key>.
1. Fügen Sie für Objective-C `application:openURL:sourceApplication:annotation:` zu `AppDelegate.m file` hinzu.

### Verarbeiten eines benutzerdefinierten URL-Typs in AppDelegate

>[!BEGINTABS]

>[!TAB Ziel C]

```objectivec
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

```objectivec
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Installieren von Marketo SDK auf Android

### Einrichtung der Android-Erweiterung

Befolgen Sie die Anweisungen im [!DNL Adobe Launch].

### Konfigurieren von Berechtigungen

Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen „INTERNET“ und „ACCESS_NETWORK_STATE“ anfordern. Überspringen Sie diesen Schritt, wenn die App sie bereits anfordert.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Erweiterung initialisieren

ProGuard Konfiguration (optional)

Wenn Ihre App ProGuard verwendet, fügen Sie die folgenden Zeilen zur `proguard.cfg` Datei im `project` hinzu. Diese Konfiguration schließt die Marketo SDK von der Verschleierung aus.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android-Testgeräte

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

Die MME SDK für Android unterstützt die direkte Verwendung von Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM).

### Hinzufügen von FCM zu Ihrer Anwendung

1. Integrieren Sie die neueste Marketo Android SDK in die Android-App. Weitere Informationen finden Sie in den Schritten [GitHub](https://github.com/Marketo/android-sdk).
1. Konfigurieren Sie die Firebase-App in der Firebase Console.
   1. Erstellen oder Hinzufügen eines Projekts in der [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase Console.
      1. Wählen Sie in [Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
      1. Wählen Sie im Firebase-Begrüßungsbildschirm die Option **[!UICONTROL Firebase zu Ihrer Android-App hinzufügen]**.
      1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` für Ihre Firebase-App wird heruntergeladen.
      1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

   1. Navigieren Sie **[!UICONTROL Projekteinstellungen]** in [!UICONTROL Projektübersicht].
      1. Wählen Sie die **[!UICONTROL Allgemein]** und laden Sie `google-services.json` herunter.
      1. Wählen Sie die Registerkarte **[!UICONTROL Cloud Messaging]**. Kopieren Sie [!UICONTROL Serverschlüssel] und [!UICONTROL Absender-ID] und stellen Sie sie Marketo zur Verfügung.
   1. Konfigurieren von FCM in der Android-App.
      1. Wechseln Sie zur Projektansicht in Android Studio, um den Projektstammordner anzuzeigen.
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

         1. Wählen Sie **[!UICONTROL Jetzt synchronisieren]** in der Leiste aus, die in der IDE angezeigt wird.
   1. Bearbeiten Sie das App-Manifest. FCM SDK fügt automatisch die erforderlichen Berechtigungen und Empfängerfunktionen hinzu. Entfernen Sie die folgenden veralteten Elemente, die zu einer Duplizierung der Nachricht führen können:

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

Diese Fragen betreffen die Unterstützung von Firebase Cloud Messaging.

**F: Wo finde ich Anleitungen, um auf die neueste Version des MME SDK zu aktualisieren?** Weitere Informationen finden [&#x200B; in den &#x200B;](installation.md) auf der Marketo Developer-Site.

**F: Ist es bei der Aktualisierung auf die neueste Version von SDK erforderlich, dass ich eine aktualisierte Version meiner Android-Anwendung für meine bestehenden Anwender veröffentliche?** Nein.

**F: Wie wirkt sich dies auf bestehende MME-Kunden mit veröffentlichten Android-Apps aus, die Marketo Android SDK verwenden?** Migrieren Sie eine bestehende Android GCM-Client-App wie folgt zu Firebase Cloud Messaging (FCM):

1. Wählen Sie in [Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
1. Wählen Sie im Firebase-Begrüßungsbildschirm die Option **[!UICONTROL Firebase zu Ihrer Android-App hinzufügen]**.
1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue Datei „google-services.json“ für Ihre Firebase-App wird heruntergeladen.
1. Wählen Sie **[!UICONTROL Weiter]** und befolgen Sie die detaillierten Anweisungen zum Hinzufügen des Google Services-Plug-ins in Android Studio.

**F: Können wir Leads auswählen, die mit dem alten Marketo SDK erstellt wurden, das eine GCM-App verwendet hat?** Ja. Sie können alle Leads auswählen, die mit der Marketo SDK für Push-Benachrichtigungen erstellt wurden.
