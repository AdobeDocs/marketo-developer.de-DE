---
title: Basis-URL
feature: REST API
description: Beschreibt die URLs für Marketo.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Basis-URL

Die Dokumentation [Endpunktverweis](endpoint-reference.md) für jeden API-Aufruf enthält die REST-Methode, den Pfad, die Ressource und die Parameter, die an die Basis-URL angehängt werden müssen, um eine Anfrage zu erstellen.

Im Folgenden finden Sie ein Beispiel für eine korrekt formatierte REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

die aus folgenden Teilen besteht:

- Basis-URL: `https://284-RPR-133.mktorest.com/rest`
- Pfad: `/v1/lead/`
- Ressource: `318582.json`
- Abfrageparameter: `fields=email,firstName,lastName`

Die Basis-URL enthält die Konto-ID (auch Munchkin-ID genannt) und ist daher für jedes Marketo-Abonnement eindeutig. Ihre Basis-URL wird gefunden, indem Sie sich bei Marketo anmelden und zum Menü **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Webdienste]** navigieren. Sie wird unter dem Abschnitt &quot;REST API&quot;als &quot;Endpoint:&quot;bezeichnet, wie in den folgenden Screenshots dargestellt.

![Basis-URL-Endpunkt der Web-Services](assets/rest-api-base-url-web-services.png)

Nachdem Sie die Basis-URL gefunden haben, kopieren Sie sie und fügen Sie sie in URLs ein, die Sie beim Aufrufen von REST-APIs verwenden.
