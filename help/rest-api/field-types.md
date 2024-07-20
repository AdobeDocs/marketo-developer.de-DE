---
title: Feldtypen
feature: REST API
description: Eine Liste der Marketo-Feldtypen
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 8%

---

# Feldtypen

Hier finden Sie eine Beschreibung der Feldtypen in Marketo. Weitere Informationen zu Feldtypen finden Sie [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Weitere Informationen zu den Feldtypbeschränkungen finden Sie [hier](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Feldtyp | Beschreibung | Beispiel |
| --- | --- | --- |
| Datum/Uhrzeit | Wird für die Eingabe eines Datums und einer Uhrzeit verwendet. Befolgt das Format [W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Es empfiehlt sich, immer den Zeitzonenversatz einzubeziehen. Vollständiges Datum plus Stunden und Minuten: JJJ-MM-DDThh:mm:ssTZD, wobei TZD &quot;+hh:mm&quot;oder &quot;-hh:mm&quot;ist Hinweis: Einige Asset-APIs geben &quot;Z+0000&quot;als TZD für updatedAt und createdAt zurück. | 15.05.2010:41:32-05:00 |
| E-Mail | Ein Zeichenfolgenfeld, das E-Mail-Adressen akzeptiert | example@example.com |
| Gleitkomma | Ein Zahlenfeld, das reale Zahlen enthält und eine Dezimalstelle verwenden kann. | 10,4 |
| Ganze Zahl | Ganzzahlen | 10 |
| Formel | Felder, deren Werte durch Manipulation von Daten aus anderen Feldern in einem Lead-Datensatz generiert werden. Sie werden nicht exportiert und können nicht in Smart-Kampagnen verwendet werden. | Siehe diesen [Artikel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) . |
| Prozent | Ein als Ganzzahl ausgedrückter Prozentsatz | 30 |
| URL | Ein Textfeld, das die Eingabe auf URLs beschränkt, einschließlich des Protokolls der URL. | http://example.com/ |
| Telefon | Telefonnummer | 111-11-1111 |
| Textbereich | Längerer Text. | Unterstützt bis zu 30.000 Byte. Standardmäßige ASCII-Zeichen verwenden 1 Byte pro Zeichen (maximal 30.000 Zeichen). Unicode-Zeichen können bis zu 4 Byte pro Zeichen verwenden (wodurch die  Anzahl der Zeichen, die weniger als 30.000 Zeichen enthalten dürfen). |
| Zeichenfolge | Kürzerer Text (bis zu 255 Zeichen) | Lorem ipsum dolor sit amet |
| Bewertung | Ein ganzzahliges Feld, das mit dem Schritt zum Ändern der Punktzahl bearbeitet werden kann | 10 |
| Boolesch (zuvor Kontrollkästchen) | Ermöglicht Benutzern die Auswahl eines True-Werts (aktiviert) oder False-Werts (deaktiviert). | Richtig |
| Währung | Ein float -Feld, das den für das Marketo-Abonnement ausgewählten Standardwährungstyp darstellt | 10,40 |
| Datum | Wird für Datum verwendet. Befolgt das W3C-Format. | 07.05.2010 |
| Referenz | Ein Zeichenfolgenfeld, das einen Schlüssel zu einem anderen Datensatz (einen Fremdschlüssel) enthält. | Unternehmen kontaktieren |
