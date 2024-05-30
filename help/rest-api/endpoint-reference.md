---
title: "Endpoint-Referenz"
feature: REST API
description: "Marketo API-Endpunktverweise"
source-git-commit: 2454f126dc4275697ef6773420453ad8853eae73
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

Der Endpunktverweis verwendet die [Swagger-UI](https://swagger.io/tools/swagger-ui/) JavaScript-Paket zum Generieren der Referenzseiten auf der Clientseite. Jeder öffentliche Endpunkt wird aufgelistet und gibt die Struktur des Antwortmodells, die erforderlichen Anforderungsparameter und ggf. das Anforderungsmodell an.

## Verwenden der Swagger-Definitionen von Marketo

Der Swagger-Standard erfordert die Bereitstellung eines Hosts oder die Erstellung des Hosts durch den Host, der die Datei bereitstellt. Es ist wichtig, den Host in der Definition zu korrigieren, bevor Sie vorhandene Tools verwenden, da Marketo einen leeren Host-Parameter mit der Definition bereitstellt. Der REST-API-Host für jede Marketo-Instanz ist eindeutig und folgt dem Muster:

`{Munchkin ID}.mktorest.com`

## Asset-APIs

Die Marketo Asset-APIs verwenden `application/x-www-url-formencoded` -Stilparameter in -Anforderungen für -Endpunkte, für die eine POST-Methode erforderlich ist. In einigen Fällen, z. B. bei Ordnerparametern, kann der Wert für den Parameter jedoch ein JSON-Array oder -Objekt sein. Diese Parameter werden in der Endpunktreferenz vermerkt.
