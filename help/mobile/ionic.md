---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Verwenden von [!DNL Ionic] mit Marketo für Mobilgeräte
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---

# Ironisch

Hier wird die Integration des Marketo Cordova-Plug-ins beschrieben. [!DNL Ionic] Kondensatoren werden derzeit nicht unterstützt.

## Voraussetzungen

1. [Fügen Sie eine Anwendung in Marketo Admin hinzu](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (rufen Sie Ihren Geheimnisschlüssel für die Anwendung und die Munchkin-ID ab).
1. Push-Benachrichtigungen einrichten ([iOS](push-notifications.md)) | [Android](push-notifications.md) ).
1. Installieren Sie [[!DNL Ionic]](https://ionicframework.com/getting-started/) und [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installationsanweisungen

### Einrichten des Marketo [!DNL Ionic]-Plugins

1. Wenn die Cordova-CLI installiert ist, wechseln Sie zum Ordner Ihrer [!DNL Ionic]-Anwendung und führen Sie den folgenden Befehl aus, um das Marketo-Plug-in zu Ihrer Anwendung hinzuzufügen:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Plug-in zur Anwendung hinzugefügt wurde:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migration zur neueren Version (optional)

1. Um ein vorhandenes Plug-in zu entfernen, führen Sie den folgenden Befehl aus:

   `$ ionic plugin remove com.marketo.plugin`

1. Um das Plug-in zu lesen, führen Sie den folgenden Befehl aus:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Push-Benachrichtigungen in xCode aktivieren

1. Aktivieren Sie die Push-Benachrichtigungsfunktion im xCode-Projekt.![Benachrichtigungsfunktion](assets/notification-capability.png)

### Push-Benachrichtigungen verfolgen

Fügen Sie den folgenden Code in die Funktion `application:didFinishLaunchingWithOptions:` ein.

>[!BEGINTABS]

>[!TAB Ziel C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Marketo Framework initialisieren

Um sicherzustellen, dass das Marketo-Framework beim Start der App initiiert wird, fügen Sie den folgenden Code unter der Funktion `onDeviceReady` in Ihrer JavaScript-Hauptdatei hinzu.

Sie müssen `ionicCordova` als Framework-Typ für [!DNL Ionic] Cordova-Apps übergeben.

#### Syntax

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

#### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework erfolgreich initialisiert wurde.
- Rückruf mit Fehler : Funktion wird ausgeführt, wenn das Marketo-Framework nicht initialisiert werden kann.
- MUNCHKIN-ID : Munchkin-ID, die zum Zeitpunkt der Registrierung von Marketo empfangen wurde.
- GEHEIMSCHLÜSSEL : Geheimer Schlüssel, der zum Zeitpunkt der Registrierung von Marketo erhalten wurde.

### Marketo-Push-Benachrichtigung initialisieren

Um sicherzustellen, dass Marketo-Push-Benachrichtigungen initiiert werden, fügen Sie den folgenden Code nach der initialisierten Funktion in Ihrer JavaScript-Hauptdatei hinzu.

#### Syntax

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

#### Parameter

- Success Callback : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung erfolgreich initialisiert wird.
- Rückruf bei Fehler : Funktion wird ausgeführt, wenn die Initialisierung der Marketo-Push-Benachrichtigung fehlschlägt.
- GCM_PROJECT_ID : GCM-Projekt-ID, die nach dem Erstellen der App in der [Google Developers Console](https://accounts.google.com/ServiceLogin?service=cloudconsole&amp;passive=1209600&amp;osid=1&amp;continue=https://console.cloud.google.com/apis/dashboard&amp;followup=https://console.cloud.google.com/apis/dashboard) gefunden wurde.

Die Registrierung des Tokens kann auch bei der Abmeldung aufgehoben werden.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead zuordnen

Sie können einen Marketo-Lead erstellen, indem Sie die Funktion &quot;AssociateLead&quot;aufrufen.

### Syntax

```javascript
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

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Ionic";
lead[marketo.KEY_LAST_NAME] = "App";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";

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

```javascript
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

Binden Sie die Ereignistypen &quot;pause&quot;und &quot;resume&quot;, wie unten gezeigt, um Start- und Stopp-Ereignisse zu melden. Damit wird die in Ihrer Mobile App verbrachte Zeit verfolgt. Hinweis: Dies ist in Android erforderlich.

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

## Erstellen von Leads

Es gibt drei Möglichkeiten, Leads aus einer Hybrid-App zu erstellen:

1. MARKETO MME SDK
1. MARKETO REST API
1. Formular senden

Je nach verwendeter Methode wird ein neu erstellter Lead von verschiedenen Triggern und Filtern erkannt. Leads, die mit dem MME SDK oder der REST API erstellt wurden, werden in den Triggern und Filtern &quot;Lead erstellt&quot;angezeigt. Leads, die von Formularübermittlungen erstellt wurden, erscheinen in den Triggern und Filtern &quot;Formular ausfüllen&quot;.

Die Best Practice besteht darin, mit der Methode konsistent zu bleiben, die von der Web-Anwendung beim Erstellen von Leads verwendet wird. Wenn Sie bereits über eine Web-App verfügen, die die Formularübermittlung als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. Wenn Sie bereits über eine Web-App verfügen, die unsere REST-API als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. In Fällen, in denen Sie weder die Formularübermittlung noch die REST-API als Mechanismus zum Erstellen von Leads in Ihrer Web-App verwenden, können Sie erwägen, das MME-SDK zum Erstellen von Leads in Marketo zu verwenden.
