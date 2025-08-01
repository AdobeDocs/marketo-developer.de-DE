---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Verwenden  [!DNL Ionic]  Marketo für Mobilgeräte
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 2%

---

# ionisch

In diesem Abschnitt wird die Integration des Marketo Cordova-Plug-ins beschrieben. [!DNL Ionic] Kondensator wird derzeit nicht unterstützt.

## Voraussetzungen

1. [Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Anwendungsschlüssels und der Munchkin-ID).
1. Push-Benachrichtigungen einrichten ([iOS](push-notifications.md) | [Android](push-notifications.md) ).
1. Installieren Sie [[!DNL Ionic]](https://ionicframework.com/getting-started/) &amp; [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Installationsanweisungen

### Marketo [!DNL Ionic]-Plug-in einrichten

1. Wenn die Cordova-CLI installiert ist, wechseln Sie zu Ihrem [!DNL Ionic]-Anwendungsverzeichnis und führen Sie den folgenden Befehl aus, um das Marketo-Plug-in zu Ihrer Anwendung hinzuzufügen:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Um zu bestätigen, dass das Plug-in zur Anwendung hinzugefügt wurde, führen Sie den folgenden Befehl aus:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Zu neuerer Version migrieren (optional)

1. Um ein vorhandenes Plug-in zu entfernen, führen Sie den folgenden Befehl aus:

   `$ ionic plugin remove com.marketo.plugin`

1. Um das Plug-in zu lesen, führen Sie den folgenden Befehl aus:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Aktivieren von Push-Benachrichtigungen in xCode

1. Aktivieren der Push-Benachrichtigungsfunktion in einem xCode-Projekt.![Benachrichtigungsfunktion](assets/notification-capability.png)

### Push-Benachrichtigungen tracken

Fügen Sie den folgenden Code in die `application:didFinishLaunchingWithOptions:` ein.

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

### Marketo-Framework initialisieren

Um sicherzustellen, dass das Marketo-Framework beim Start des Programms initiiert wird, fügen Sie den folgenden Code unter der `onDeviceReady` in Ihrer JavaScript-Hauptdatei hinzu.

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
- Fehlgeschlagener Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework nicht initialisiert werden kann.
- MUNCHKIN-ID : Munchkin-ID, die zum Zeitpunkt der Registrierung von Marketo erhalten wurde.
- GEHEIMSCHLÜSSEL : Geheimer Schlüssel, der zum Zeitpunkt der Registrierung von Marketo empfangen wurde.

### Marketo-Push-Benachrichtigung initialisieren

Um sicherzustellen, dass die Marketo-Push-Benachrichtigung initiiert wird, fügen Sie der Hauptdatei von JavaScript nach der initialisierten Funktion den folgenden Code hinzu.

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

- Success Callback : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung erfolgreich initialisiert wurde.
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn die Marketo-Push-Benachrichtigung nicht initialisiert werden kann.
- GCM_PROJECT_ID : GCM-Projekt-ID, die nach dem Erstellen der App in [ Entwicklerkonsole von Google gefunden wurde.](https://accounts.google.com/ServiceLogin?service=cloudconsole&passive=1209600&osid=1&continue=https://console.cloud.google.com/apis/dashboard&followup=https://console.cloud.google.com/apis/dashboard)

Die Registrierung des Tokens kann auch beim Abmelden aufgehoben werden.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Lead verknüpfen

Sie können einen Marketo-Lead erstellen, indem Sie die Funktion AssociateLead aufrufen.

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
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn das Marketo-Framework den Lead nicht verknüpfen kann.
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

Sie können jede vom Benutzer durchgeführte Aktion melden, indem Sie die `reportaction`-Funktion aufrufen.

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

- Erfolgs-Callback : Funktion, die ausgeführt wird, wenn das Marketo-Framework eine Aktion erfolgreich meldet.
- Fehlgeschlagener Rückruf : Funktion, die ausgeführt wird, wenn das Marketo-Framework keine Berichtsaktion ausgibt.
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

Binden Sie die Ereignistypen „Anhalten“ und „Fortsetzen“ wie unten gezeigt, um Start- und Stopp-Ereignisse zu melden. Damit wird die in der Mobile App verbrachte Zeit verfolgt. Hinweis: Dies ist in Android erforderlich.

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

Je nach verwendeter Methode wird ein neu erstellter Lead von verschiedenen Triggern und Filtern erkannt. Leads, die mit der MME SDK- oder REST-API erstellt wurden, werden in den Triggern und Filtern „Lead erstellt“ angezeigt. Durch Formularübermittlungen erstellte Leads werden in den Triggern und Filtern des „ausgefüllten Formulars“ angezeigt.

Es empfiehlt sich, bei der Erstellung von Leads die Konsistenz mit der von der Web-Anwendung verwendeten Methode zu wahren. Wenn Sie bereits über eine Web-Anwendung verfügen, die die Formularübermittlung als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-Anwendung. Wenn Sie bereits über eine Web-Anwendung verfügen, die unsere REST-API als Mechanismus zum Erstellen von Leads verwendet, verwenden Sie denselben Mechanismus beim Erstellen von Leads in Ihrer Hybrid-App. Wenn Sie weder die Formularübermittlung noch die REST-API als Mechanismus zum Erstellen von Leads in Ihrer Web-Anwendung verwenden, können Sie erwägen, die MME-SDK zum Erstellen von Leads in Marketo zu verwenden.
