---
title: Forms API-Referenz
description: Forms API-Referenz
feature: Forms, Javascript
exl-id: 0f8d242f-0b27-4087-b080-3d41ebaa25b3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 2%

---

# Forms API-Referenz

Es gibt zwei Hauptobjekte, mit denen Sie mit der Forms 2.0-API interagieren. Das Objekt `MktoForms2` und das Objekt `Form`. Das Objekt `MktoForms2` ist der öffentlich sichtbare Namespace der obersten Ebene für die Forms2-Funktionalität und enthält Funktionen zum Erstellen, Laden und Abrufen von Formularobjekten.

## MktoForms2-Methoden

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Methode</strong></td>
      <td><strong>Beschreibung</strong></td>
      <td><strong>Parameter</strong></td>
      <td><strong>Rückgaben</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, callback)</td>
      <td>Lädt einen Formulardeskriptor von Marketo-Servern und erstellt ein neues Formularobjekt.</td>
      <td> baseUrl(String) - URL zur Marketo-Serverinstanz für Ihr Abonnement</td>
      <td>nicht definiert</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (String) -Munchkin-ID des Abonnements</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (Zeichenfolge oder Zahl) - Die Formular-Version-ID (Vid) des Formulars, das geladen werden soll</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (optional) (Funktion) - Eine Rückruffunktion, die das konstruierte Formularobjekt nach dem Laden und Initialisieren an weitergibt.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(form, opts)</td>
      <td>Gibt ein modales Dialogfeld mit Lightbox-Stil-Stil mit dem darin enthaltenen Formularobjekt wieder.</td>
      <td>form (Form Object) - Eine Instanz eines Formularobjekts, das Sie in einer Lightbox wiedergeben möchten.</td>
      <td>Ein Lightbox-Objekt mit den Methoden .show() und .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optional)(Object) - Ein Objekt von Optionen, die an das Lightbox-Objekt übergeben werden</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Function) - Ein Callback, der beim Senden des Formulars ausgelöst wird.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true - Steuert, ob eine Schließen-Schaltfläche (X) im Lightbox-Dialogfeld angezeigt wird.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Erstellt ein neues Formularobjekt aus einem Formulardeskriptor-JS-Objekt. Fügt eine Callback-Funktion hinzu, die aufgerufen wird, sobald alle Stylesheets und bekannten Lead-Informationen abgerufen und das Formularobjekt erstellt wurde.</td>
      <td>formData (Form Descriptor Object) - Ein Formulardeskriptorobjekt, wie vom Marketo Forms V2-Editor erstellt</td>
      <td>nicht definiert</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (optional)(Funktion) - Dieser Rückruf wird mit einem einzelnen Argument aufgerufen, einer neu erstellten Instanz des Formularobjekts.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Ruft ein zuvor erstelltes Formularobjekt anhand der Formular-ID ab</td>
      <td> formId (Zahl oder Zeichenfolge) - Formular-ID-Kennung.</td>
      <td>Formularobjekt</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Ruft ein Array aller Formularobjekte ab, die zuvor auf der Seite erstellt wurden.</td>
      <td>Nicht zutreffend</td>
      <td>Array von Formularobjekten</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Ruft ein JS-Objekt ab, das Daten aus der URL und dem Referrer enthält, die für Tracking-Zwecke interessant sein können.</td>
      <td>Nicht zutreffend</td>
      <td>Objekt</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Fügt einen Rückruf hinzu, der genau einmal für jedes Formular auf der Seite aufgerufen wird, das "bereit"wird. "Bereitschaft"bedeutet, dass das Formular vorhanden ist, ursprünglich wiedergegeben wurde und die ersten Rückrufe aufgerufen wurden. Wenn bereits ein Formular vorhanden ist, das zum Zeitpunkt des Aufrufs dieser Funktion bereit ist, wird der übergebene Rückruf sofort aufgerufen.</td>
      <td>callback(Function) - Der Rückruf wird an ein einzelnes Argument übergeben, ein Formularobjekt.</td>
      <td>MktoForms2-Objekt</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Fügt einen Rückruf hinzu, der jedes Mal aufgerufen wird, wenn ein Formular auf der Seite gerendert wird. Forms werden bei der anfänglichen Erstellung wiedergegeben und jedes Mal, wenn diese Sichtbarkeitsregeln die Struktur des Formulars ändern.</td>
      <td>callback (Funktion) - Der Rückruf wird an ein einzelnes Argument übergeben, das Formularobjekt des wiedergegebenen Formulars.</td>
      <td>MktoForms2-Objekt</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Wie onFormRender fügt dies einen Callback hinzu, der jedes Mal aufgerufen wird, wenn ein Formular wiedergegeben wird. Darüber hinaus wird dadurch auch der Rückruf sofort für alle Formulare aufgerufen, die bereits wiedergegeben wurden.</td>
      <td>callback(Function) - Der Rückruf wird an ein einzelnes Argument übergeben, das Formularobjekt des wiedergegebenen Formulars.</td>
      <td></td>
    </tr>
