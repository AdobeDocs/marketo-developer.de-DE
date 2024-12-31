---
title: Musterübereinstimmung
description: Musterübereinstimmung
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 7%

---

# Musterübereinstimmung

RTP stellt eine Dienstprogramm-Funktion zur Verfügung, um zu überprüfen, ob das Muster mit bestimmten Zeichenfolgen übereinstimmt. Das Dienstprogramm kann nicht asynchron verwendet werden, da es eine Angabe zurückgibt, ob eine Übereinstimmung vorliegt oder nicht.

Sie müssen Web Personalization-Kunde werden und das [RTP-Tag ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

> rtp.checkPattern(check_against, pattern);

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| check_against | Erforderlich | String | Zeichenfolge, mit der das Muster abgeglichen wird. Beispiel: aktuelle Seiten-URL, Produktname. |
| pattern | Erforderlich | String | % für Platzhalter hinzufügen. Das Muster kann sein: Start mit Ende mit enthält vollständige Übereinstimmung |


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
