---
title: Push-Benachrichtigungen
feature: Mobile Marketing
description: Aktivieren von Push-Benachrichtigungen für Marketo Mobile
exl-id: 41d657d8-9eea-4314-ab24-fd4cb2be7f61
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Push-Benachrichtigungen

Aktivieren von Push-Benachrichtigungen.

## Einrichten von Push-Benachrichtigungen in iOS

Es gibt drei Schritte zum Aktivieren von Push-Benachrichtigungen:

1. Push-Benachrichtigungen für das Apple Developer Account konfigurieren
1. Aktivieren Sie Push-Benachrichtigungen in xCode.
1. Aktivieren Sie Push-Benachrichtigungen in App mit dem Marketo SDK.

### Push-Benachrichtigungen für Apple-Entwicklerkonto konfigurieren

1. Melden Sie sich beim Apple Developer [Member Center](http://developer.apple.com/membercenter) an.
1. Klicken Sie auf &quot;Zertifikate, Kennungen und Profile&quot;.
1. Klicken Sie unter &quot;iOS, tvOS, watchOS&quot;auf den Ordner &quot;Zertifikate->Alle&quot;.
1. Wählen Sie auf dem Bildschirm oben links neben den Zertifikaten ![](assets/certificates-plus.png) das &quot;+&quot;aus.
1. Aktivieren Sie das Kontrollkästchen &quot;Apple Push Notification Service SSL (Sandbox &amp; Production)&quot;und klicken Sie auf &quot;Weiter&quot;.
1. Wählen Sie die Anwendungs-ID aus, die Sie für den App-Build verwenden.![](assets/push-appid.png)
1. Erstellen und laden Sie CSR hoch, um das Push-Zertifikat zu generieren. ![](assets/push-ssl.png)
1. Laden Sie das Zertifikat auf den lokalen Computer herunter und doppelklicken Sie zur Installation auf . ![](assets/certificate-download.png)
1. Öffnen Sie &quot;Keychain Access&quot;, klicken Sie mit der rechten Maustaste auf das Zertifikat und exportieren Sie zwei Elemente in die Datei &quot;`.p12`&quot;.![key_chain](assets/key-chain.png)
1. Laden Sie diese Datei über die Marketo-Admin Console hoch, um Benachrichtigungen zu konfigurieren.
1. Aktualisieren Sie App-Bereitstellungsprofile.

### Push-Benachrichtigungen in xCode aktivieren

Aktivieren Sie die Push-Benachrichtigungsfunktion im xCode-Projekt.![](assets/push-xcode.png)

### Push-Benachrichtigungen in App mit Marketo SDK aktivieren

Fügen Sie der Datei `AppDelegate.m` den folgenden Code hinzu, um Push-Benachrichtigungen an die Geräte Ihres Kunden zu senden.

**Hinweis** - Verwenden Sie bei Verwendung der Erweiterung [!DNL Adobe Launch] `ALMarketo` als Klassennamen.

Importieren Sie Folgendes in `AppDelegate.h`.

>[!BEGINTABS]

>[!TAB Ziel C]

```
#import <UserNotifications/UserNotifications.h>
```

>[!TAB Swift]

```
import UserNotifications
```

>[!ENDTABS]

Fügen Sie `UNUserNotificationCenterDelegate` zu `AppDelegate` hinzu, wie unten dargestellt.

>[!BEGINTABS]

>[!TAB Ziel C]

```
@interface AppDelegate : UIResponder <UIApplicationDelegate, UNUserNotificationCenterDelegate>
```

>[!TAB Swift]

```
class AppDelegate: UIResponder, UIApplicationDelegate , UNUserNotificationCenterDelegate
```

>[!ENDTABS]

Starten Sie den Push-Benachrichtigungsdienst. Um Push-Benachrichtigungen zu aktivieren, fügen Sie unten den Code hinzu.

>[!BEGINTABS]

>[!TAB Ziel C]

```objectivec
BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
        }];

    return YES;
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
            
    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound,    .badge]) { granted, error in
            if let error = error {
                print("\(error.localizedDescription)")
            } else {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
        
        return true
}
```

>[!ENDTABS]

Rufen Sie diese Methode auf, um den Registrierungsprozess mit Apple Push Service zu starten. Wenn die Registrierung erfolgreich ist, ruft die App die `application:didRegisterForRemoteNotificationsWithDeviceToken:` -Methode des App-Delegationsobjekts auf und übergibt es ein Geräte-Token.

Wenn die Registrierung fehlschlägt, ruft die App stattdessen die Methode `application:didFailToRegisterForRemoteNotificationsWithError:` des App-Delegats auf.

Registrieren Sie Push-Token bei Marketo. Um Push-Benachrichtigungen von Marketo zu erhalten, müssen Sie das Geräte-Token bei Marketo registrieren.

>[!BEGINTABS]

>[!TAB Ziel C]

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    // Register the push token with Marketo
    Marketo.sharedInstance().registerPushDeviceToken(deviceToken)
}
```

>[!ENDTABS]

Die Registrierung des Tokens kann auch aufgehoben werden, wenn sich der Benutzer abmeldet.

>[!BEGINTABS]

>[!TAB Ziel C]

```
[[Marketo sharedInstance] unregisterPushDeviceToken];
```

>[!TAB Swift]

```
Marketo.sharedInstance().unregisterPushDeviceToken
```

>[!ENDTABS]

Um das Push-Token erneut zu registrieren, extrahieren Sie den Code aus Schritt 3 in eine AppDelegate-Methode und rufen Sie von der ViewController-Anmeldemethode auf.

Push-Benachrichtigung verarbeiten. Um Push-Benachrichtigungen von Marketo zu erhalten, müssen Sie das Geräte-Token bei Marketo registrieren.

>[!BEGINTABS]

>[!TAB Ziel C]

```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [[Marketo sharedInstance] handlePushNotification:userInfo];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
    Marketo.sharedInstance().handlePushNotification(userInfo)
}
```

>[!ENDTABS]

Fügen Sie die folgende Methode in AppDelegate hinzu

Mit dieser Methode können Sie Warnhinweise, Töne oder Erhöhungszeichen anzeigen, während die App im Vordergrund ausgeführt wird. Sie müssen in dieser Methode den von Ihnen ausgewählten completionHandler aufrufen.

>[!BEGINTABS]

>[!TAB Ziel C]

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center
    willPresentNotification:(UNNotification *)notification
        withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{

    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter, 
            willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (
    UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .sound,.badge])
}
```

