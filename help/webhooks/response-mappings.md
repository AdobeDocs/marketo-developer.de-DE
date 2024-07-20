---
title: Response Mappings
feature: Webhooks
description: Reaktionszuordnungen für Marketo
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 1%

---

# Response Mappings

Marketo kann von einem Webhook empfangene Daten von zwei Inhaltstypen übersetzen und diese Werte zurück in ein Lead-Feld zurückgeben: JSON und XML. Der Marketo-Feldparameter verwendet immer den [SOAP API-Namen](../rest-api/fields.md) des Felds. Jeder Webhook kann über eine unbegrenzte Anzahl von Antwortzuordnungen verfügen, die hinzugefügt und bearbeitet werden, indem Sie auf die Schaltfläche [!UICONTROL Bearbeiten] im Bereich &quot;Antwortzuordnungen&quot;Ihres Webhooks klicken:

![Response-Mapping](assets/response-mapping.png)

Antwortzuordnungen werden über eine Kopplung von &quot;Response Attribute&quot;, dem Pfad zur gewünschten Eigenschaft im XML- oder JSON-Dokument und dem &quot;Marketo Field&quot; erstellt, das das Lead-Feld angibt, in das der Wert aus dem Response-Attribut geschrieben wurde.

Die Schlüssel für Eigenschaften müssen aus alphanumerischen Zeichen, Bindestrichen (-), Unterstrichen(_), Doppelpunkten (:) und Leerzeichen bestehen, auf die über Marketo-Antwortzuordnungen zugegriffen werden kann.

## JSON-Zuordnungen

Auf JSON-Eigenschaften kann mit Punkt- und Array-Notation zugegriffen werden. Die Array-Notation in Marketo akzeptiert keine Zeichenfolgen als Eingabe und akzeptiert nur Ganzzahlen. Um Daten aus einem JSON-Dokument abzurufen, muss der Antworttyp auf JSON festgelegt sein:

```json
{ "foo":"bar"}
```

Um in einer Antwortzuordnung auf die Eigenschaft `foo` zuzugreifen, verwenden Sie die Eigenschaft `name` , da sie sich in der ersten Ebene des JSON-Objekts befindet, `foo`. So sieht das in Marketo aus:

![Antwortzuordnung](assets/json-resp.png)

Im Folgenden finden Sie ein komplizierteres Beispiel mit einem Array:

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

Wir möchten über das erste Element des Auftrags-Arrays auf das orderDate zugreifen. Um auf diese Eigenschaft zuzugreifen, verwenden Sie Folgendes: `orders[0].orderDate`

## XML-Zuordnungen

Auf Werte kann von einzelnen Elementen in XML-Dokumenten zugegriffen werden. Hierbei wird Punktnotation verwendet, die den JSON-Zuordnungen ähnelt. Sehen wir uns dieses einfache Beispiel an:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Um hier auf die foo-Eigenschaft zuzugreifen, verwenden Sie Folgendes: `example.foo`

Das Beispielelement muss zuerst referenziert werden, bevor auf `foo` zugegriffen werden kann. Um auf eine Eigenschaft zugreifen zu können, müssen alle Elemente in der Hierarchie in der Zuordnung referenziert werden. XML-Dokumente mit Arrays sind etwas komplizierter. Verwenden Sie das folgende Beispiel:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

Das Dokument besteht aus dem übergeordneten Array `elementList` mit untergeordneten Elementen, das eine Eigenschaft enthält: `foo`. Für die Marketo-Antwortzuordnungen wird das Array als `elementList.element` referenziert, sodass auf die untergeordneten Elemente der elementList über `elementList.element[i]` zugegriffen wird. Um den Wert von foo vom ersten untergeordneten Element von elementList zu erhalten, verwenden wir dieses Antwortattribut: `elementList.element[0].foo` Dies gibt den Wert &quot;baz&quot;an unser dafür vorgesehenes Feld zurück. Der Versuch, auf Eigenschaften innerhalb von Elementen zuzugreifen, die sowohl eindeutige als auch nicht eindeutige Elementnamen enthalten, führt zu undefiniertem Verhalten. Jedes Element muss eine einzelne Eigenschaft oder ein Array sein. Die Typen können nicht miteinander kombiniert werden.

## Typen

Beim Zuordnen von Attributen zu Feldern müssen Sie sicherstellen, dass der Typ in Ihrer Webhook-Antwort mit dem Zielfeld kompatibel ist. Wenn der Wert in der Antwort beispielsweise eine Zeichenfolge ist und das ausgewählte Feld vom Typ &quot;integer&quot;ist, wird der Wert nicht geschrieben. Lesen Sie mehr über [Feldtypen](../rest-api/field-types.md).
