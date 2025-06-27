---
title: Feldtypen
feature: REST API
description: Eine Liste von Marketo-Feldtypen
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: fc9b9037986a35036dbd909339f59bd33aa67e71
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 9%

---

# Feldtypen

Im Folgenden finden Sie eine Beschreibung der Feldtypen in Marketo. Weitere Informationen zu Feldtypen finden Sie [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Weitere Informationen zu Feldtypbeschränkungen finden Sie [hier](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Feldtyp | Beschreibung | Beispiel |
| --- | --- | --- |
| Datum/Uhrzeit | Wird für die Eingabe von Datum und Uhrzeit verwendet. Folgt [W3C-Format](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Best Practice ist es, den Zeitzonenversatz einzubeziehen. Abschlussdatum plus Stunden und Minuten: JJJJ-MM-TThh:mm:ssTZD, wobei TZD &quot;+hh:mm“ oder &quot;-hh:mm“ ist Hinweis: Einige Asset-APIs geben „Z+0000“ als TZD für `updatedAt` und `createdAt` zurück. | 07.05.201015:41:32-05:00 |
| E-Mail | Ein Zeichenfolgenfeld, das E-Mail-Adressen akzeptiert | example@example.com |
| Gleitkomma | Ein Zahlenfeld, das reelle Zahlen enthält und eine Dezimalstelle verwenden kann. | 10,4 |
| Ganzzahl | Ganzzahlen | 10 |
| Formel | Felder, deren Werte durch Bearbeiten von Daten aus anderen Feldern in einem Lead-Datensatz generiert werden. Sie werden nicht exportiert und können nicht in Smart-Kampagnen verwendet werden. | Siehe diesen [Artikel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Prozent | Ein als Ganzzahl ausgedrückter Prozentsatz | 30 |
| URL | Ein Textfeld, das die Eingabe auf URLs einschränkt, einschließlich des Protokolls der URL. | http://example.com/ |
| Telefon | Telefonnummer | 111-111-1111 |
| Textbereich | Längerer Text. | Unterstützt bis zu 30.000 Byte. Standard-ASCII-Zeichen verwenden 1 Byte pro Zeichen (bis zu 30.000 Zeichen). Unicode-Zeichen können bis zu 4 Byte pro Zeichen verbrauchen (wodurch der  Anzahl der Zeichen (maximal 30.000 Zeichen). |
| String | Kürzerer Text | Text mit bis zu 255 Zeichen |
| Ergebnis | Ein ganzzahliges Feld, das mit dem Schritt Score-Fluss ändern bearbeitet werden kann | 10 |
| Boolesch (zuvor Checkbox) | Ermöglicht Benutzern die Auswahl eines Werts vom Typ True (aktiviert) oder False (deaktiviert). | True |
| Währung | Ein schwebendes Feld, das den für das Marketo-Abonnement ausgewählten Standardwährungstyp darstellt | 10,40 |
| Datum | Für Datum verwendet. Folgt W3C-Format. | 07.05.2010 |
| Referenz | Ein Zeichenfolgenfeld, das den Schlüssel zu einem anderen Datensatz (einem Fremdschlüssel) enthält. | Unternehmen kontaktieren |
