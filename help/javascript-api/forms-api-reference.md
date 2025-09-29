---
title: Forms-API-Referenz
description: Ausführliche Referenz für die Marketo Forms 2.0-API mit Details zu MktoForms2 und Formularmethoden, Parametern, Callbacks und Rückgaben zum Laden und Rendern von Formularen.
feature: Forms, Javascript
exl-id: 0f8d242f-0b27-4087-b080-3d41ebaa25b3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 2%

---

# Forms-API-Referenz

Es gibt zwei Hauptobjekte, mit denen Sie mithilfe der Forms 2.0-API interagieren werden. Das `MktoForms2` und das `Form`. Das `MktoForms2`-Objekt ist der öffentlich sichtbare Namespace der obersten Ebene für Forms2-Funktionen und enthält Funktionen zum Erstellen, Laden und Abrufen von Formularobjekten.

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
      <td>Lädt einen Formulardeskriptor von den Marketo-Servern und erstellt ein neues Formularobjekt.</td>
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
      <td>formId (Zeichenfolge oder Zahl) - Die Formularversions-ID (Vid) des zu ladenden Formulars</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Callback (optional) (Funktion) - Eine Callback-Funktion, mit der das erstellte Formularobjekt an übergeben wird, nachdem es geladen und initialisiert wurde.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.Lightbox(form, opts)</td>
      <td>Rendert ein modales Dialogfeld im Lightbox-Stil mit dem darin enthaltenen Formularobjekt.</td>
      <td>form (Formularobjekt) - Eine Instanz eines Formularobjekts, das Sie in einer Lightbox gerendert haben möchten.</td>
      <td>Ein Lightbox-Objekt mit den Methoden .show() und .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optional)(Object) - Ein Objekt mit Optionen, die an das Lightbox-Objekt übergeben werden</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Funktion) - Ein Rückruf, der ausgelöst wird, wenn das Formular übermittelt wird.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolescher Wert) default true - Steuert, ob im Lightbox-Dialogfeld eine Schaltfläche zum Schließen (X) angezeigt wird.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Erstellt ein neues Formularobjekt aus einem JS-Objekt für den Formulardeskriptor. Fügt eine Callback-Funktion hinzu, die aufgerufen wird, sobald alle Stylesheets und bekannte Lead-Informationen abgerufen und das Formularobjekt erstellt wurde.</td>
      <td>formData (Formulardeskriptorobjekt) - Ein Formulardeskriptorobjekt, wie es vom Marketo Forms V2-Editor erstellt wurde</td>
      <td>nicht definiert</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>Callback (optional) (Funktion) - Dieser Callback wird mit einem einzigen Argument aufgerufen, einer neu erstellten Instanz des Formularobjekts.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Ruft ein zuvor erstelltes Formularobjekt anhand der Formularkennung ab</td>
      <td> formId (Zahl oder Zeichenfolge) - Formular-VID-Kennung.</td>
      <td>Formularobjekt</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Ruft ein Array aller Formularobjekte ab, die zuvor auf der Seite erstellt wurden.</td>
      <td>k. A.</td>
      <td>Array von Formularobjekten</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Ruft ein JS-Objekt ab, das Daten von der URL und dem Referrer enthält, die für Tracking-Zwecke interessant sein können.</td>
      <td>k. A.</td>
      <td>Objekt</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Fügt für jedes Formular auf der Seite, das „bereit“ wird, einen Callback hinzu, der genau einmal aufgerufen wird. Bereitschaft bedeutet, dass das Formular vorhanden ist, ursprünglich gerendert wurde und seine ersten Callbacks aufgerufen wurden. Wenn zum Zeitpunkt des Aufrufs dieser Funktion bereits ein Formular vorhanden ist, wird der übergebene Callback sofort aufgerufen.</td>
      <td>callback(Funktion) - Der Callback wird an ein einzelnes Argument, ein Formularobjekt, übergeben.</td>
      <td>MktoForms2-Objekt</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Fügt einen Callback hinzu, der jedes Mal aufgerufen wird, wenn ein Formular auf der Seite gerendert wird. Forms werden gerendert, wenn sie anfänglich erstellt werden, und anschließend jedes Mal, wenn die Sichtbarkeitsregeln die Formularstruktur ändern.</td>
      <td>Callback (Funktion) - Dem Callback wird ein einzelnes Argument übergeben, das Formularobjekt des Formulars, das gerendert wurde.</td>
      <td>MktoForms2-Objekt</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Wie onFormRender fügt dies einen Callback hinzu, der jedes Mal aufgerufen wird, wenn ein Formular gerendert wird. Außerdem wird dadurch der Callback sofort für alle Formulare aufgerufen, die bereits gerendert wurden.</td>
      <td>callback(Funktion) - Der Callback wird als einzelnes Argument übergeben, das Formularobjekt des gerenderten Formulars.</td>
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
      <td>.render(formElement)</td>
      <td>Rendert ein Formularobjekt und gibt ein jQuery-Objekt zurück, das ein Formularelement einschließt, das das Formular enthält. Wenn ein formElement übergeben wird, wird es als Formularelement verwendet, andernfalls wird ein neues erstellt.</td>
      <td>formElement (optional) - Ein von einem jQuery-Objekt umschlossenes Formularelement, in das gerendert werden soll.</td>
      <td> Ein in ein jQuery-Objekt eingeschlossenes Formularelement, das das gerenderte Formular enthält.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Ruft die ID des Formulars ab.</td>
      <td>k. A.</td>
      <td>Number : Die ID des Formularobjekts, das dieses Formular darstellt</td>
    </tr>
    <tr valign="top">
      <td>.getFormElement()</td>
      <td>Ruft das in jQuery eingeschlossene Formularelement eines gerenderten Formulars ab.</td>
      <td>k. A.</td>
      <td>Ein in ein jQuery-Objekt eingeschlossenes Formularelement oder null, wenn das Formular noch nicht mit der render()-Methode gerendert wurde.</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Erzwingt die Validierung des Formulars, wobei eventuell vorhandene Fehler hervorgehoben und das Ergebnis zurückgegeben werden. Sendet das Formular nicht.</td>
      <td>k. A.</td>
      <td>Boolesch - Gibt „true“ zurück, wenn alle Validierer im Formular übergeben wurden, andernfalls „false“.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Fügt einen Validierungsrückruf hinzu, der bei jeder ausgelösten Validierung aufgerufen wird.</td>
      <td>callback(Function) - Ein Callback, der bei jeder Validierung ausgelöst wird. Dem Callback wird ein Parameter übergeben, ein boolescher Wert, der angibt, ob die Validierung erfolgreich war.</td>
      <td>Formularobjekt : Dasselbe Formularobjekt, für das die Methode zu Verkettungszwecken aufgerufen wurde.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Trigger des Übermittlungsereignisses des Formulars. Dieser startet den Übermittlungsfluss, führt die Validierung durch, löst alle onSubmit-Ereignisse aus, übermittelt das Formular und löst alle onSuccess-Ereignisse aus, wenn die Formularübermittlung erfolgreich war.</td>
      <td>k. A.</td>
      <td>Formularobjekt : Dasselbe Formularobjekt, für das die Methode zu Verkettungszwecken aufgerufen wurde.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Fügt einen Callback hinzu, der aufgerufen wird, wenn das Formular übermittelt wird. Dieser wird ausgelöst, wenn die Übermittlung beginnt, bevor der Erfolg/Misserfolg der Anfrage bekannt ist.</td>
      <td>Callback : Eine Funktion, die aufgerufen wird, wenn das Formular übermittelt wird. Diesem Callback wird ein Argument, dieses Formularobjekt, übergeben.</td>
      <td>Formularobjekt : Dasselbe Formularobjekt, für das die Methode zu Verkettungszwecken aufgerufen wurde.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Fügt einen Callback hinzu, der aufgerufen wird, wenn das Formular erfolgreich übermittelt wurde, aber bevor der Lead zur Folgeseite weitergeleitet wird. Kann verwendet werden, um zu verhindern, dass der Lead nach erfolgreicher Übermittlung an die Folgeseite weitergeleitet wird.</td>
      <td>Callback : Eine Funktion, die aufgerufen wird, wenn das Formular erfolgreich gesendet wurde. Dieser Callback wird mit zwei Argumenten weitergeleitet. Ein JS-Objekt, das die gesendeten Werte und eine String-URL der Folgeseite enthält, an die der Benutzer weitergeleitet wird, oder null oder leere Zeichenfolge, wenn keine konfigurierte Folgeseite vorhanden ist. Spezielles Verhalten: Wenn dieser Callback „false“ zurückgibt (gemessen mit ===), wird der Besucher NICHT zur Folgeseite weitergeleitet und die Seite NICHT neu geladen. Dadurch kann der Implementierer die Follow-up-URL zusätzlich verarbeiten oder eine Aktion auf der Seite mit JavaScript durchführen, anstatt die Seite zu verlassen.</td>
      <td>Formularobjekt : Dasselbe Formularobjekt, für das die Methode zu Verkettungszwecken aufgerufen wurde.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>auch verfügbar als:</em> <em>.submitable(canSubmit)</em></td>
      <td>Ruft ab oder legt fest, ob das Formular gesendet werden kann. Wird der Aufruf ohne Argumente durchgeführt, wird der Wert abgerufen. Wird er mit einem Argument aufgerufen, wird der Wert festgelegt. Damit kann verhindert werden, dass ein Formular gesendet wird, während andere Kriterien außerhalb des normalen Formulars erfüllt sein müssen.</td>
      <td>canSubmit (optional) (Boolescher Wert) - Legt das Formular als übermittelbar oder nicht übermittelbar fest.</td>
      <td>Boolescher Wert oder Formularobjekt - Wenn aufgerufen, aber ohne Argumente, wird ein boolescher Wert zurückgegeben, der angibt, ob das Formular übermittelt werden kann. Wird dieses Formularobjekt mit einem Argument aufgerufen, wird es zu Verkettungszwecken zurückgegeben. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Gibt „true“ zurück, wenn alle Felder im Formular nicht leere Werte aufweisen.</td>
      <td>k. A.</td>
      <td>Boolesch - True, wenn alle Felder nicht leere/leere/unset/null-Werte aufweisen, andernfalls „false“.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(values)</td>
      <td>Legt Werte für ein oder mehrere Felder im Formular fest.</td>
      <td>calves : Ein JS-Objekt. Für jedes Schlüssel/Wert-Paar im -Objekt wird das Formularfeld mit dem Namen Schlüssel auf Wert festgelegt.</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Ruft alle Werte aller Felder im Formular ab.</td>
      <td>k. A.</td>
      <td>Objekt - Ein JS-Objekt, das Schlüssel/Wert-Paare enthält, die die Namen und Werte der Felder im Formular darstellen.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Fügt dem Formular „input type=hidden“-Felder hinzu.</td>
      <td>Werte - Ein JS-Objekt, das Schlüssel/Wert-Paare enthält, die die Namen und Werte der ausgeblendeten Felder darstellen, die dem Formular hinzugefügt werden sollen.</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.Vals(values)</td>
      <td>jQuery-Stil .vals() Setter/Getter. Wenn aufgerufen wird, ohne Argumente, entspricht dies dem Aufruf von getValues(). Wenn mit einem Argument aufgerufen wird, entspricht dies dem Aufruf von setValues()</td>
      <td>Werte (optional) - Objekt</td>
      <td>nicht definiert</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Zeigt eine Fehlermeldung an, die auf ein Element verweist.</td>
      <td>msg (String von HTML) - Eine Zeichenfolge, die den Text des Fehlers enthält, den Sie anzeigen möchten.</td>
            <td>Formularobjekt - Dieses Formularobjekt zum Verketten.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>element (optional)(jQuery-Objekt) - Das Element, auf das der Fehler verweisen soll. Wenn diese Option deaktiviert ist, wird die Senden-Schaltfläche des Formulars verwendet.</td>
<td></td>
    </tr>
  </tbody>
</table>
