---
title: Beispiele
description: Beispiele für Marketo Forms 2.0 JavaScript zum Ausblenden oder Umleiten bei der Übermittlung, Festlegen und Lesen von Feldern, Validieren mit benutzerdefinierten Fehlern, Lightbox und externen Triggern.
feature: Javascript
exl-id: dc5f0cc5-ff5a-48b0-be36-52c10e56f798
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# Beispiele

Nachstehend finden Sie eine Reihe von Beispielen für Webformulare zu Forms 2.0.

## Formular nach erfolgreicher Übermittlung ausblenden

In diesem Beispiel wird der Besucher nicht zur Folgeseite weitergeleitet und die aktuelle Seite wird nicht neu geladen.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Get the form's jQuery element and hide it
        form.getFormElem().hide();
        // Return false to prevent the submission handler from taking the lead to the follow up url
        return false;
    });
});
```

## Besucher zur benutzerdefinierten URL führen

In diesem Beispiel wird der Besucher nach erfolgreicher Übermittlung zu einer von JavaScript bestimmten URL weitergeleitet und nicht zur konfigurierten Dankeseite.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    //Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Take the lead to a different page on successful submit, ignoring the form's configured followUpUrl
        location.href = "https://google.com/?q=marketo+forms+v2+examples";
        // Return false to prevent the submission handler continuing with its own processing
        return false;
    });
});
```

## Festlegen von Formularfeldwerten

In diesem Beispiel werden Formularfelder festgelegt.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Set the value of the Phone and Country fields
    form.vals({ "Phone":"555-555-1234", "Country":"USA"});
});
```

## Lesen von Formularfeldwerten beim Senden eines Formulars

In diesem Beispiel werden Formularfelder beim Senden des Formulars gelesen.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSubmit handler
    form.onSubmit(function(){
        // Get the form field values
        var vals = form.vals();
        // You may wish to call other function calls here, for example to fire google analytics tracking or the like
        // callSomeFunction(vals);
        // We'll just alert them to show the principle
        alert("Submitted values: " + JSON.stringify(vals));
    });
});
```

## Senden eines Formulars bei einem Nicht-Formular-Klickereignis

In diesem Beispiel wird ein Formular basierend auf einem Klickereignis für ein anderes Element oder Ereignis gesendet, das nicht Teil des Formulars ist.

```javascript
// Load the form normally
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057);

// Find the button element that you want to attach the event to
var btn = document.getElementById("MyAlternativeSubmitButtonId");
btn.onclick = function() {
    // When the button is clicked, get the form object and submit it
    MktoForms2.whenReady(function (form) {
        form.submit();
    });
};
```

## Verhindern, dass ein Benutzer ein Formular sendet

Für dieses Beispiel müssen Sie mindestens dreimal auf die Schaltfläche Zähler klicken, bevor die Schaltfläche Senden im Formular funktioniert.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Check if the form is submittable
    if (form.submittable()) {
        // Set it to be non submittable
        form.submittable(false);
    }
});

var clickCount = 0;
// Wire up the click counter button
var clickCounterBtn = document.getElementById("ClickCounter");
clickCounterBtn.onclick = function() {
    clickCount++;
    // Update the buttons's text
    clickCounterBtn.innerHTML = "Click Counter Button ("+clickCount+")";

    if (clickCount >= 3) {
        // Get the form and set it to be submittable
        MktoForms2.whenReady(function (form) {
            form.submittable(true);
        });
    }
};
```

## Festlegen von Werten für ausgeblendete Felder im Formular

In diesem Beispiel werden Werte für ausgeblendete Felder festgelegt.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Set values for the hidden fields, "userIsAwesome" and "enrollDate"
    // Note that these fields were configured in the form editor as hidden fields already
    form.vals({"userIsAwesome":"true", "enrollDate":"2014-01-01"});
});
```

## Formular in Lightbox anzeigen

Dieses Beispiel zeigt das Formular in einem Dialogfeld im Lightbox-Stil, wenn die URL einen `lightboxForm=true` enthält.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    if (location.href.indexOf("lightboxForm=true") != -1) {
        MktoForms2.lightbox(form).show();
    }
});
```

## Benutzerdefinierte Fehlermeldung anzeigen

Dieses Beispiel zeigt eine benutzerdefinierte Fehlermeldung beim Senden, die auf benutzerdefinierter Geschäftslogik basiert.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    //listen for the validate event
    form.onValidate(function() {
        // Get the values
        var vals = form.vals();
        //Check your condition
        if (vals.Country == "USA" && vals.vehicleSize != "Massive") {
            // Prevent form submission
            form.submittable(false);
            // Show error message, pointed at VehicleSize element
            var vehicleSizeElem = form.getFormElem().find("#vehicleSize");
            form.showErrorMessage("All Americans must have a massive vehicle", vehicleSizeElem);
        }
        else {
            // Enable submission for those who met the criteria
            form.submittable(true);
        }
  });
});
```
