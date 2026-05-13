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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 157
ht-degree: 2%

---

# Basis-URL

Die [Endpunktreferenz](endpoint-reference.md) Dokumentation für jeden API-Aufruf zeigt die REST-Methode, den Pfad, die Ressource und die Parameter, die zur Erstellung einer Anfrage an die Basis-URL angehängt werden müssen.

Im Folgenden finden Sie ein Beispiel für eine gut geformte REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

das sich aus folgenden Teilen zusammensetzt:

- Basis-URL: `https://284-RPR-133.mktorest.com/rest`
- Pfad: `/v1/lead/`
- Ressource: `318582.json`
- Abfrageparameter: `fields=email,firstName,lastName`

Die Basis-URL enthält die Konto-ID (auch als Munchkin-ID bezeichnet) und ist daher für jedes Marketo-Abonnement eindeutig. Ihre Basis-URL finden Sie, indem Sie sich bei Marketo anmelden und zum Menü **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** navigieren. Sie ist unter dem Abschnitt „REST-API“ als „Endpunkt:“ gekennzeichnet, wie in den folgenden Screenshots gezeigt.

![Web-Services-Basis-URL-Endpunkt](assets/rest-api-base-url-web-services.png)

Nachdem Sie die Basis-URL gefunden haben, kopieren Sie sie und fügen Sie sie in URLs ein, die Sie beim Aufruf einer der REST-APIs verwenden.
