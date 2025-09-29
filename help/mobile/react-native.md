---
title: React Native
feature: Mobile Marketing
description: Installieren und richten Sie die Marketo SDK in React Native-Apps mit den Schritten Android Gradle und iOS CocoaPods, nativer Modulüberbrückung, Push-Benachrichtigung und Lead-Zuordnung ein.
exl-id: 462fd32e-91f1-4582-93f2-9efe4d4761ff
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 1%

---

# React Native

Dieser Artikel enthält Informationen zur Installation und Einrichtung der nativen SDK von Marketo, um Ihre Mobile App in unsere Plattform zu integrieren.

## Voraussetzungen

[Anwendung in Marketo Admin hinzufügen](https://experienceleague.adobe.com/de/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (Abrufen des geheimen Anwendungsschlüssels und der Munchkin-ID).

## SDK-Integration

### Integration von Android SDK

**Setup mit Gradle**

Fügen Sie die Marketo SDK-Abhängigkeit mit der neuesten Version hinzu: Fügen Sie in der `build.gradle` auf Programmebene im Abschnitt „Abhängigkeiten“ hinzu (einschließlich der entsprechenden Version von Marketo SDK)

```
implementation 'com.marketo:MarketoSDK:0.x.x'
```

**MavenCentral-Repository hinzufügen**

Marketo SDK ist im [Maven Central Repository](https://mvnrepository.com/) verfügbar. Um diese Dateien zu synchronisieren, fügen Sie `mavencentral` Repository zum `build.gradle` hinzu

```
build script {
  repositories {
    google()
    mavencentral()
  }
}
```

Synchronisieren Sie dann Ihr Projekt mit den Gradle-Dateien.

#### Integration von iOS SDK

Bevor Sie eine Brücke für Ihr React Native-Projekt erstellen, müssen Sie unser SDK in Ihrem Xcode-Projekt einrichten.

**SDK-Integration - mit CocoaPods**

Die Verwendung unserer iOS SDK in Ihrer App ist ganz einfach. Führen Sie die folgenden Schritte aus, um es im Xcode-Projekt Ihrer App mit CocoaPods einzurichten, damit Sie unsere Plattform mit Ihrer App integrieren können.

Download [CocoaPods](https://cocoapods.org/) - Als Ruby-Edelstein verteilt, ist es ein Abhängigkeitsmanager für Objective-C und Swift, der die Verwendung von Drittanbieterbibliotheken in Ihrem Code vereinfacht, wie z. B. die iOS SDK.

Um es herunterzuladen und zu installieren, starten Sie ein Befehlszeilen-Terminal auf Ihrer Mac und führen Sie den folgenden Befehl darauf aus:

1. Installieren Sie CocoaPods.

`$ sudo gem install cocoapods`

1. Öffnen Sie Ihr Profil. (Innerhalb des iOS-Ordners des ReactNative-Projekts)

`$ open -a Xcode Podfile`

1. Fügen Sie Ihrem Profil die folgende Zeile hinzu.

`$ pod 'Marketo-iOS-SDK'`

1. Speichern und schließen Sie Ihr Profil.

1. Herunterladen und Installieren von Marketo iOS SDK.

`$ pod install`

1. Öffnen Sie Workspace in Xcode.

`$ open App.xcworkspace`

## Native Installationsanweisungen für Module

Manchmal muss eine React Native-App auf eine native Plattform-API zugreifen, die in JavaScript nicht standardmäßig verfügbar ist, z. B. auf die nativen APIs für den Zugriff auf Apple oder Google Pay. Vielleicht möchten Sie einige bestehende Objective-C-, Swift-, Java- oder C++-Bibliotheken wiederverwenden, ohne sie erneut in JavaScript implementieren zu müssen, oder einen leistungsstarken Multi-Thread-Code für Dinge wie die Bildverarbeitung schreiben.

Das NativeModule-System stellt Instanzen von Java-/Objective-C/C++-Klassen (nativ) für JavaScript (JS) als JS-Objekte zur Verfügung, sodass Sie beliebigen nativen Code von JS aus ausführen können. Obwohl wir nicht erwarten, dass diese Funktion Teil des üblichen Entwicklungsprozesses ist, ist es wichtig, dass sie vorhanden ist. Wenn React Native keine native API exportiert, die Ihre JS-App benötigt, sollten Sie sie selbst exportieren können!

React Native Bridge wird für die Kommunikation zwischen der JSX- und der nativen App-Ebene verwendet. In unserem Fall kann die Host-App den JSX-Code schreiben, der die Methoden von Marketo SDK aufrufen kann.

### Android

Diese Datei enthält die Wrapper-Methoden , die die Methoden von Marketo SDK intern mit den von Ihnen angegebenen Parametern aufrufen können.

```
public class RNMarketoModule extends ReactContextBaseJavaModule {

   final Marketo marketoSdk;
   RNMarketoModule(ReactApplicationContext context) {
       super(context);
       marketoSdk = Marketo.getInstance(context);
   }

   @NonNull
   @Override
   public String getName() {
       return "RNMarketoModule";
   }

   @ReactMethod
   public void associateLead(ReadableMap leadData) {

       MarketoLead mLead = new MarketoLead();
       try {
           mLead.setCity(leadData.getString(MarketoLead.KEY_CITY));
           mLead.setFirstName(leadData.getString(MarketoLead.KEY_FIRST_NAME));
           mLead.setLastName(leadData.getString(MarketoLead.KEY_LAST_NAME));
           mLead.setAddress(leadData.getString(MarketoLead.KEY_ADDRESS));
           mLead.setEmail(leadData.getString(MarketoLead.KEY_EMAIL));
           mLead.setBirthDay(leadData.getString(MarketoLead.KEY_BIRTHDAY));
           mLead.setCountry(leadData.getString(MarketoLead.KEY_COUNTRY));
           mLead.setFacebookId(leadData.getString(MarketoLead.KEY_FACEBOOK));
           mLead.setGender(leadData.getString(MarketoLead.KEY_GENDER));
           mLead.setState(leadData.getString(MarketoLead.KEY_STATE));
           mLead.setPostalCode(leadData.getString(MarketoLead.KEY_POSTAL_CODE));
           mLead.setTwitterId(leadData.getString(MarketoLead.KEY_TWITTER));
           marketoSdk.associateLead(mLead);
       }
       catch (MktoException e){
       }
   }
   @ReactMethod
   public void setSecureSignature(ReadableMap readableMap) {
       MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
       secureMode.setAccessKey(readableMap.getString("accessKey"));
       secureMode.setEmail(readableMap.getString("email"));
       secureMode.setSignature(readableMap.getString("signature"));
       secureMode.setTimestamp(readableMap.getInt("timeStamp"));
       marketoSdk.setSecureSignature(secureMode);
   }
   @ReactMethod
      public void initializeSDK(String frameworkType, String munchkinId, String appSecreteKey){
          marketoSdk.initializeSDK(munchkinId,appSecreteKey,frameworkType);
    }


   @ReactMethod
   public void initializeMarketoPush(String projectId){
       marketoSdk.initializeMarketoPush( projectId);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId, String channelName){
       marketoSdk.initializeMarketoPush( projectId, channelName);
   }

   @ReactMethod
   public void uninitializeMarketoPush(){
       marketoSdk.uninitializeMarketoPush();
   }

   @ReactMethod
   public void reportAction(String action){
       Marketo.reportAction(action, null);
   }

   @ReactMethod
   public void reportAction(String action, ReadableMap readableMap){
       MarketoActionMetaData marketoActionMetaData = new MarketoActionMetaData();
       marketoActionMetaData.setActionDetails(readableMap.getString("setMetric"));
       marketoActionMetaData.setActionMetric(readableMap.getString("setLength"));
       marketoActionMetaData.setActionLength(readableMap.getString("actionDetails"));
       marketoActionMetaData.setActionType(readableMap.getString("actionType"));
       Marketo.reportAction(action, marketoActionMetaData);
   }
}
```

**Paket registrieren**

Informieren Sie React-Native über das Marketo-Paket.

```
public class MarketoPluginPackage implements ReactPackage {

   @NonNull
   @Override
   public List createNativeModules(@NonNull ReactApplicationContext reactContext) {
           List modules = new ArrayList<>();

           modules.add(new RNMarketoModule(reactContext));

           return modules;
   }

   @NonNull
   @Override
   public List createViewManagers(@NonNull ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }
}
```

Um die Paketregistrierung abzuschließen, fügen Sie das MarketoPluginPackage der React-Paketliste in der Application-Klasse hinzu:

```
public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }

        @Override
        protected List getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List packages = new PackageList(this).getPackages();
          packages.add(new MarketoPluginPackage());     //Add the Marketo Package here.
          // Packages that cannot be autolinked yet can be added manually here, for example:
          return packages;
        }
}
```

### iOS

Im folgenden Handbuch erstellen Sie ein natives Modul, _RNMarketoModule_, mit dem Sie von JavaScript aus auf Marketo-APIs zugreifen können.

Öffnen Sie zunächst das iOS-Projekt in Ihrer React Native-Anwendung in Xcode. Ihr iOS-Projekt finden Sie hier in einer React Native-App. Es wird empfohlen, zum Schreiben des nativen Codes Xcode zu verwenden. Xcode wurde für die iOS-Entwicklung entwickelt und seine Verwendung hilft Ihnen, kleinere Fehler wie Code-Syntax schnell zu beheben.

Erstellen Sie unsere wichtigsten benutzerdefinierten Kopfzeilen- und Implementierungsdateien des nativen Moduls. Erstellen Sie eine neue Datei mit dem Namen `MktoBridge.h` und fügen Sie ihr Folgendes hinzu:

```
//
//  MktoBridge.h
//
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject

@end

NS_ASSUME_NONNULL_END
```

Erstellen Sie die entsprechende Implementierungsdatei `MktoBridge.m` im selben Ordner und schließen Sie die folgenden Inhalte ein:

```
//
//  MktoBridge.h
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject <RCTBridgeModule>

@end

NS_ASSUME_NONNULL_END


//
//  MktoBridge.m
//  Created by Marketo, An Adobe company.
//

#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBridge.h>
#import "ConstantStringsHeader.h"

@implementation MktoBridge

RCT_EXPORT_MODULE(RNMarketoModule);

+(BOOL)requiresMainQueueSetup{
  return NO;
}

RCT_EXPORT_METHOD(initializeSDK:(NSString *) munchkinId SecretKey: (NSString *) secretKey andFrameworkType: (NSString *) frameworkType){
  [[Marketo sharedInstance] initializeWithMunchkinID:munchkinId appSecret:secretKey mobileFrameworkType:frameworkType launchOptions:nil];
}

RCT_EXPORT_METHOD(reportAction:(NSString *)actionName withMetaData:(NSDictionary *)metaData){
  MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
  [meta setType:[metaData objectForKey:KEY_ACTION_TYPE]];
  [meta setDetails:[metaData objectForKey:KEY_ACTION_DETAILS]];
  [meta setLength:[metaData valueForKey:KEY_ACTION_LENGTH]];
  [meta setMetric:[metaData valueForKey:KEY_ACTION_METRIC]];
  [[Marketo sharedInstance] reportAction:actionName withMetaData:meta];
}

RCT_EXPORT_METHOD(associateLead:(NSDictionary *)leadDetails){
  MarketoLead *lead = [[MarketoLead alloc] init];
  if ([leadDetails objectForKey:KEY_EMAIL] != nil) {
    [lead setEmail:[leadDetails objectForKey:KEY_EMAIL]];
  }
  if ([leadDetails objectForKey:KEY_FIRST_NAME] != nil) {
    [lead setFirstName:[leadDetails objectForKey:KEY_FIRST_NAME]];
  }

  if ([leadDetails objectForKey:KEY_LAST_NAME] != nil) {
    [lead setLastName:[leadDetails objectForKey:KEY_LAST_NAME]];
  }

  if ([leadDetails objectForKey:KEY_CITY] != nil) {
    [lead setCity:[leadDetails objectForKey:KEY_CITY]];
  }
    [[Marketo sharedInstance] associateLead:lead];
}

RCT_EXPORT_METHOD(uninitializeMarketoPush){
  [[Marketo sharedInstance] unregisterPushDeviceToken];
}

RCT_EXPORT_METHOD(reportAll){
  [[Marketo sharedInstance] reportAll];
}

RCT_EXPORT_METHOD(setSecureSignature:(NSDictionary *)secureSignature){
  MKTSecuritySignature *secSignature = [[MKTSecuritySignature alloc]
                                        initWithAccessKey:[secureSignature objectForKey:KEY_ACCESSKEY]
                                        signature:[secureSignature objectForKey:KEY_SIGNATURE]
                                        timestamp: [secureSignature objectForKey:KEY_EMAIL]
                                        email:[secureSignature objectForKey:KEY_EMAIL]];

    [[Marketo sharedInstance] setSecureSignature:secSignature];
}

RCT_EXPORT_METHOD(requestPermission:(RCTPromiseResolveBlock)resolve rejecter:(RCTPromiseRejectBlock)reject) {
  UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
  [center requestAuthorizationWithOptions:(UNAuthorizationOptionAlert + UNAuthorizationOptionSound + UNAuthorizationOptionBadge)
                        completionHandler:^(BOOL granted, NSError * _Nullable error) {
    if (error) {
      reject(@"PERMISSION_ERROR", @"Permission request failed", error);
    } else {
      resolve(@(granted));
    }
  }];
}

RCT_EXPORT_METHOD(registerForRemoteNotifications) {
  dispatch_async(dispatch_get_main_queue(), ^{
    [[UIApplication sharedApplication] registerForRemoteNotifications];
  });
}


@end
```

#### Initialisieren von Marketo SDK

Suchen Sie in der Anwendung einen Ort, an dem Sie der Methode createCalendarEvent() des nativen Moduls einen Aufruf hinzufügen möchten. Nachfolgend finden Sie ein Beispiel für eine Komponente, NewModuleButton, die Sie Ihrer App hinzufügen können. Sie können das native Modul in der Funktion onPress() von NewModuleButton aufrufen.

```
import React from 'react';
import { NativeModules, Button } from 'react-native';

const NewModuleButton = () => {
  const onPress = () => {
    console.log('We will invoke the native module here!');
  };

  return (

  );
  };

export default NewModuleButton;
```

Diese JavaScript-Datei lädt das native Modul auf die JavaScript-Ebene.

```javascript
import React from 'react';
import {Node} from 'react';
import { NativeModules } from 'react-native';

const { RNMarketoModule } = NativeModules;
```

Sobald die oben genannten Dateien korrekt platziert sind, können wir das js-Modul in eine beliebige js-Klasse importieren und seine Methoden direkt aufrufen. Beispiel:

Beachten Sie, dass wir „reactNative“ als Framework-Typ für React-native Apps übergeben müssen.

```
// Initialize marketo SDK with Munchkin & Seretkey you have from step 1.
RNMarketoModule.initializeSDK("MunchkinID","SecreteKEY","FrameworkType")

//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})

//You can report any user performed action by calling the reportaction function.
RNMarketoModule.reportAction("Bought Shirt", {actionType:"Shopping", actionDetails: "Red Shirt", setLength : 20, setMetric : 30 })

//You can set Secure Signature by calling this method.
RNMarketoModule.setSecureSignature({accessKey: "Key102", email: "testleadrk@001.com", signature : "asdfghjkloiuytds", timeStamp: "12345678987654"})

//This function will Enable user notifications (Only Android)
RNMarketoModule.initializeMarketoPush("350312872033", "MKTO")

//The token can also be unregistered on logout.
RNMarketoModule.uninitializeMarketoPush()
```

#### Konfigurieren von Push-Benachrichtigungen

Push mit Projekt-ID und Kanalnamen initialisieren

```
RNMarketoModule.initializeMarketoPush("ProjectId", "Channel_name")
```

Fügen Sie den folgenden Service zu `AndroidManifest.xml` hinzu

```xml
<service android:exported="true" android:name=".MyFirebaseMessagingService" android:stopWithTask="true">
    <intent-filter>
        <action  android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter/>
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter/>
</activity/>
```

Erstellen Sie eine Klasse mit dem Namen `FirebaseMessagingService.java` und fügen Sie den folgenden Code hinzu

```java
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import com.marketo.Marketo;

public class MyFirebaseMessagingService extends FirebaseMessagingService {

   @Override
   public void onNewToken(String token) {
       super.onNewToken(token);
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.setPushNotificationToken(token);
   }

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.showPushNotification(remoteMessage);
   }
}
```

In Ihrem Xcode-Projekt müssen Berechtigungen aktiviert sein, um Push-Benachrichtigungen an das Gerät des Benutzers senden zu können.

Um Push-Benachrichtigungen zu senden, [Push-Benachrichtigungen hinzufügen](push-notifications.md).

Einrichten von iOS-Push-Benachrichtigungen,
Erstellen Sie die Datei PushNotifications.tsx und fügen Sie Folgendes hinzu:

```
import { NativeModules } from 'react-native';
const { RNMarketoModule } = NativeModules;

const requestPermission = (): Promise<boolean> => {
return RNMarketoModule.requestPermission()
.then((granted: boolean) => {
console.log('Permission granted:', granted);
return granted;
})
.catch((error: any) => {
console.error('Permission error:', error);
throw error;
});
};

const registerForRemoteNotifications = (): void => {
RNMarketoModule.registerForRemoteNotifications();
};

export { requestPermission, registerForRemoteNotifications };
```

`App.tsx` hinzufügen, um Push-Benachrichtigungen zuzulassen

```
import React, { useEffect } from 'react';

useEffect(() => {
requestPermission().then(granted => {
if (granted) {
registerForRemoteNotifications();
}
});
}, []);
```

Aktualisieren Sie `AppDelegate.mm` mit APNS-Delegatmethoden:

```
#import "AppDelegate.h"
#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBundleURLProvider.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.moduleName = @"MyNewApp";
  // You can add your custom initial props in the dictionary below.
  // They will be passed down to the ViewController used by React Native.
  self.initialProps = @{};

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  return [self bundleURL];
}

-(void)userNotificationCenter:(UNUserNotificationCenter *)center
      willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center
                      didReceiveNotificationResponse:response
                               withCompletionHandler:completionHandler];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [[Marketo sharedInstance] unregisterPushDeviceToken];
}

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error{
  NSLog(@"Failed to register for remote notification - %@", [error userInfo]);
}

- (NSURL *)bundleURL
{
#if DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index"];
#else
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
}

@end
```

### Hinzufügen von Testgeräten

**Android**

Fügen Sie „MarketoActivity“ zu `AndroidManifest.xml` Datei im Anwendungs-Tag hinzu.

```xml
<activity android:name="com.marketo.MarketoActivity" android:configChanges="orientation|screenSize" android:exported="true">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

**iOS**

1. Wählen Sie Projekt > Target > Info > URL-Typen aus.

1. Kennung hinzufügen: ${PRODUCT_NAME}

1. URL-Schemata festlegen: `mkto-<S_ecret Key_>`

1. `application:openURL:sourceApplication:annotation:` in `AppDelegate.m`-Datei einschließen (Objective-C)

**iOS - Benutzerdefinierte URL-Typen/Deeplinks in AppDelegate verarbeiten**

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

Diese Konstanten werden beim Aufruf der API aus JavaScript verwendet. Sie müssen konstante Dateien erstellen und Folgendes hinzufügen.

```
// Lead attributes.
static NSString *const KEY_FIRST_NAME = @"firstName";
static NSString *const KEY_LAST_NAME = @"lastName";
static NSString *const KEY_ADDRESS = @"address";
static NSString *const KEY_CITY = @"city";
static NSString *const KEY_STATE = @"state";
static NSString *const KEY_COUNTRY = @"country";
static NSString *const KEY_GENDER = @"gender";
static NSString *const KEY_EMAIL = @"email";
static NSString *const KEY_TWITTER = @"twitterId";
static NSString *const KEY_FACEBOOK = @"facebookId";
static NSString *const KEY_LINKEDIN = @"linkedinId";
static NSString *const KEY_LEAD_SOURCE = @"leadSource";
static NSString *const KEY_BIRTHDAY = @"dateOfBirth";
static NSString *const KEY_FACEBOOK_PROFILE_URL = @"facebookProfileURL";
static NSString *const KEY_FACEBOOK_PROFILE_PIC = @"facebookPhotoURL";

// Custom actions
static NSString *const KEY_ACTION_TYPE = @"actionType";
static NSString *const KEY_ACTION_DETAILS = @"actionDetails";
static NSString *const KEY_ACTION_LENGTH = @"setLength";
static NSString *const KEY_ACTION_METRIC = @"setMetric";

//Secure Signature
static NSString *const KEY_ACCESSKEY = @"accessKey";
static NSString *const KEY_SIGNATURE = @"signature";
static NSString *const KEY_TIMESTAMP = @"timeStamp";
```

Beispielverwendung

```
//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})
```
