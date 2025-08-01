---
title: Social
description: Social
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Social

Mit [Marketo Social-Marketing](https://business.adobe.com/products/marketo/social-marketing.html) können Marketing-Experten Social-Widgets in Websites und Landingpages einbetten. Zu den sozialen Widgets gehören Umfragen, Schaltflächen zum Teilen über Social Media, Videos, Gewinnspiele und Werbeaktionen wie Empfehlungsangebote.

## Beispiel für eingebettetes Freigabe-Widget

```html
<!-- Marketo Widget Loader Script -->

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget -->

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![social-share-widget](assets/social-share-widget.png)

Es gibt zwei grundlegende Methoden zum Anpassen eines Social-Media-Widgets:

1. Verwenden der normalen Benutzeroberfläche des Produkts und Anhängen von Ereignis-Listenern, um informiert zu werden, wenn bestimmte Aktionen in der Benutzeroberfläche stattgefunden haben, um zusätzliche Geschäftslogik auszuführen.
1. Ersetzen der normalen Benutzeroberfläche des Produkts durch eine benutzerdefinierte Benutzeroberfläche und Aktivieren des Popup-Fensters „Phasen“ der Benutzeroberfläche, falls gewünscht.

## Ereignisse an die normale Benutzeroberfläche anhängen

Es gibt zwei Möglichkeiten, Ereignisse in der CF JavaScript-Bibliothek global oder für ein einzelnes Widget zu abonnieren. Ereignisse werden im Folgenden in der Tabelle Ereignisse dokumentiert.

### Globales Ereignisabonnement

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

### Ereignisabonnement pro Widget

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

Dieses Beispiel zeigt ein zuvor ausgeblendetes Element mit der ID „signedUp“, nachdem ein Benutzer eine Angebotsregistrierung für ein Widget mit dem Namen „referral_SignUp“ abgeschlossen hat.

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

## Tabelle mit einfachen Ereignissen

| Ereignisname | Beschreibung | Widgets, die dieses Ereignis verwenden | Unterstützte Argumente (an die Rückruffunktion des Ereignisses übergeben) |
| --- | --- | --- | --- |
| share_sent | Wird jedes Mal ausgelöst, wenn eine Freigabeanforderung zur Verarbeitung an den Server gesendet wird | Alle Widgets, die die Möglichkeit zur Freigabe haben | 1.“share_sent“ (String)<br>2. Gesendete Parameter (Objekt) |
| share_success | Wird ausgelöst, wenn die Freigabeanfrage erfolgreich verarbeitet wurde. | Alle Widgets, die freigeben können. | 1.“share_success“ (Zeichenfolge)<br>2. Antwortobjekt freigeben, das die gesendete und gekürzte URL enthält (Objekt) |
| Stimme_Erfolg | Wird ausgelöst, wenn ein Benutzer erfolgreich an einer Abstimmung teilgenommen hat. | Umfrage-, VS-, Abstimmungs-Widgets | &#x200B;1. „vote_success“ (Zeichenfolge)<br>2. Element, für das gestimmt wurde, einschließlich Titel, Beschreibung, Entitätskennung (Objekt) |
| offer_enrolled | Wird ausgelöst, wenn ein Benutzer sich erfolgreich für ein Angebot registriert hat | Alle Angebots-Widgets | 1.“offer_enrolled“ (Zeichenfolge)<br>2. Geänderte Benutzereigenschaften (Objekt),<br>3. Geänderte Benutzerattribute (Objekt) |
| profile_saved | Wird ausgelöst, wenn ein Benutzer sein Profil aus der Profilerfassung aktualisiert hat | Alle Nicht-Angebots-Widgets, für die die Profilerfassung aktiviert ist | 1.“profile_saved„(Zeichenfolge)<br>2. Geänderte Benutzereigenschaften (Objekt)<br>3. Geänderte Benutzerattribute (Objekt) |
| video_loaded | Wird ausgelöst, wenn ein eingebettetes Video vollständig geladen und initialisiert wurde. | VideoShare-Widget | &#x200B;1. „video_loaded“ (Zeichenfolge) 2. &quot;.cf_videoshare_wrap“-Element, das das Video enthält (jQuery-Objekt) |

## Ersetzen der Benutzeroberfläche durch eine benutzerdefinierte Benutzeroberfläche

Um die Benutzeroberfläche durch eine benutzerdefinierte Benutzeroberfläche zu ersetzen, müssen Sie zunächst die normale Benutzeroberfläche deaktivieren. Dies geschieht, indem Sie die Option _popupUIOnly_ auf _true_ setzen. Wenn diese Option festgelegt ist, wird die Standard-Benutzeroberfläche beim Laden der Seite nicht gerendert. Stattdessen ruft das Widget seine Daten ab und wartet darauf, dass Sie eine seiner Popup-Phasen starten, indem Sie die Funktion _CF.widget.activate_ aufrufen und Optionen für die zu erledigenden Aufgaben angeben.

Im Folgenden finden Sie ein Beispiel für die Erstellung einer benutzerdefinierten Schaltfläche, mit der der Fluss für die Anmeldung bei Verweisangeboten für ein Verweisangebots-Widget mit dem Namen _referral_SignUp_ gestartet wird.

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

Da das Hinzufügen von Klick-Handlern häufig vorkommt, gibt es eine Kurzanleitung zum Hinzufügen. Im Folgenden finden Sie eine funktionale Entsprechung zum vorherigen Beispiel.

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

## Widget-Benutzeroberflächendaten werden in die Ersatz-Benutzeroberfläche eingefügt

Wenn Sie Daten über das Widget benötigen, um Ihre Ersatz-Benutzeroberfläche zu zeichnen, können Sie die Daten aus dem speziellen Ereignis abrufen _ui_data_. Sie können dieses Ereignis mit der normalen `CF.widget.listen` überwachen, aber dies kann eine potenzielle Race-Condition verursachen, bei der Ihr Ereignis-Listener hinzugefügt wird, nachdem das Widget bereits das Ereignis_ui_data_ ausgelöst hat, sodass Sie nie Daten erhalten. Um diesen Wettlauf zu vermeiden, verwenden Sie das `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data`-Ereignis, das ausgelöst wird, wenn eine Aktion stattgefunden hat, die dazu geführt hätte, dass die Standard-Benutzeroberfläche des Widgets neu gezeichnet wurde, auch wenn Sie diese Benutzeroberfläche mit `popupUIOnly` Option deaktiviert haben.

Ein Beispiel, das `uiData` Funktion verwendet, um die Anzahl der Einträge anzuzeigen, die ein Benutzer für ein Gewinnspiel mit dem Widget-Namen _sweeps_sweepstakes_ hat.

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

## Referenz zu Daten der Empfehlungsangebotsregistrierung-Benutzeroberfläche

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

## Datenreferenz zur Verweisangebotsverfolgung in der Benutzeroberfläche

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

## Datenreferenz zur Benutzeroberfläche für Gewinnspiele (für Gewinnspiele bei Social-Media-Kampagnen, nicht für Gewinnspiele bei LM)

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

## Datenreferenz zum Social Sign-On (Widget zum Ausfüllen von Formularen)

| Typ | Beschreibung |
|---------------|----------------------------------------------------|
| Datum | Datumswert im Format „JJJJ-MM-TT“ |
| number | Eine Ganzzahl oder Gleitkommazahl |
| RTF | Eine HTML-Zeichenfolge |
| Bewertung | Eine vorzeichenbehaftete 32-Bit-Ganzzahl |
| SFDC Campaign | Wird bei der Integration der Kampagnenverwaltung in Salesforce verwendet |
| Text | Eine Zeichenfolge |

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
