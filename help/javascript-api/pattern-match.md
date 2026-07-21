---
title: Musterübereinstimmung
description: Verwenden Sie das Dienstprogramm RTP.checkPattern, um Zeichenfolgenmuster mit Prozentwert-Platzhaltern zu testen. Weitere Informationen finden Sie unter Synchronisierungsbeschränkungen, Verwendungs- und URL-Beispiele und unter Erforderliche RTP-Tag-Einrichtung.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 188
ht-degree: 5%

---

# Musterübereinstimmung

RTP stellt eine Dienstprogrammfunktion bereit, die prüft, ob ein Muster mit einer Zeichenfolge übereinstimmt. Das Dienstprogramm gibt ein Übereinstimmungsergebnis synchron zurück und kann nicht asynchron verwendet werden.

Sie müssen Web Personalization-Kunde sein und das [RTP-Tag](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site bereitstellen lassen, bevor Sie die User Context-API verwenden.

## Nutzung

> rtp.checkPattern(check_against, pattern);

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| check_against | Erforderlich | String | Zeichenfolge, mit der das Muster abgeglichen wird, z. B. die URL der aktuellen Seite oder ein Produktname. |
| pattern | Erforderlich | String | Übereinstimmendes Muster. Fügen Sie `%` als Platzhalter hinzu, um sie an den Start, das Ende oder den Inhalt einer Zeichenfolge anzupassen. `%` für vollständige Übereinstimmung auslassen. |

## Beispiele

In diesem Beispiel wird eine benutzerdefinierte Variable auf Index 1 festgelegt, wenn die aktuelle Seiten-URL mit „productA“ endet.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Im folgenden Beispiel ist der aktuelle URL-Pfad &quot;/products/productB“. Im Beispiel wird geprüft, ob der Pfad „Produkte“ enthält, und dann eine benutzerdefinierte Variable festgelegt.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
