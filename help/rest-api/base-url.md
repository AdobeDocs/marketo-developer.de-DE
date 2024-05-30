---
title: "Basis-URL"
feature: REST API
description: "Beschreibt die URLs für Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# Basis-URL

Die [Endpunktverweis](endpoint-reference.md) -Dokumentation für jeden API-Aufruf zeigt die REST-Methode, den Pfad, die Ressource und die Parameter, die an die Basis-URL angehängt werden müssen, um eine Anfrage zu erstellen.

Im Folgenden finden Sie ein Beispiel für eine korrekt formatierte REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

die aus folgenden Teilen besteht:

- Basis-URL: `https://284-RPR-133.mktorest.com/rest`
- Pfad: `/v1/lead/`
- Ressource: `318582.json`
- Abfrageparameter: `fields=email,firstName,lastName`

Die Basis-URL enthält die Konto-ID (auch Munchkin-ID genannt) und ist daher für jedes Marketo-Abonnement eindeutig. Ihre Basis-URL wird gefunden, indem Sie sich bei Marketo anmelden und zur **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web-Services]** Menü. Sie wird unter dem Abschnitt &quot;REST API&quot;als &quot;Endpoint:&quot;bezeichnet, wie in den folgenden Screenshots dargestellt.

![Basis-URL-Endpunkt der Web-Services](assets/rest-api-base-url-web-services.png)

Nachdem Sie die Basis-URL gefunden haben, kopieren Sie sie und fügen Sie sie in URLs ein, die Sie beim Aufrufen von REST-APIs verwenden.
