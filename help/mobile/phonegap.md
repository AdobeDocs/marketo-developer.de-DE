---
title: Telefonlücke
feature: Mobile Marketing
description: Verwenden von PhoneGap mit Marketo auf Mobilgeräten
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# Telefonlücke

Integration des Marketo PhoneGap-Plug-ins

## Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Anwendungsschlüssels und der Munchkin-ID).
1. Push-Benachrichtigungen einrichten ([iOS](push-notifications.md) | [Android](push-notifications.md)).
1. [Installieren von PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installationsanweisungen

1. Einrichten des Marketo PhoneGap-Plug-ins

   Wenn die Cordova-CLI installiert ist, wechseln Sie zu Ihrem PhoneGap-Anwendungsverzeichnis und führen Sie den folgenden Befehl aus, um das Marketo-Plug-in zu Ihrer Anwendung hinzuzufügen:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. FCM-Plug-in installieren

   `$ cordova plugin add cordova-plugin-fcm`

   Um zu bestätigen, dass das Plug-in zur Anwendung hinzugefügt wurde, führen Sie den folgenden Befehl aus und überprüfen Sie

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migrieren zur neueren Version (optional)**

Um ein vorhandenes Plug-in zu entfernen, führen Sie den folgenden Befehl aus:

`$ cordova plugin remove com.marketo.plugin`

Um das Plug-in erneut hinzuzufügen, führen Sie den folgenden Befehl aus:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova Version 8.0.0 (Cordova@Android7.0.0) und höher**

Öffnen Sie nach der Erstellung der Cordova Android-Plattform die App mit Android Studio und aktualisieren Sie den `dirs` der `Marketo.gradle` im `com.marketo.plugin`.

```
repositories{    
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Fügen Sie die Plattformen hinzu, die für die App ausgewählt werden sollen `$cordova platform add android` `$ cordova platform add ios`

Überprüfen der Liste der `$cordova platform ls` hinzugefügten Plattformen

1. Firebase Cloud Messaging-Unterstützung

1. Konfigurieren der Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts in [&#128279;](https://console.firebase.google.com/)Firebase Console.
      1. Wählen Sie in [Firebase](https://console.firebase.google.com/)Konsole **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte aus und klicken Sie auf **[!UICONTROL Firebase hinzufügen]**.
      1. Wählen Sie im Firebase-Willkommensbildschirm „Firebase zu Ihrer Android-App hinzufügen“ aus.
      1. Geben Sie Ihren Paketnamen und SHA-1 an und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` für Ihre Firebase-App wird heruntergeladen.
   1. Navigieren Sie **[!UICONTROL Projekteinstellungen]** in [!UICONTROL Projektübersicht]
      1. Klicken Sie auf **[!UICONTROL Registerkarte]** Allgemein“. Laden Sie die Datei „google-services.json“ herunter.
      1. Klicken Sie auf **[!UICONTROL Registerkarte]** Cloud Messaging“. Kopieren [!UICONTROL Server-Schlüssel] und [!UICONTROL Absender-ID]. Stellen Sie diese [!UICONTROL Serverschlüssel] und [!UICONTROL Absender-ID] für Marketo bereit.
   1. Konfigurieren von FCM-Änderungen in der PhoneGap-App
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


### 3. Aktivieren von Push-Benachrichtigungen in xCode

Aktivieren der Push-Benachrichtigungsfunktion in einem xCode-Projekt.

### 4. Push-Benachrichtigungen tracken

Fügen Sie den folgenden Code in die `application:didFinishLaunchingWithOptions:` ein.

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` wie unten beschrieben

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` wie unten beschrieben

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### 5. Marketo-Framework initialisieren

Um sicherzustellen, dass das Marketo-Framework beim Start des Programms initiiert wird, fügen Sie den folgenden Code unter der `onDeviceReady` in Ihrer JavaScript-Hauptdatei hinzu.

Beachten Sie, dass wir `phonegap` als Framework-Typ für PhoneGap-Apps übergeben müssen.

### Aufbau

```
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

- Success Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework erfolgreich initialisiert wurde.
- Fehlgeschlagener Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework nicht initialisiert werden kann.
- MUNCHKIN-ID : Munchkin-ID, die zum Zeitpunkt der Registrierung von Marketo erhalten wurde.
- GEHEIMSCHLÜSSEL : Geheimer Schlüssel, der zum Zeitpunkt der Registrierung von Marketo empfangen wurde.

### 6. Initialisieren der Marketo-Push-Benachrichtigung

Um sicherzustellen, dass die Marketo-Push-Benachrichtigung gestartet wird, fügen Sie der Hauptdatei von JavaScript nach der Initialisierungsfunktion den folgenden Code hinzu.

### Aufbau

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung erfolgreich initialisiert wurde.
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung nicht initialisiert werden kann.
- GCM_PROJECT_ID : GCM-Projekt-ID, die nach dem Erstellen der App in [&#128279;](https://console.developers.google.com/) Entwicklerkonsole von Google gefunden wurde.

Die Registrierung des Tokens kann auch beim Abmelden aufgehoben werden.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead verknüpfen

Sie können einen Marketo-Lead erstellen, indem Sie die Funktion AssociateLead aufrufen.

### Aufbau

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead erfolgreich verknüpft.
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead nicht verknüpfen kann.
- Lead-Daten : Lead-Daten im JSON-Zeichenfolgenformat.

### Beispiel

```
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

Sie können jede vom Benutzer durchgeführte Aktion melden, indem Sie die `reportaction`-Funktion aufrufen.

### Aufbau

```
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parameter

- Erfolgs-Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework eine Aktion erfolgreich meldet.
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn das Marketo-Framework keine Berichtsaktion ausgibt.
- Aktionsname : Aktionsname.
- Aktionsdaten : Aktionsdaten im JSON-Zeichenfolgenformat.

### Beispiel

```
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

Binden Sie die Ereignistypen „Anhalten“ und „Fortsetzen“ wie unten gezeigt, um Start- und Stopp-Ereignisse zu melden.  Damit wird die in der Mobile App verbrachte Zeit verfolgt. Hinweis: Dies ist in Android erforderlich.

```
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

Je nach verwendeter Methode wird ein neu erstellter Lead von verschiedenen Triggern und Filtern erkannt. Leads, die mit der MME SDK- oder REST-API erstellt wurden, werden in den Triggern und Filtern „Lead erstellt“ angezeigt. Durch Formularübermittlungen erstellte Leads werden in den Triggern und Filtern des „ausgefüllten Formulars“ angezeigt.

Es empfiehlt sich, bei der Erstellung von Leads die Konsistenz mit der von der Web-Anwendung verwendeten Methode zu wahren. Wenn Sie bereits über eine Web-Anwendung verfügen, die die Formularübermittlung als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-Anwendung. Wenn Sie bereits über eine Web-Anwendung verfügen, die unsere REST-API als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. Wenn Sie weder die Formularübermittlung noch die REST-API als Mechanismus zum Erstellen von Leads in Ihrer Web-Anwendung verwenden, können Sie erwägen, die MME-SDK zum Erstellen von Leads in Marketo zu verwenden.
