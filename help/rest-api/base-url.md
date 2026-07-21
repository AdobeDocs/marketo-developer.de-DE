---
title: Basis-URL
feature: REST API
description: Erfahren Sie, wie Sie Marketo-REST-API-Anfragen erstellen, den Basis-URL-Pfad und die Ressourcenparameter verstehen und Ihre eindeutige Basis-URL finden.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 146
ht-degree: 3%

---

# Basis-URL

Jeder API-Aufruf in der [Endpunktreferenz](endpoint-reference.md) gibt die REST-Methode, den Pfad, die Ressource und die Parameter an. Hängen Sie diese Komponenten an die Basis-URL an, um eine Anfrage zu erstellen.

Im Folgenden finden Sie ein Beispiel für eine gut geformte REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

Das Beispiel enthält die folgenden Komponenten:

- **Basis-URL:** `https://284-RPR-133.mktorest.com/rest`
- **path:** `/v1/lead/`
- **Ressource:** `318582.json`
- **Abfrageparameter:** `fields=email,firstName,lastName`

Die Basis-URL enthält die Konto-ID, auch als Munchkin-ID bezeichnet, und ist für jedes Marketo-Abonnement eindeutig.

Um die Basis-URL zu finden, melden Sie sich bei Marketo an und gehen Sie zu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]**. Die Basis-URL ist im Abschnitt „REST-API“ mit „Endpunkt:“ beschriftet, wie in der folgenden Abbildung dargestellt.

![Web-Services-Basis-URL-Endpunkt](assets/rest-api-base-url-web-services.png)

Kopieren Sie die Basis-URL und fügen Sie sie für jeden REST-API-Aufruf in die URL ein.