>[!ENDTABS]

Umgang mit neu empfangenen Push-Benachrichtigungen in AppDelegate

Die Methode wird auf den Delegaten aufgerufen, wenn der Benutzer auf die Benachrichtigung reagiert hat, indem er die Anwendung öffnet, die Benachrichtigung verworfen oder eine UNNotificationAction ausgewählt hat. Der Delegate muss festgelegt werden, bevor die Anwendung von applicationDidFinishLaunching: zurückgibt.

>[!BEGINTABS]

>[!TAB Ziel C]

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                didReceive response: UNNotificationResponse,
                                withCompletionHandler
                                completionHandler: @escaping () -> Void) {
        Marketo.sharedInstance().userNotificationCenter(center, didReceive: response, withCompletionHandler: completionHandler)
}
```

>[!ENDTABS]

Push-Benachrichtigungen tracken

Wenn Ihre App im Hintergrund ausgeführt wird (oder nicht aktiv ist), erhält das Gerät eine Push-Benachrichtigung, wie unten dargestellt. Marketo verfolgt, wann der Benutzer auf die Benachrichtigung tippt.

![mobile8](assets/mobile8.png)

Wenn das Gerät eine Push-Benachrichtigung erhält, wird diese an den `application:didReceiveRemoteNotification:` -Callback in Ihrem App-Delegaten übergeben.

Im Folgenden finden Sie ein Marketo-Aktivitätsprotokoll von Marketo, in dem App-Ereignisse und Push-Benachrichtigungs-Ereignisse angezeigt werden.

![mobile9](assets/mobile9.png)

## Einrichten von Push-Benachrichtigungen in Android

1. Fügen Sie die folgende Berechtigung innerhalb des Anwendungs-Tags hinzu.

   Öffnen Sie `AndroidManifest.xml` und fügen Sie die folgenden Berechtigungen hinzu. Ihre App muss die Berechtigungen &quot;INTERNET&quot;und &quot;ACCESS_NETWORK_STATE&quot;anfordern. Wenn Ihre App diese Berechtigungen bereits anfordert, überspringen Sie diesen Schritt.

   ```xml
   <uses‐permission android:name="android.permission.INTERNET"/>
   <uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
   
   <!‐‐Following permissions are required for push notification.‐‐>
   <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
   <!‐‐Keeps the processor from sleeping when a message is received.‐‐>
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
   <uses-permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" />
   <!-- This app has permission to register and receive data message. -->
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   ```

1. Einrichten von FCM mit HTTPv1 (Google hat am 12. Juni 2023 das [veraltete XMPP-Protokoll](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) und wird im Juni 2024 entfernt) 

- Aktivieren Sie MME FCM HTTPv1 im Marketo Feature Manager ![](assets/feature-manager.png)
   - Laden Sie die JSON-Datei des Dienstkontos für die App in MLM hoch.
   - Sie können die JSON-Datei &quot;Service Account&quot;von der Firebase Console herunterladen.   ![](assets/fcm-console.png)
   - Warten Sie eine Stunde nach dem Hochladen der JSON-Datei des Dienstkontos in Marketo, bevor Sie Push-Benachrichtigungen senden.  

## Android-Testgeräte

Fügen Sie Marketo-Aktivität in der Manifestdatei im Anwendungs-Tag hinzu.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

## Marketo Push-Dienst registrieren

1. Um Push-Benachrichtigungen von Marketo zu erhalten, müssen Sie den Firebase-Messaging-Dienst zu Ihrem `AndroidManifest.xml` hinzufügen. Fügen Sie vor dem schließenden Anwendungs-Tag hinzu.

   ```xml
   <meta-data
       android:name="com.google.android.gms.version"
       android:value="@integer/google_play_services_version" />
   <service android:name=".MyFirebaseMessagingService">
   <intent-filter>
   <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
   <action android:name="com.google.firebase.MESSAGING_EVENT"/>
   </intent-filter>
   </service>
   ```

1. Fügen Sie Marketo SDK-Methoden in der Datei &quot;`MyFirebaseMessagingService`&quot;wie folgt hinzu:

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String s) {
           super.onNewToken(s);
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.setPushNotificaitonToken(s);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.showPushNotificaiton(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

   **Hinweis** - Fügen Sie bei Verwendung der Adobe-Erweiterung wie folgt hinzu

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String token) {
           super.onNewToken(token);
           ALMarketo.setPushNotificationToken(token);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           ALMarketo.showPushNotification(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

**HINWEIS**: Das FCM-SDK fügt automatisch alle erforderlichen Berechtigungen sowie die erforderliche Empfängerfunktion hinzu. Achten Sie darauf, die folgenden veralteten (und potenziell schädlichen) Elemente aus dem Manifest Ihrer App zu entfernen, wenn Sie frühere Versionen des SDK verwendet haben.

```xml
<receiver android:name="com.marketo.MarketoBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <!‐‐Receives the actual messages.‐‐>
        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
        <!‐‐Register to enable push notification‐‐>
        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
        <!‐‐‐Replace YOUR_PACKAGE_NAME with your own package name‐‐>
        <category android:name="YOUR_PACKAGE_NAME"/>
    </intent-filter>
