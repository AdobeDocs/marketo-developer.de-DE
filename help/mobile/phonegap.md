---
title: PhoneGap
feature: Mobile Marketing
description: Verwenden von PhoneGap mit Marketo auf Mobilgeräten
exl-id: 99f14c76-9438-4942-9309-643bca434d07
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 1%

---

# PhoneGap

Integration des Marketo PhoneGap-Plug-ins

## Voraussetzungen

1. [Fügen Sie eine Anwendung in Marketo Admin hinzu](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (rufen Sie Ihren Geheimnisschlüssel für die Anwendung und die Munchkin-ID ab).
1. Push-Benachrichtigungen einrichten ([iOS](push-notifications.md)) | [Android](push-notifications.md)).
1. [Installieren Sie PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installationsanweisungen

1. Einrichten des Marketo PhoneGap-Plug-ins

   Wenn die Cordova-CLI installiert ist, wechseln Sie zum Ordner der PhoneGap-Anwendung und führen Sie den folgenden Befehl aus, um das Marketo-Plug-in zu Ihrer Anwendung hinzuzufügen:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installieren des FCM-Plug-ins

   `$ cordova plugin add cordova-plugin-fcm`

   Führen Sie den folgenden Befehl aus und überprüfen Sie, ob das Plug-in zur Anwendung hinzugefügt wurde

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migration zur neueren Version (optional)**

Um ein vorhandenes Plug-in zu entfernen, führen Sie den folgenden Befehl aus:

`$ cordova plugin remove com.marketo.plugin`

Um das Plug-in erneut hinzuzufügen, führen Sie den folgenden Befehl aus:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova-Version 8.0.0 (Cordova@Android7.0.0) und höher**

Sobald die Cordova Android-Plattform erstellt ist, öffnen Sie die App mit Android Studio und aktualisieren Sie den `dirs` -Wert der im Ordner `com.marketo.plugin` enthaltenen Datei `Marketo.gradle` .

```
repositories{    
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Plattformen für die App hinzufügen `$cordova platform add android` `$ cordova platform add ios`

Überprüfen Sie die Liste der hinzugefügten Plattformen `$cordova platform ls`

1. Firebase Cloud Messaging-Unterstützung

1. Konfigurieren Sie die Firebase App in der Firebase Console.
   1. Erstellen/Hinzufügen eines Projekts in der Firebase-Konsole [](https://console.firebase.google.com/).
      1. Wählen Sie in der [Firebase-Konsole](https://console.firebase.google.com/) die Option **[!UICONTROL Projekt hinzufügen]** aus.
      1. Wählen Sie Ihr GCM-Projekt aus der Liste der vorhandenen Google Cloud-Projekte und dann **[!UICONTROL Firebase hinzufügen]** aus.
      1. Wählen Sie im Begrüßungsbildschirm von Firebase die Option &quot;Firebase zu Ihrer Android-App hinzufügen&quot;.
      1. Geben Sie Ihren Paketnamen und SHA-1 ein und wählen Sie **[!UICONTROL App hinzufügen]** aus. Eine neue `google-services.json` -Datei für Ihre Firebase-App wird heruntergeladen.
   1. Navigieren Sie in [!UICONTROL Project Overview] zu **[!UICONTROL Projekteinstellungen]** .
      1. Klicken Sie auf die Registerkarte **[!UICONTROL Allgemein]** . Laden Sie die Datei &quot;google-services.json&quot;herunter.
      1. Klicken Sie auf die Registerkarte **[!UICONTROL Cloud Messaging]** . Kopieren Sie [!UICONTROL Server-Schlüssel] und [!UICONTROL Sender-ID]. Stellen Sie diese [!UICONTROL Server-Schlüssel] und die [!UICONTROL Sender-ID] für Marketo bereit.
   1. Konfigurieren von FCM-Änderungen in der PhoneGap-App
      1. Verschieben Sie die heruntergeladene Datei &quot;google-services.json&quot;in den Stammordner des PhoneGap-App-Moduls .
      1. Entfernen Sie die Datei &quot;MyFirebaseInstanceIDService&quot;aus dem Speicherort `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (veraltet).
      1. Ändern Sie die Datei &quot;MyFirebaseMessagingService&quot;im Verzeichnis &quot;`platforms/android/app/src/main/java/com/gae/scaffolder/plugin`&quot; wie folgt:

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

         1. Ändern Sie die Datei &quot;fcm_config_files_process.js&quot;im Verzeichnis plugins/cordova-plugin-fcm/scripts wie folgt

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


### 3. Push-Benachrichtigungen in xCode aktivieren

Aktivieren Sie die Push-Benachrichtigungsfunktion im xCode-Projekt.

### 4. Push-Benachrichtigungen tracken

Fügen Sie den folgenden Code in die Funktion `application:didFinishLaunchingWithOptions:` ein.

>[!BEGINTABS]

>[!TAB Ziel C]

Aktualisieren Sie die `applicationDidBecomeActive` -Methode wie unten

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Aktualisieren Sie die `applicationDidBecomeActive` -Methode wie unten

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### 5. Initialisieren von Marketo Framework

Um sicherzustellen, dass das Marketo-Framework beim Start der App initiiert wird, fügen Sie den folgenden Code unter der Funktion `onDeviceReady` in Ihrer JavaScript-Hauptdatei hinzu.

Beachten Sie, dass wir `phonegap` als Framework-Typ für PhoneGap-Apps übergeben müssen.

### Syntax

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
- Rückruf mit Fehler : Funktion wird ausgeführt, wenn das Marketo-Framework nicht initialisiert werden kann.
- MUNCHKIN-ID : Munchkin-ID, die zum Zeitpunkt der Registrierung von Marketo empfangen wurde.
- GEHEIMSCHLÜSSEL : Geheimer Schlüssel, der zum Zeitpunkt der Registrierung von Marketo erhalten wurde.

### 6. Marketo-Push-Benachrichtigung initialisieren

Um sicherzustellen, dass Marketo-Push-Benachrichtigungen initiiert werden, fügen Sie den folgenden Code nach der Initialisierungsfunktion in Ihrer JavaScript-Hauptdatei hinzu.

### Syntax

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung erfolgreich initialisiert wird.
- Rückruf bei Fehler : Funktion wird ausgeführt, wenn die Initialisierung der Marketo-Push-Benachrichtigung fehlschlägt.
- GCM_PROJECT_ID : GCM-Projekt-ID, die nach dem Erstellen der App in der [Google Developers Console](https://console.developers.google.com/) gefunden wurde.

Die Registrierung des Tokens kann auch bei der Abmeldung aufgehoben werden.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead zuordnen

Sie können einen Marketo-Lead erstellen, indem Sie die Funktion &quot;AssociateLead&quot;aufrufen.

### Syntax

```
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead erfolgreich verknüpft.
- Fehler-Rückruf : Funktion wird ausgeführt, wenn das Marketo-Framework den Lead nicht zuordnen kann.
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

Sie können jede vom Benutzer ausgeführte Aktion melden, indem Sie die Funktion `reportaction` aufrufen.

### Syntax

```
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parameter

- Erfolgsrückruf : Funktion, die ausgeführt wird, wenn die Marketo-Framework-Berichtsaktion erfolgreich ausgeführt wird.
- Rückruf mit Fehler : Funktion wird ausgeführt, wenn das Marketo-Framework keine Aktion in Berichten aufzeichnet.
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

Binden Sie die Ereignistypen &quot;pause&quot;und &quot;resume&quot;, wie unten gezeigt, um Start- und Stopp-Ereignisse zu melden.  Damit wird die in Ihrer Mobile App verbrachte Zeit verfolgt. Hinweis: Dies ist in Android erforderlich.

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

## Erstellen von Leads

Es gibt drei Möglichkeiten, Leads aus einer Hybrid-App zu erstellen:

1. MARKETO MME SDK
1. MARKETO REST API
1. Formular senden

Je nach verwendeter Methode wird ein neu erstellter Lead von verschiedenen Triggern und Filtern erkannt. Leads, die mit dem MME SDK oder der REST API erstellt wurden, werden in den Triggern und Filtern &quot;Lead erstellt&quot;angezeigt. Leads, die von Formularübermittlungen erstellt wurden, erscheinen in den Triggern und Filtern &quot;Formular ausfüllen&quot;.

Die Best Practice besteht darin, mit der Methode konsistent zu bleiben, die von der Web-Anwendung beim Erstellen von Leads verwendet wird. Wenn Sie bereits über eine Web-App verfügen, die die Formularübermittlung als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. Wenn Sie bereits über eine Web-App verfügen, die unsere REST-API als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. In Fällen, in denen Sie weder die Formularübermittlung noch die REST-API als Mechanismus zum Erstellen von Leads in Ihrer Web-App verwenden, können Sie erwägen, das MME-SDK zum Erstellen von Leads in Marketo zu verwenden.
