---
title: Musterübereinstimmung
description: Verwenden Sie das Dienstprogramm RTP.checkPattern, um Zeichenfolgenmuster mit Prozentwert-Platzhaltern zu testen. Weitere Informationen finden Sie unter Synchronisierungsbeschränkungen, Verwendungs- und URL-Beispiele und unter Erforderliche RTP-Tag-Einrichtung.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 6%

---

# Musterübereinstimmung

RTP stellt eine Dienstprogramm-Funktion zur Verfügung, um zu überprüfen, ob das Muster mit bestimmten Zeichenfolgen übereinstimmt. Das Dienstprogramm kann nicht asynchron verwendet werden, da es eine Angabe zurückgibt, ob eine Übereinstimmung vorliegt oder nicht.

Sie müssen Web Personalization-Kunde werden und das [RTP-Tag ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

> rtp.checkPattern(check_against, pattern);

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| check_against | Erforderlich | String | Zeichenfolge, mit der das Muster abgeglichen wird. Beispiel: aktuelle Seiten-URL, Produktname. |
| pattern | Erforderlich | String | % für Platzhalter hinzufügen. Das Muster kann sein:start wobei Ende mit vollständige Übereinstimmung enthält. |

## Beispiele

Benutzerdefinierte Variable in Index 1 festlegen, wenn die URL der aktuellen Seite mit „productA“ endet.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Der aktuelle URL-Pfad lautet &quot;/products/productB“. In diesem Beispiel wird überprüft, ob der Pfad „Produkte“ enthält, und die benutzerdefinierte Variable wird festgelegt.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
