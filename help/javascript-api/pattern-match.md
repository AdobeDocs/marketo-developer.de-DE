---
title: Musterübereinstimmung
description: Verwenden Sie das Dienstprogramm RTP.checkPattern, um Zeichenfolgenmuster mit Prozentwert-Platzhaltern zu testen. Weitere Informationen finden Sie unter Synchronisierungsbeschränkungen, Verwendungs- und URL-Beispiele und unter Erforderliche RTP-Tag-Einrichtung.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 5%

---

# Musterübereinstimmung

RTP stellt eine Dienstprogramm-Funktion zur Verfügung, um zu überprüfen, ob das Muster mit bestimmten Zeichenfolgen übereinstimmt. Das Dienstprogramm kann nicht asynchron verwendet werden, da es eine Angabe zurückgibt, ob eine Übereinstimmung vorliegt oder nicht.

Sie müssen Web Personalization-Kunde werden und das [RTP-Tag ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

> rtp.checkPattern(check_against, pattern);

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
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
