---
title: Feldtypen
feature: REST API
description: Umfassende Liste der Marketo-Feldtypen mit Definitionen, Beispielen und Formaten, einschließlich ISO 8601-Datum/Uhrzeit, Textbereichsbeschränkungen, Währung und boolescher Wert.
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
TQID: https://experienceleague.adobe.com/Q-L1NCCS1caYip-niSrBAkp6k37ErzmsLCFvn7fRJW0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2: id: ad89fb33-8541-4339-afe7-bb13d1633714
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 371
ht-degree: 8%

---

# Feldtypen

In der folgenden Tabelle werden die in Marketo verfügbaren Feldtypen beschrieben. Weitere Informationen finden Sie unter [Glossar für benutzerdefinierte Feldtypen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary) und [Marketo-Feldbeschränkungen nach Feldtyp](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Feldtyp | Beschreibung | Beispiel |
| --- | --- | --- |
| Datum/Uhrzeit | Wird für die Eingabe von Datum und Uhrzeit verwendet. Folgt [W3C-Format](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Best Practice ist es, den Zeitzonenversatz einzubeziehen. Abschlussdatum plus Stunden und Minuten: JJJJ-MM-TThh:mm:ssTZD, wobei TZD &quot;+hh:mm&quot; oder &quot;-hh:mm&quot; ist Hinweis: Einige Asset-APIs geben „Z+0000“ als TZD für `updatedAt` und `createdAt` zurück. | 07.05.201015:41:32-05:00 |
| E-Mail | Ein Zeichenfolgenfeld, das E-Mail-Adressen akzeptiert | <example@example.com> |
| Gleitkomma | Ein Zahlenfeld, das reelle Zahlen enthält und eine Dezimalstelle verwenden kann. | 10,4 |
| Ganzzahl | Ganzzahlen | 10 |
| Formel | Felder, deren Werte durch Bearbeiten von Daten aus anderen Feldern in einem Lead-Datensatz generiert werden. Sie werden nicht exportiert und können nicht in Smart-Kampagnen verwendet werden. | Siehe diesen [Artikel](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Prozent | Ein als Ganzzahl ausgedrückter Prozentsatz | 30 |
| URL | Ein Textfeld, das die Eingabe auf URLs einschränkt, einschließlich des Protokolls der URL. | <http://example.com/> |
| Telefon | Telefonnummer | 111-111-1111 |
| Textbereich | Längerer Text. | Unterstützt bis zu 30.000 Byte. Standard-ASCII-Zeichen verwenden 1 Byte pro Zeichen (bis zu 30.000 Zeichen). Unicode-Zeichen können bis zu 4 Byte pro Zeichen verwenden (wodurch die zulässige Zeichenanzahl auf weniger als 30.000 Zeichen reduziert wird). |
| String | Kürzerer Text | Text mit bis zu 255 Zeichen |
| Ergebnis | Ein ganzzahliges Feld, das mit dem Schritt Score-Fluss ändern bearbeitet werden kann | 10 |
| Boolesch (zuvor Checkbox) | Ermöglicht Benutzern die Auswahl eines Werts vom Typ True (aktiviert) oder False (deaktiviert). | True |
| Währung | Ein schwebendes Feld, das den für das Marketo-Abonnement ausgewählten Standardwährungstyp darstellt | 10,40 |
| Datum | Für Datum verwendet. Folgt W3C-Format. | 2010-05-07 |
| Referenz | Ein Zeichenfolgenfeld, das den Schlüssel zu einem anderen Datensatz (einem Fremdschlüssel) enthält. | Unternehmen kontaktieren |
