---
title: EXL-Vorschautest
description: Beispiele für die Adobe EXL-Markdown-Syntax zum Testen der Erweiterungsvorschau.
source-git-commit: 8f7ff2e1b6d0a4d8f63affb7bd1a2d0abbcc118c
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 9%

---


# EXL-Vorschautest

## Warnungsblöcke

>[!NOTE]
>
>Dies ist eine Notiz. Benutzen Sie Hinweise für zusätzliche Informationen, die der Leser kennen sollte.

>[!TIP]
>
>Das ist ein Tipp. Tipps für optionale, aber hilfreiche Informationen.

>[!IMPORTANT]
>
>Dies ist ein wichtiger Warnhinweis. Benutzen Sie die Informationen, die der Leser nicht übersehen darf.

>[!WARNING]
>
>Dies ist eine Warnung. Verwenden Sie , um Informationen zu potenziellen Problemen zu erhalten.

>[!CAUTION]
>
>Das ist eine Vorsicht. Verwenden Sie , um Informationen über potenzielle Risiken zu erhalten.

>[!ADMIN]
>
>Dies ist ein Admin-Warnhinweis. Für Inhalte, die nur von Administratoren bzw. Administratorinnen erstellt werden.

>[!AVAILABILITY]
>
>Dies ist ein Verfügbarkeitshinweis. Details zur Funktionsverfügbarkeit.

>[!PREREQUISITES]
>
>Dies ist ein Block mit Voraussetzungen. Listen Sie auf, was der Leser benötigt, bevor Sie beginnen.

## Schattenboxen

>[!BEGINSHADEBOX „Optionaler Titel“]

Dieser Inhalt wird mit einem grauen Hintergrund angezeigt. Verwenden Sie Schattierungsfelder, um verwandte Inhalte visuell zu gruppieren.

Sie können Listen einschließen:

- Element eins
- Punkt zwei
- Punkt drei

>[!ENDSHADEBOX]

>[!BEGINSHADEBOX]

Schattierungsfeld ohne Titel.

>[!ENDSHADEBOX]

## Reduzierbare Abschnitte

+++Zum Erweitern klicken - einfaches Beispiel

Dieser Inhalt wird ausgeblendet, bis der Benutzer den Titel auswählt.

Sie können hier beliebige Inhalte einfügen, einschließlich Codeblöcken:

```javascript
const example = 'hello world';
console.log(example);
```

+++

+++Erweiterte Konfiguration

Verwenden Sie ausblendbare Abschnitte für optionale oder erweiterte Inhalte, die sonst den Hauptfluss überladen würden.

| Einstellung | Wert | Beschreibung |
| --- | --- | --- |
| timeout | 30 | Sekunden vor Zeitüberschreitung der Anfrage |
| Weitere Versuche | 3 | Anzahl der Wiederholungsversuche |

{style="table-layout:auto"}

+++

## Eingebettetes Video

>[!VIDEO](https://video.tv.adobe.com/v/3427028/?quality=12&learn=on)

## Lokalisierungsmakros

Verwenden Sie [!DNL Marketo], um Produktnamen so einzuschließen, dass sie nicht lokalisiert sind.

Verwenden Sie **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** für Benutzeroberflächenelement-Kennzeichnungen.

Kombinationsbeispiel: Wählen Sie in [!DNL Adobe Analytics] **[!UICONTROL Workspace]** > **[!UICONTROL Projekt erstellen]**.

## Abzeichen

[!BADGE Beta]{type=Informative}

[!BADGE Allgemein verfügbar]{type=Positive}

[!BADGE Veraltet]{type=Negative}

[!BADGE experimentell]{type=Caution}

## Registerkarten

>[!BEGINTABS]

>[!TAB Anforderungen]

>[!IMPORTANT]
>
>Sie müssen über Administratorrechte verfügen, um diese Aufgabe durchzuführen.

Erforderliche Felder:

| Feld | Typ | Erforderlich |
| --- | --- | --- |
| Name | Zeichenfolge | Ja |
| E-Mail | Zeichenfolge | Ja |
| Rolle | Aufzählung | Nein |

>[!TAB Schritte]

1. Öffnen Sie die Admin Console.
1. Wählen Sie **[!UICONTROL Benutzer]** > **[!UICONTROL Benutzer hinzufügen]** aus.
1. Füllen Sie die erforderlichen Felder aus.
1. Wählen Sie **[!UICONTROL Speichern]** aus.

>[!NOTE]
>
>Änderungen werden sofort wirksam.

>[!TAB Ergebnis]

Der neue Benutzer erhält eine Begrüßungs-E-Mail mit einem Link zum Festlegen seines Kennworts.

- Link läuft nach 24 Stunden ab.
- Benutzer können einen neuen Link über die Anmeldeseite anfordern.

>[!ENDTABS]

## Codeblöcke

```json
{
  "name": "example",
  "version": "1.0.0",
  "enabled": true
}
```

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tabellen

| Spalte 1 | Spalte 2 | Spalte 3 |
| --- | --- | --- |
| Zeile 1, Zelle 1 | Zeile 1, Zelle 2 | Zeile 1, Zelle 3 |
| Zeile 2, Zelle 1 | Zeile 2, Zelle 2 | Zeile 2, Zelle 3 |
