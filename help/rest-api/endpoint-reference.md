---
title: Endpunktverweis
feature: REST API
description: Marketo API-Endpunktverweise
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---

# Endpunktverweis

- [Asset](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identität](https://developer.adobe.com/marketo-apis/api/identity/)
- [Lead-Datenbank](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Benutzerverwaltung](https://developer.adobe.com/marketo-apis/api/user/)

## Endpunktverweis

Marketo verwendet Swagger, um eine formale Definition der öffentlichen Oberfläche für seine REST-APIs bereitzustellen. Swagger bietet ein Rich-Definition-Modell für URL-Strukturen, Anforderungsmodelle und Antwortmodelle und verfügt über ein entwickeltes Tool-System zur Verwendung mit API-Interaktion, -Tests und der Client-Generierung.

Der Endpunktverweis verwendet das JavaScript-Paket [Swagger-UI](https://swagger.io/tools/swagger-ui/) , um die Referenzseiten Client-seitig zu generieren. Jeder öffentliche Endpunkt wird aufgelistet und gibt die Struktur des Antwortmodells, die erforderlichen Anforderungsparameter und ggf. das Anforderungsmodell an.

## Verwenden der Swagger-Definitionen von Marketo

Der Swagger-Standard erfordert die Bereitstellung eines Hosts oder die Erstellung des Hosts durch den Host, der die Datei bereitstellt. Es ist wichtig, den Host in der Definition zu korrigieren, bevor Sie vorhandene Tools verwenden, da Marketo einen leeren Host-Parameter mit der Definition bereitstellt. Der REST-API-Host für jede Marketo-Instanz ist eindeutig und folgt dem Muster:

`{Munchkin ID}.mktorest.com`

## Asset-APIs

Die Marketo Asset-APIs verwenden in Anfragen für Endpunkte, für die eine POST erforderlich ist, `application/x-www-url-formencoded` -Stilparameter. In einigen Fällen, z. B. bei Ordnerparametern, kann der Wert für den Parameter jedoch ein JSON-Array oder -Objekt sein. Diese Parameter werden in der Endpunktreferenz vermerkt.
