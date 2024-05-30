---
title: "Pattern Match"
description: "Pattern Match"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 6%

---


# Musterübereinstimmung

RTP stellt eine Dienstprogrammfunktion bereit, um zu überprüfen, ob das Muster mit bestimmten Zeichenfolgen übereinstimmt. Das Dienstprogramm kann nicht asynchron verwendet werden, da es eine Meldung zurückgibt, ob eine Übereinstimmung vorliegt oder nicht.

Sie müssen Web Personalization-Kunde werden und über die [RTP-Tag bereitgestellt](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) auf Ihrer Site vor der Verwendung der User Context-API.

## Nutzung

> rtp.checkPattern(check_against, pattern);

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| check_between | Erforderlich | Zeichenfolge | Zeichenfolge, mit der das Muster übereinstimmt. Beispiel: aktuelle Seiten-URL, Produktname. |
| pattern | Erforderlich | Zeichenfolge | % für Platzhalter hinzufügen. Das Muster kann wie folgt lauten: start with contains full match |


## Beispiele

Legen Sie die benutzerdefinierte Variable in Index 1 fest, wenn die aktuelle Seiten-URL mit &quot;productA&quot;endet.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Der aktuelle URL-Pfad lautet &quot;/products/productB&quot;. In diesem Beispiel wird geprüft, ob der Pfad &quot;products&quot;enthält und eine benutzerdefinierte Variable festlegt.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
