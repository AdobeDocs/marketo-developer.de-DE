---
title: Social
description: Social
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Social

Mit [Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) können Marketing-Experten Social-Widgets in Websites und Landingpages einbetten. Social-Widgets umfassen Umfragen, Social-Sharing-Schaltflächen, Videos, Preisausschreiben und Promotions wie Werbeangebote.

## Beispiel für ein eingebettetes Freigabe-Widget

```html
<!-- Marketo Widget Loader Script --> 

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget --> 

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![social-share-widget](assets/social-share-widget.png)

Es gibt zwei grundlegende Methoden zur Anpassung eines Social-Widgets:

1. Verwenden der normalen Benutzeroberfläche des Produkts und Anhängen von Ereignis-Listenern, um informiert zu werden, wenn bestimmte Aktionen in der Benutzeroberfläche durchgeführt wurden, um zusätzliche Geschäftslogik auszuführen.
1. Ersetzen der normalen Benutzeroberfläche des Produkts durch eine benutzerdefinierte Benutzeroberfläche und Aktivieren der Popup-&quot;Phasen&quot;der Benutzeroberfläche, falls gewünscht.

## Anhängen von Ereignissen an die normale Benutzeroberfläche

Es gibt zwei Möglichkeiten, Ereignisse in der CF JavaScript-Bibliothek global oder für ein einzelnes Widget zu abonnieren. Ereignisse werden nachfolgend in der Ereignistabelle dokumentiert.

### Globale Ereignisanmeldung

```html
<script>
cf_scripts.afterload(function(){
    CF.events.listen("event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever ANY widget fires the event "event_name_here".
        }
    );
});
</script>
```

### Abonnement pro Widget-Ereignis

```html
<script>
cf_scripts.afterload(function(){
    CF.widget.listen("widget_name_here", "event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever the widget named "widget_name_here" fires the event "event_name_here".
        }
    );
});
</script>
```

## Beispiel

Dieses Beispiel zeigt ein zuvor ausgeblendetes Element mit der ID &quot;signedUp&quot;, nachdem ein Benutzer eine Angebotsregistrierung für ein Widget namens &quot;referral_SignUp&quot;abgeschlossen hat.

```html
<div id='signedUp'style='display:none; color:green;'>This is a custom message to let you know that you signed up!</div>
<div class='cf_widgetLoader cf_w_referral_SignUp'></div>

<script>
    cf_scripts.afterload(function(){
        CF.widget.listen("referral_SignUp", "offer_enrolled", function(){
        cf_jq("#signedUp").show();
    });
});
</script>
```

## Grundlegende Ereignistabelle

| Ereignisname | Beschreibung | Widgets, die dieses Ereignis verwenden | Unterstützte Argumente (an die Ereignisrückruffunktion übergeben) |
| --- | --- | --- | --- | 
| share_sent | Wird jedes Mal ausgelöst, wenn eine Freigabeanfrage zur Verarbeitung an den Server gesendet wird | Alle Widgets, die freigeben können | 1.&quot;share_sent&quot; (String)<br>2. Gesendete Parameter (Objekt) |
| share_success | Wird ausgelöst, wenn die Freigabeanfrage erfolgreich verarbeitet wurde. | Alle Widgets, die freigeben können. | 1.&quot;share_success&quot; (Zeichenfolge)<br>2. Antwortobjekt freigeben, das die gesendete Nachricht und die verkürzte URL enthält (Objekt) |
| option_success | Wird ausgelöst, wenn ein Benutzer erfolgreich in einer Umfrage abgestimmt hat. | Umfrage, VS, Abstimmungs-Widgets | 1. &quot;option_success&quot;(String)<br>2. Punkt, für den abgestimmt wurde, einschließlich Titel, Beschreibung, Entitätskennung (Objekt) |
| offer_enrolled | Wird ausgelöst, wenn sich ein Benutzer erfolgreich für ein Angebot angemeldet hat | Alle Angebots-Widgets | 1.&quot;offer_enrolled&quot; (String)<br>2. Änderung der Benutzereigenschaften (Objekt), <br>3. Geänderte Benutzerattribute (Objekt) |
| profile_saved | Wird ausgelöst, wenn ein Benutzer sein Profil über die Profilerfassung aktualisiert hat | Alle Widgets ohne Angebote, bei denen die Profilerfassung aktiviert ist | 1.&quot;profile_saved&quot; (Zeichenfolge)<br>2. Änderung der Benutzereigenschaften (Objekt)<br>3. Geänderte Benutzerattribute (Objekt) |
| video_loaded | Wird ausgelöst, wenn ein eingebettetes Video vollständig geladen und initialisiert wurde. | VideoShare-Widget | 1. &quot;video_loaded&quot;(String) 2. &quot;.cf_videoshare_wrap&quot; Element, das das Video enthält (jQuery Object) |

## Ersetzen der Benutzeroberfläche durch eine benutzerdefinierte Benutzeroberfläche

Um die Benutzeroberfläche durch eine benutzerdefinierte Benutzeroberfläche zu ersetzen, müssen Sie zunächst die normale Benutzeroberfläche deaktivieren. Dazu legen Sie die Option _popupUIOnly_ auf _true_ fest. Wenn diese Option festgelegt ist, wird die Standard-Benutzeroberfläche beim Laden der Seite nicht gerendert, sondern das Widget ruft seine Daten ab und wartet darauf, dass Sie eine seiner Popup-Phasen starten, indem es die Funktion _CF.widget.activate_ aufruft und Optionen bereitstellt, was sie tun sollte.

Im Folgenden finden Sie ein Beispiel für die Erstellung einer benutzerdefinierten Schaltfläche, mit der der Fluss zur Angebotsanmeldung für ein Referrer-Angebot-Widget namens _referral_SignUp_ gestartet wird.

```html
<button id="myNewSignUpButton">My newSign Up button</button>