</receiver>

<!‐‐Marketo service to handle push registration and notification‐‐>
<service android:name="com.marketo.MarketoIntentService"/>
```

1. Marketo Push initialisieren Nach dem Speichern der oben genannten Konfiguration müssen Sie die Marketo-Push-Benachrichtigung initialisieren. Erstellen oder öffnen Sie Ihre Anwendungsklasse und kopieren/fügen Sie den unten stehenden Code ein. Sie können Ihre Absender-ID über die Firebase-Konsole abrufen.


   ```java
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   
   // Enable push notification here. The push notification channel name can by any string
   marketoSdk.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Verwenden Sie bei Verwendung der Erweiterung [!DNL Adobe Launch] die folgenden Anweisungen

   ```java
   // Enable push notification here. The push notification channel name can by any string
   ALMarketo.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Wenn Sie keine SENDER_ID haben, aktivieren Sie den Google Cloud Messaging Service, indem Sie die in [diesem Tutorial](https://developers.google.com/cloud-messaging/) beschriebenen Schritte ausführen.

   Die Registrierung des Tokens kann auch aufgehoben werden, wenn sich der Benutzer abmeldet.

   ```java
   marketoSdk.uninitializeMarketoPush();
   ```

   Verwenden Sie bei Verwendung der Erweiterung [!DNL Adobe Launch] die folgende Anweisung

   ```java
   ALMarketo.uninitializeMarketoPush();
   ```

   Hinweis: Um das Push-Token erneut zu registrieren, extrahieren Sie den Code aus Schritt 3 in eine AppDelegate-Methode und rufen Sie von der ViewController-Anmeldemethode auf.

1. Benachrichtigungssymbol festlegen (optional) Um ein benutzerdefiniertes Benachrichtigungssymbol zu konfigurieren, sollte die folgende Methode aufgerufen werden.

   ```java
   MarketoConfig.Notification config = new MarketoConfig.Notification();
   // Optional bitmap for honeycomb and above
   config.setNotificationLargeIcon(bitmap);
   
   // Required icon Resource ID
   config.setNotificationSmallIcon(R.drawable.notification_small_icon); 
   
   // Set the configuration 
   //Use the static methods on ALMarketo class when using Adobe Extension
   Marketo.getInstance(context).setNotificationConfig(config); 
   
   // Get the configuration set 
   Marketo.getInstance(context).getNotificationConfig();
   ```

## Fehlerbehebung

Das Einrichten von mobilen Push-Nachrichten umfasst viele Schritte und die Koordination von Entwicklern und Marketingexperten. Wenn Sie Schwierigkeiten haben, gibt es einige einfache Dinge, die Sie überprüfen können.

Nachdem Sie sich vergewissert haben, dass die einfachen Dinge korrekt sind, können Sie die Programmierdetails genauer untersuchen.

### Push-Nachricht wird nicht angezeigt

Überprüfen Sie zunächst, ob Push-Nachrichten auf dem Handset deaktiviert sind. Benutzer mit Mobilgeräten können steuern, ob sie Nachrichten für eine bestimmte App erhalten oder nicht. Häufig deaktivieren Entwickler (und Marketingexperten) diese Nachrichten irgendwann während der Entwicklung. Zunächst muss geprüft werden, ob der Empfänger Push-Nachrichten für Ihre App deaktiviert hat.

Zweitens: Ist die App bereits geöffnet und aktiv auf dem Gerät? Wenn Ihre App die aktive App auf dem Gerät ist, werden mobile Push-Nachrichten nicht auf dem Bildschirm angezeigt. Stattdessen werden sie im Bereich &quot;Lokale Benachrichtigungen&quot;Ihrer App angezeigt.

### Anzeigen der Aktivitätsprotokolle in Marketo

Beim Nachverfolgen eines Fehlers sollten Sie zunächst in den Marketo-Aktivitätsprotokollen nachsehen. Sie können Aktivitätsprotokolle verwenden, um zu überprüfen, ob eine Nachricht gesendet wurde.

Überprüfen Sie im Aktivitätsprotokoll die Aktivitätsdatensätze einer Person, die eine Nachricht erhalten soll. Wenn die Nachricht gesendet wurde, ist ein Datensatz im Aktivitätsprotokoll vorhanden. Ist dies nicht der Fall, liegt das Problem wahrscheinlich an der Konfiguration des iOS-Zertifikats oder des Android-API-Schlüssels in Marketo.

### Zertifikat oder Schlüssel ist ungültig

Überprüfen Sie Ihre Konfiguration, um sicherzustellen, dass das richtige Zertifikat für Sandbox oder Produktion geladen wurde. Manchmal ist es am besten, wenn der Entwickler die Zertifikate (iOS) oder Schlüssel (Android) erneut exportiert und dann in Marketo neu lädt, um sicherzustellen, dass sie korrekt sind.

### .p12-Datei fehlt ein Zertifikat oder ein Schlüssel (iOS)

Stellen Sie beim Exportieren des Zertifikats sicher, dass Sie den Schlüssel _und_ des Zertifikats exportieren.

### Bereitstellung von Profilen veraltet (iOS)

Wenn Sie ein neues Gerät hinzufügen, müssen Sie Ihre Bereitstellungsprofile aktualisieren und neue Zertifikate generieren. Stellen Sie sicher, dass Ihr Xcode-Projekt dann auf die richtigen Profile und Zertifikate verweist, und importieren Sie diese Zertifikate in Marketo.

### IOS-Zertifikat (IOS) kann nicht hochgeladen werden

Stellen Sie sicher, dass das beim Exportieren des Zertifikats verwendete Kennwort keine Leerzeichen enthält.  Beispiel:

`Hello World 123`

verwenden Sie Folgendes:

`HelloWorld123`

### Fehlerbehebung für iOS-Zertifikate

Für Sandbox-Anwendungen können Sie entweder ein &quot;Entwickler&quot;- oder ein &quot;universelles&quot; Zertifikat verwenden. Für Produktionsanwendungen müssen Sie jedoch ein gültiges &quot;Distribution&quot;- oder &quot;universelle&quot; Zertifikat hochladen.

### Push-Absprung/ungültiges Token

Ein vorhandenes Anmeldetoken kann in einer Reihe von Szenarien außer Kraft gesetzt werden, darunter:

- Wenn sich die Client-App bei GCM abmeldet.
- Wenn die Client-App automatisch abgemeldet wird, was passieren kann, wenn der Benutzer die Anwendung deinstalliert. Wenn beispielsweise der APNS-Feedback-Dienst in iOS das APNS-Token als ungültig meldete.
- Wenn das Anmeldetoken abläuft. Beispielsweise könnte Google entscheiden, Registrierungstoken zu aktualisieren, oder das APNS-Token ist für iOS-Geräte abgelaufen.
- Wenn die Client-App aktualisiert wird, die neue Version jedoch nicht für den Empfang von Nachrichten konfiguriert ist.
