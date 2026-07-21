---
title: Telefonlücke
feature: Mobile Marketing
description: Richten Sie das Marketo PhoneGap-Plug-in mit Cordova ein, konfigurieren Sie Firebase Cloud Messaging, aktivieren Sie iOS- und Android-Push-Benachrichtigungen, verfolgen Sie Benachrichtigungen und initialisieren Sie die SDK.
exl-id: 99f14c76-9438-4942-9309-643bca434d07
TQID: https://experienceleague.adobe.com/eFAwR7r5IE6vKigsEWrJdCmC3VrfB-nl0h8x7Vgt1VY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 775
ht-degree: 2%

---

# Telefonlücke

Integrieren des Marketo PhoneGap-Plug-ins in eine Cordova-App.

## Voraussetzungen

1. [Fügen Sie eine Anwendung in Marketo Admin hinzu](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) und rufen Sie den geheimen Anwendungsschlüssel und die Munchkin-ID ab.
1. Push-Benachrichtigungen für [iOS](push-notifications.md) oder [Android einrichten](push-notifications.md).
1. [Installieren von PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installationsanweisungen

1. Richten Sie das Marketo PhoneGap-Plug-in ein.

   Gehen Sie zum PhoneGap-Anwendungsverzeichnis und führen Sie den folgenden Befehl aus, um das Marketo-Plug-in hinzuzufügen:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installieren Sie das FCM-Plug-in.

   `$ cordova plugin add cordova-plugin-fcm`

   Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Plug-in hinzugefügt wurde:

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrieren zur neueren Version (optional)**

Um ein vorhandenes Plug-in zu entfernen, führen Sie den folgenden Befehl aus:

`$ cordova plugin remove com.marketo.plugin`

Um das Plug-in erneut hinzuzufügen, führen Sie den folgenden Befehl aus:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova Version 8.0.0 (Cordova@Android7.0.0) und höher**

Öffnen Sie nach dem Erstellen der Cordova Android-Plattform die App in Android Studio. Aktualisieren Sie den `dirs` in der `Marketo.gradle` im `com.marketo.plugin`.

```groovy
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Zielplattformen für die App hinzufügen: `$cordova platform add android` `$ cordova platform add ios`

Überprüfen Sie die hinzugefügten Plattformen: `$cordova platform ls`

1. Firebase Cloud Messaging-Unterstützung

1. Konfigurieren Sie die Firebase-App in der Firebase Console.
   1. Erstellen oder Hinzufügen eines Projekts in der [&#128279;](https://console.firebase.google.com/)Firebase Console.
      1. Wählen Sie in [Firebase](https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
      1. Wählen Sie im Firebase-Willkommensbildschirm „Firebase zu Ihrer Android-App hinzufügen“ aus.
      1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` für Ihre Firebase-App wird heruntergeladen.
   1. Navigieren Sie **[!UICONTROL Projekteinstellungen]** in [!UICONTROL Projektübersicht].
      1. Wählen Sie die **[!UICONTROL Allgemein]** und laden Sie die Datei „google-services.json“ herunter.
      1. Wählen Sie die Registerkarte **[!UICONTROL Cloud Messaging]**. Kopieren Sie [!UICONTROL Serverschlüssel] und [!UICONTROL Absender-ID] und stellen Sie sie Marketo zur Verfügung.
   1. Konfigurieren von FCM in der PhoneGap-App.
      1. Verschieben Sie die heruntergeladene Datei „google-services.json“ in das Stammverzeichnis des PhoneGap-App-Moduls.
      1. Datei &#39;MyFirebaseInstanceIDService&#39; aus Speicherort `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (veraltet) entfernen
      1. Ändern Sie die Datei „MyFirebaseMessagingService“ am Speicherort `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` wie folgt:

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Ändern Sie die Datei „fcm_config_files_process.js“ am Speicherort plugins/cordova-plugin-fcm/scripts wie folgt

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```

### &#x200B;3. Aktivieren von Push-Benachrichtigungen in xCode

Aktivieren der Push-Benachrichtigungsfunktion im xCode-Projekt.

### &#x200B;4. Push-Benachrichtigungen tracken

Fügen Sie den folgenden Code in die `application:didFinishLaunchingWithOptions:` ein.

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` wie folgt.

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` wie folgt.

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Marketo-Framework initialisieren

Um das Marketo-Framework beim Start der App zu initialisieren, fügen Sie den folgenden Code unter der `onDeviceReady` in der JavaScript-Hauptdatei hinzu.

Übergeben Sie `phonegap` als Framework-Typ für PhoneGap-Apps.

### Syntax

```javascript
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

### Parameter

- Success Callback: Funktion, die ausgeführt wird, wenn das Marketo-Framework erfolgreich initialisiert wurde.
- Failure Callback: Funktion, die ausgeführt wird, wenn das Marketo-Framework nicht initialisiert werden kann.
- MUNCHKIN-ID: Munchkin-ID, die während der Registrierung von Marketo empfangen wurde.
- SECRET KEY: Geheimer Schlüssel, der von Marketo während der Registrierung empfangen wurde.

### &#x200B;6. Marketo-Push-Benachrichtigung initialisieren

Um Marketo-Push-Benachrichtigungen zu initialisieren, fügen Sie in der JavaScript-Hauptdatei nach der Initialisierungsfunktion den folgenden Code hinzu.

### Syntax

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parameter

- Success Callback: Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung erfolgreich initialisiert wurde.
- Fehlgeschlagener Rückruf: Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung nicht initialisiert werden kann.
- GCM_PROJECT_ID: GCM-Projekt-ID in [Google Developers Console](https://console.developers.google.com/) nach dem Erstellen der App.

Sie können die Registrierung des Tokens auch bei der Abmeldung aufheben.

```javascript
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead verknüpfen

Rufen Sie die Funktion AssociateLead auf, um einen Marketo-Lead zu erstellen.

### Syntax

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parameter

- Success Callback: Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead erfolgreich verknüpft.
- Failure Callback: Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead nicht verknüpfen kann.
- Lead-Daten: Lead-Daten im JSON-Zeichenfolgenformat.

### Beispiel

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Berichtsaktion

Rufen Sie die Funktion `reportaction` auf, um eine Benutzeraktion zu melden.

### Syntax

```javascript
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parameter

- Erfolgreicher Rückruf: Funktion, die ausgeführt wird, wenn das Marketo-Framework die Aktion erfolgreich meldet.
- Fehlgeschlagener Rückruf: Funktion, die ausgeführt wird, wenn das Marketo-Framework die Aktion nicht meldet.
- Aktionsname: Aktionsname.
- Aktionsdaten: Aktionsdaten im JSON-Zeichenfolgenformat.

### Beispiel

```javascript
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Sitzungsberichte

Binden Sie die Ereignistypen „Anhalten“ und „Fortsetzen“ an die Start- und Stopp-Ereignisse eines Berichts. Diese Ereignisse verfolgen die in der Mobile App verbrachte Zeit und sind in Android erforderlich.

```javascript
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Leads erstellen

Es gibt drei Möglichkeiten, Leads aus einer Hybrid-App zu erstellen:

1. MARKETO MME SDK
1. Marketo REST-API
1. Formular senden

Die Trigger und Filter, die einen neuen Lead erkennen, hängen von der Erstellungsmethode ab:

- Leads, die mit der MME SDK- oder REST-API erstellt wurden, werden in den Triggern und Filtern „Lead erstellt“ angezeigt.
- Durch Formularübermittlung erstellte Leads werden in den Triggern und Filtern des „ausgefüllten Formulars“ angezeigt.

Verwenden Sie dieselbe Methode zur Lead-Erstellung in der Hybrid-App und der Web-App. Wenn die Web-Anwendung die Formularübermittlung oder die REST-API verwendet, verwenden Sie diese Methode in der Hybrid-App. Wenn die Web-Anwendung keine dieser Methoden verwendet, sollten Sie die Verwendung der MME-SDK zum Erstellen von Leads in Marketo erwägen.
