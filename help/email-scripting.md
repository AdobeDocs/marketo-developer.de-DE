---
title: E-Mail-Skripterstellung
feature: Email Programs
description: Übersicht über die E-Mail-Skripterstellung
exl-id: ff396f8b-80c2-4c87-959e-fb8783c391bf
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '963'
ht-degree: 0%

---

# E-Mail-Skripterstellung

HINWEIS: Sie sollten dringend den [Velocity-Benutzerhandbuch](https://velocity.apache.org/engine/devel/user-guide.html) für einen tiefen Einblick in das Verhalten der Velocity-Vorlagensprache.

[Apache Velocity](https://velocity.apache.org/) ist eine auf Java basierende Sprache, die für die Vorlagenerstellung und die Skripterstellung von HTML-Inhalten entwickelt wurde. Marketo ermöglicht die Verwendung von Skript-Token im Kontext von E-Mails. Dadurch erhalten Sie Zugriff auf Daten, die in &quot;Chancen&quot;und &quot;Benutzerdefinierte Objekte&quot;gespeichert sind, und können dynamische Inhalte in E-Mails erstellen. Velocity bietet standardmäßigen Steuerungsfluss auf hoher Ebene mit if/else, for und für jede , um bedingte und iterative Bearbeitung von Inhalten zu ermöglichen. Hier ist ein einfaches Beispiel, um eine Grußformel mit der richtigen Anrede zu drucken:

```java
//check if the lead is male
if(${lead.MarketoSocialGender} == "Male")
    if the lead is male, use the salutation 'Mr.'
    set($greeting = "Dear Mr. ${lead.LastName},")
//check is the lead is female
elseif(${lead.MarketoSocialGender} == "Female")
    if female, use the salutation 'Ms.'
    set($greeting = "Dear Ms. ${lead.LastName},")
else
    //otherwise, use the first name
    set($greeting = "Dear ${lead.FirstName},")
end
print the greeting and some content
${greeting}

    Lorem ipsum dolor sit amet...
```

## Variablen

Variablen erhalten immer das Präfix &#39;$&#39; und werden mithilfe von #set festgelegt und aktualisiert:

```
#set($variable = "value")
```

Ihre Werte können dann über verschiedene Referenztypen mit unterschiedlichem Verhalten abgerufen werden:

```
$variable ##outputs 'value'
$variablename ##outputs '$variablename'
${variable}name ##outputs 'valuename'
```

Es gibt auch eine stille Referenznotation, bei der eine `!` Eingeschlossen nach der `$`. Wenn bei Geschwindigkeit ein nicht definierter Verweis auftritt, bleibt die Zeichenfolge, die den Verweis darstellt, an Ort und Stelle. Wenn bei stillen Referenznotation ein nicht definierter Verweis auftritt, wird kein Wert ausgegeben:

```
##Defined Reference

#set($foo = "bar")
$foo ##outputs "bar"

##Undefined Reference

##normal
$baz ##outputs "$baz"

##quiet
$!baz ##outputs nothing
```

Weitere Informationen zum Referenzieren von Variablen finden Sie unter [Apache-Benutzerhandbuch](https://velocity.apache.org/engine/devel/user-guide.html#formal-reference-notation).

## Velocity-Tools

Das Apache Velocity-Projekt stellt Funktionen durch die Verwendung von [Velocity-Tools](https://velocity.apache.org/tools/devel/apidocs/overview-summary.html). Diese sind lediglich Wrapper für Java-Objekte und stellen ihre Methoden über globale Variablen bereit, die für alle Skripte zur Verfügung gestellt werden.

- [AlternatorTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/AlternatorTool.html)
- [ComparisonDateTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ComparisonDateTool.html)
- [ConversionTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ConversionTool.html)
- [DateTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DateTool.html)
- [DisplayTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DisplayTool.html)
- [MathTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/MathTool.html)
- [NumberTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/NumberTool.html)
- [EscapeTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/EscapeTool.html)
- [LoopTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/LoopTool.html)

So verwenden Sie beispielsweise eine Methode aus `ComparisonDateTool`, greifen Sie auf zu, wenn über die `$date` in einem Skript-Token:

```
#set($birthday = $convert.parseDate("2015-08-07","yyyy-MM-dd"))
##use whenIs to determine how many days away it is
$date.whenIs($birthday).days ##outputs 1
```

## Skript-Token erstellen

Das Velocity-Skript wird mithilfe von E-Mail-Skript-Token in E-Mails eingeschlossen. Diese können in Marketingaktivitäten entweder in einem Marketingordner oder in einem Programm erstellt werden. Damit ein Token in einer E-Mail verwendet werden kann, muss es sich bei der E-Mail um ein untergeordnetes Element eines Programms handeln, das entweder Eigentümer des Tokens ist oder es von einem Marketing-Ordner übernimmt. Um ein Token zu erstellen, navigieren Sie zu einem Ordner oder Programm und wählen Sie die [!UICONTROL Meine Token] Registerkarte. Ziehen Sie aus dem Kontextmenü die Option &quot;E-Mail-Script&quot;in die Token-Liste

![Skript-Token](assets/script-token.png)

Von hier aus können Sie den Namen des Tokens bearbeiten und den Editor über die [!UICONTROL Klicken zum Bearbeiten] Option:

![Skript bearbeiten](assets/script-edit.png)

Sobald Sie sich im Editor befinden, können Sie ein Skript mit Zugriff auf alle Variablen in Objekten erstellen, auf die über Skripte zugegriffen werden kann. Um eine Feldreferenz aus einem Objekt zu erhalten, ziehen Sie sie aus der rechten Baumstruktur in das Skript:

![Skript-Token bearbeiten](assets/edit-script-token.png)

## Einbetten und Testen von Skripten

Nachdem Sie Ihr Skript in einem Programm-My Token definiert haben, können Sie es mit dem Marketo-E-Mail-Editor in einer bestimmten E-Mail referenzieren.

![Email Script](assets/email-script-marketo-email.png)

Sie können Ihr Skript mithilfe der [!UICONTROL Beispiel-E-Mail senden] E-Mail-Aktion im Marketo-E-Mail-Designer. Damit das Skript ordnungsgemäß verarbeitet werden kann, müssen Sie einen vorhandenen Lead auswählen, der im [!UICONTROL Lead] -Feld. Wenn Sie mit `$TriggerObject`, können Sie das auslösende Objekt über die [!UICONTROL Trigger] param. Dabei werden die Daten des zuletzt aktualisierten Objekts dieses Typs als `$TriggerObject` -Variable.

![E-Mail-Skript testen](assets/velocity-test.png)

Sie können auch die [!UICONTROL E-Mail-Vorschau] , um Ihr Skript zu testen. Wählen Sie dazu **[!UICONTROL Anzeigen als: Lead-Detail]** und wählen Sie einen Lead aus einer verfügbaren statischen Liste aus. Dies hat den zusätzlichen Vorteil, dass Ausnahmen ausgegeben werden, die möglicherweise während der Skriptausführung aufgetreten sind:

![E-Mail anzeigen als](assets/view-as.png)

## Nützliche Hinweise

Die Gesamtlänge aller E-Mail-Skript-Token in einer E-Mail darf 100.000 Byte nicht überschreiten. Diese Begrenzung bezieht sich auf die Gesamtlänge der Token-Zeichenfolgen selbst (nicht auf die Gesamtlänge nach der Erweiterung der Token).

- Die im E-Mail-Skript referenzierten Variablen müssen in Marketo auf einem der dem Skript verfügbaren Objekte vorhanden sein.
- Sie können benutzerdefinierte Objekte der ersten und zweiten Ebene referenzieren, die aus Ihrem nativ integrierten CRM stammen und direkt mit dem Lead oder Kontakt, aber nicht mit benutzerdefinierten Objekten der dritten Ebene verbunden sind. Benutzerdefinierte Objekte sind möglicherweise nicht die übergeordneten Elemente des Leads oder Unternehmens
- Bei benutzerdefinierten Marketo-Objekten können Sie auf benutzerdefinierte Objekte der zweiten Ebene mit einer übergeordneten untergeordneten Beziehung verweisen. Beispiel `Lead <- Parent <- Child`. Benutzerdefinierte Objekte der zweiten Ebene können nicht mit der Beziehung zwischen Edge und Bridge referenziert werden. Beispiel:  `Lead <- Bridge -> Edge`
- Sie können auf benutzerdefinierte Objekte verweisen, die mit einem Lead, Kontakt oder einem Konto verbunden sind, jedoch nicht mit mehreren.
- Benutzerdefinierte Objekte können nur über eine Verbindung, Lead, Kontakt oder Konto referenziert werden
- Sie müssen das Kontrollkästchen im Skript-Editor für die Felder aktivieren, die Sie verwenden, oder sie werden nicht verarbeitet
- Für jedes benutzerdefinierte Objekt sind die zehn zuletzt aktualisierten Datensätze pro Person/Kontakt zur Laufzeit verfügbar und werden von der letzten Aktualisierung (0) bis zur ältesten Aktualisierung (9) geordnet. Sie können die Anzahl der verfügbaren Datensätze erhöhen durch [den Anweisungen folgen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).
- Wenn Sie mehr als ein E-Mail-Skript in eine E-Mail einfügen, werden diese von oben nach unten ausgeführt. Der Bereich der Variablen, die im ersten auszuführenden Skript definiert sind, ist in nachfolgenden Skripten verfügbar.
- Tools-Referenz: [https://velocity.apache.org/tools/2.0/index.html](https://velocity.apache.org/tools/2.0/index.html)
- Ein Hinweis zu Token, die Zeilenumbruchzeichen &quot;\\n&quot;oder &quot;\\r\\n&quot;enthalten. Wenn eine E-Mail über das Senden-Beispiel oder eine Batch-Kampagne gesendet wird, werden Zeilenumbrüche in Token durch Leerzeichen ersetzt. Wenn E-Mails über Trigger Campaign gesendet werden, bleiben Zeilenumbruchzeichen unverändert.
- Um eine korrekte Analyse von URLs sicherzustellen, sollte der gesamte Pfad als Variable festgelegt und dann gedruckt und die Variable nicht in URL-Verweisen gedruckt werden. Das Protokoll (http:// oder https://) muss eingeschlossen und vom Rest der URL getrennt sein. Die URL muss auch Teil eines vollständig gebildeten Ankers sein (<a>). Das Skript muss ein vollständig geformtes Anker-Tag ausgeben, damit Links verfolgt werden können. Links werden nicht verfolgt, wenn sie aus einer for - oder foreach -Schleife ausgegeben werden.

```html
<!-- Correct -->
#set($url = "www.example.com/${object.id}")
<a href="http://${url}">Link Text</a>

<!-- Correct -->
<a href="http://www.example.com/${object.id}">Link Text</a>

<!-- Incorrect -->
<a href="${url}">Link Text</a>

<!-- Incorrect -->
<a href="{{my.link}}">Link Text</a>

<!-- Incorrect -->
<a href="http://{{my.link}}">Link Text</a>
```