</table>


## Formularmethoden

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Methode</strong></td>
      <td><strong>Beschreibung</strong></td>
      <td><strong>Parameter</strong></td>
      <td><strong>Rückgaben</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Gibt ein Formularobjekt wieder und gibt ein jQuery-Objekt zurück, das ein Formularelement umschließt, das das Formular enthält. Wenn ein formElement übergeben wird, wird dieses als Formularelement verwendet, andernfalls wird ein neues erstellt.</td>
      <td>formElement (optional) - Ein jQuery-Objekt-umschlossenes Formularelement, in das gerendert werden soll.</td>
      <td> Ein mit einem jQuery-Objekt umschlossenes Formularelement, das das wiedergegebene Formular enthält.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Ruft die ID des Formulars ab.</td>
      <td>Nicht zutreffend</td>
      <td>Number - Die ID des Formularobjekts, das dieses Formular darstellt</td>
    </tr>
    <tr valign="top">
      <td>.getFormElem()</td>
      <td>Ruft das umschlossene jQuery-Formularelement eines wiedergegebenen Formulars ab.</td>
      <td>Nicht zutreffend</td>
      <td>Ein mit einem jQuery-Objekt umschlossenes Formularelement oder null , wenn das Formular noch nicht mit der render()-Methode wiedergegeben wurde.</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Erzwingt die Validierung des Formulars, markiert etwaige Fehler und gibt das Ergebnis zurück. Sendet das Formular nicht.</td>
      <td>Nicht zutreffend</td>
      <td>Boolesch - Gibt "true"zurück, wenn alle Validatoren im Formular übergeben wurden, andernfalls "false".</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Fügt einen Validierungs-Callback hinzu, der jedes Mal aufgerufen wird, wenn eine Validierung ausgelöst wird.</td>
      <td>callback(Function) - Ein Callback, der jedes Mal ausgelöst wird, wenn eine Überprüfung erfolgt. Der Rückruf wird an einen Parameter übergeben, ein boolescher Wert, der angibt, ob die Überprüfung erfolgreich war.</td>
      <td>Formularobjekt: Das gleiche Formularobjekt, für das die Methode aufgerufen wurde, zu Verkettungszwecken.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Trigger des Sendeintereignisses des Formulars. Dadurch wird der Formularübermittlungsfluss gestartet, eine Überprüfung durchgeführt, alle onSubmit-Ereignisse ausgelöst, das Formular gesendet und alle onSuccess-Ereignisse ausgelöst, wenn die Formularübermittlung erfolgreich war.</td>
      <td>Nicht zutreffend</td>
      <td>Formularobjekt: Das gleiche Formularobjekt, für das die Methode aufgerufen wurde, zu Verkettungszwecken.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Fügt einen Rückruf hinzu, der beim Senden des Formulars aufgerufen wird. Dies wird ausgelöst, wenn die Übermittlung beginnt, bevor der Erfolg/Misserfolg der Anfrage bekannt ist.</td>
      <td>callback - Eine Funktion, die aufgerufen wird, wenn das Formular gesendet wird. Dieser Rückruf wird an ein Argument übergeben, dieses Form -Objekt.</td>
      <td>Formularobjekt: Das gleiche Formularobjekt, für das die Methode aufgerufen wurde, zu Verkettungszwecken.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Fügt einen Rückruf hinzu, der aufgerufen wird, wenn das Formular erfolgreich gesendet wurde, aber bevor der Lead an die Nachverfolgungsseite weitergeleitet wird. Kann verwendet werden, um zu verhindern, dass der Lead nach erfolgreicher Übermittlung an die Nachverfolgungsseite weitergeleitet wird.</td>
      <td>callback - Eine Funktion, die aufgerufen wird, wenn das Formular erfolgreich gesendet wurde. Dieser Rückruf wird zwei Argumente übergeben. Ein JS-Objekt, das die gesendeten Werte und eine String-URL der Nachverfolgungsseite enthält, an die der Benutzer weitergeleitet wird, oder eine Null- oder leere Zeichenfolge, wenn keine konfigurierte Nachverfolgungsseite vorhanden ist. Sonderverhalten: Wenn dieser Rückruf "false"(gemessen mit ====) zurückgibt, wird der Besucher NICHT an die Folgenachricht weitergeleitet und die Seite wird NICHT neu geladen. Auf diese Weise kann der Implementor zusätzliche Verarbeitungsschritte für die Follow-up-URL durchführen oder Aktionen auf der Seite mit JavaScript durchführen, anstatt die Seite zu verlassen.</td>
      <td>Formularobjekt: Das gleiche Formularobjekt, für das die Methode aufgerufen wurde, zu Verkettungszwecken.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>auch verfügbar als:</em> <em>.submitable(canSubmit)</em></td>
      <td>Ruft ab oder legt fest, ob das Formular gesendet werden kann. Wenn sie ohne Argumente aufgerufen wird, erhält sie den Wert. Wenn sie mit einem Argument aufgerufen wird, setzt sie den Wert. Damit kann verhindert werden, dass ein Formular gesendet wird, während andere Kriterien außerhalb des normalen Formulars erfüllt sein müssen.</td>
      <td>canSubmit (optional) (Boolesch) - Legt fest, dass das Formular gesendet werden kann oder nicht gesendet werden kann.</td>
      <td>Boolesch oder Formularobjekt: Wenn ohne Argumente aufgerufen wird, wird ein boolescher Wert zurückgegeben, der angibt, ob das Formular übermittelt werden kann. Wenn mit einem -Argument aufgerufen wird, wird dieses Formularobjekt zu Verkettungszwecken zurückgegeben. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Gibt "true"zurück, wenn für alle Felder im Formular Werte festgelegt sind, die nicht leer sind.</td>
      <td>Nicht zutreffend</td>
      <td>Boolesch - True , wenn alle Felder nicht leere/leere/unset/null-Werte aufweisen, andernfalls false .</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Legt Werte für ein oder mehrere Felder im Formular fest.</td>
      <td>vals - Ein JS-Objekt. Für jedes Schlüssel/Wert-Paar im Objekt wird das Formularfeld mit dem Namen key auf value festgelegt.</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Ruft alle Werte aller Felder im Formular ab.</td>
      <td>Nicht zutreffend</td>
      <td>Objekt - Ein JS-Objekt, das Schlüssel/Wert-Paare enthält, die die Namen und Werte der Felder im Formular darstellen.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Fügt dem Formular "input type=hidden"-Felder hinzu.</td>
      <td>values - Ein JS-Objekt, das Schlüssel/Wert-Paare enthält, die die Namen und Werte der ausgeblendeten Felder darstellen, die zum Formular hinzugefügt werden sollen.</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.vals(values)</td>
      <td>jQuery style .vals() setter/getter. Wenn ohne Argumente aufgerufen wird, entspricht dem Aufruf von getValues(). Wenn mit einem -Argument aufgerufen wird, entspricht dem Aufruf von setValues()</td>
      <td>values (optional) - Object</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Zeigt eine Fehlermeldung mit einem Verweis auf das Element an.</td>
      <td>msg (Zeichenfolge der HTML) - Eine Zeichenfolge, die den Text des anzuzeigenden Fehlers enthält.</td>
            <td>Formularobjekt - Dieses Formularobjekt zum Verketten.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (optional)(jQuery Object) - Das Element, auf das der Fehler verweisen soll. Wenn diese Option deaktiviert ist, wird die Senden-Schaltfläche des Formulars verwendet.</td>
<td></td>
    </tr>
  </tbody>
</table>