<!-- Turn off the defaultreferral offer UI by setting popupUIOnly to true-->
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // After the cf script library has loaded, find the button with
    // id="myNewSignUpButton", and attach a click listener to it.
    $("#myNewSignUpButton").click(function(){
        // When it is clicked, activate the popup widget flow for the referral,
        // asking it to point to the clicked button.
        CF.widget.activate("referral_SignUp", {pointTo:$(this)});
    });
});
</script>
```

Da das Hinzufügen von Klick-Handlern häufig vorkommt, gibt es eine Verknüpfungsmethode zum Hinzufügen. Folgendes entspricht funktionell dem vorangehenden Beispiel.

```html
<button id="myNewSignUpButton">My newSign Up button</button>
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // Use the addClickActivate convenience method, which will
    // automatically make the popup point at the clicked item with id myNewSignUpButton.
    CF.widget.addClickActivate("#myNewSignUpButton", "referral_SignUp", {});
});
</script>
```

## Abrufen von Widget-UI-Daten, die in Ihre Ersatz-Benutzeroberfläche eingefügt werden sollen

Wenn Sie zum Zeichnen Ihrer Ersatz-Benutzeroberfläche Daten über das Widget benötigen, können Sie die Daten aus dem Spezialereignis _ui_data_ abrufen. Sie können dieses Ereignis mit der normalen `CF.widget.listen` -Funktion überwachen. Dies kann jedoch zu einer potenziellen Race-Bedingung führen, bei der Ihr Ereignis-Listener hinzugefügt wird, nachdem das Widget das_ui_data_ -Ereignis bereits ausgelöst hat, sodass Sie nie Daten erhalten. Um dieses Rennen zu vermeiden, verwenden Sie das Ereignis `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` , das ausgelöst wird, wenn eine Aktion durchgeführt wird, die dazu geführt hätte, dass die standardmäßige Benutzeroberfläche des Widgets neu gezeichnet wurde, selbst wenn Sie diese Benutzeroberfläche mit der Option `popupUIOnly` deaktiviert haben.

Ein Beispiel, das die Funktion `uiData` verwendet, um die Anzahl der Einträge anzuzeigen, die ein Benutzer für ein Gewinnspiel mit dem Widget-Namen _sweeps_sweepstakes_ hat.

```html
<span>You have <span id="entryCount">?</span> entries.</span>

<div class="cf_widgetLoader cf_w_sweeps_Sweepstakes"></div>

<button id='myNewSweepsButton'>New Sweeps Up Button!</button>

<script>
cf_scripts.afterload(function($, CF){
    CF.widget.uiData("sweeps_Sweepstakes", function(uiData){
        if(uiData.user && uiData.userStatus && uiData.userEntries){
            $("entryCount").html(""+ uiData.userEntries);
        }
        else{
            $("entryCount").html("0");
        }
    });
});
</script>
```

## Data Reference: Referrer Offer Sign-up UI

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert des Formulars &quot;yyyy-MM-dd&quot; |
| number | Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| sfdc campaign | Wird in der Salesforce-Kampagnenverwaltungsintegration verwendet |
| Text | Eine Textzeichenfolge |

## Referenz zu Referrer-Angebot TrackProgress-Benutzeroberflächen-Datenreferenz

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert des Formulars &quot;yyyy-MM-dd&quot; |
| number | Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| sfdc campaign | Wird in der Salesforce-Kampagnenverwaltungsintegration verwendet |
| Text | Eine Textzeichenfolge |

## Preisausschreiben in der Benutzeroberfläche - Datenreferenz (für Gewinnspiele in Social-Media-Kampagnen, nicht für LM-Preisausschreiben)

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert des Formulars &quot;yyyy-MM-dd&quot; |
| number | Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| sfdc campaign | Wird in der Salesforce-Kampagnenverwaltungsintegration verwendet |
| Text | Eine Textzeichenfolge |

## Datenreferenz für Social Sign-On (Widget zur Formularfüllung)

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert des Formulars &quot;yyyy-MM-dd&quot; |
| number | Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| sfdc campaign | Wird in der Salesforce-Kampagnenverwaltungsintegration verwendet |
| Text | Eine Textzeichenfolge |

```javascript
{
"alt_id": "http://www.facebook.com/profile.php?id=1526228678",
"provider_name": "facebook",
"default_photo_url": "https://graph.facebook.com/1526228678/picture?type=large",
"email": "ian.b.taylor@gmail.com",
"verified_email": "ian.b.taylor@gmail.com",
"gender": "male",
"preferred_user_name": "IanTaylor",
"display_name": "Ian Taylor",
"birth_date": 343954800000,
"first_name": "Ian",
"last_name": "Taylor",
"city": null,
"state": null,
"region": null,
"postal_code": null,
"country": null,
"time_zone": null,
"connection_count": 0,
"credentials": {
"uid": "1526228678",
"scopes": "publish_actions",
"expires": "1371994082",
"accessToken": "BAAGFJ0KUFpcBABuNMptmYY...",
"type": "Facebook"
},
"about_me": null,
"cur_pos_title": "Senior Staff Engineer",
"phone_number": null,
"company": "Marketo",
"cur_pos_start_date": 1333231200000,
"cur_pos_summary": null
}
```
