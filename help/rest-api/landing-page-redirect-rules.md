---
title: Zielseiten-Umleitungsregeln
feature: REST API, Landing Pages
description: Verwenden Sie Marketo Asset REST-APIs zum Erstellen, Abfragen, Aktualisieren und Löschen von Umleitungsregeln für Landingpages mit Filtern, Paginierung, Host-Namen-Optionen und Nicht-Marketo-Zielen.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
TQID: https://experienceleague.adobe.com/2gePbKA3xeoRdnL8mNnObN-GPTX00Ii4-zcM0lBjs-o
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 626
ht-degree: 4%

---

# Zielseiten-Umleitungsregeln

[Endpunkt-Referenz für Umleitungsregeln für Landingpages](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules)

Verwenden Sie die REST-APIs für Landingpage-Umleitungsregeln, um Umleitungs-URLs für Landingpages abzufragen, zu erstellen, zu aktualisieren und zu löschen.

Umleitungsregeln senden eine Landingpage-URL an eine andere Seiten-URL. Quell- und Zielseiten können Marketo- oder Nicht-Marketo-Seiten sein. Die zugehörige Produktdokumentation finden Sie unter [Marketo Engage-Dokumentation](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=de).

## Abfrage

Regeln für die Umleitung von Landingpages [nach ID](#by_id) oder [Browsen](#browse).

### Nach ID

Der Endpunkt [Regeln für die Umleitung von Landingpages nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) nimmt einen `id` für die Umleitungsregel und gibt den entsprechenden Datensatz zurück.

```http
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Durchsuchen

Der Endpunkt [Regeln für Landingpage-Umleitung abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) gibt Datensätze für Umleitungsregeln für Landingpages zurück.

Verwenden Sie optionale Abfrageparameter, um die Ergebnisse zu filtern.

Der `offset` ist eine Ganzzahl, die die maximale Anzahl an zurückzugebenden Einträgen angibt (der Standardwert ist 20). Maximal 200. Der `maxReturn` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann zusammen mit dem Versatz verwendet werden (der Standardwert ist 0).

Der `hostname`-Parameter filtert nach Host-Namen der Landingpage.

Die `redirectToLandingPageId` Ganzzahl filtert nach der Ziel-Landingpage-ID. Der `redirectToPath`-Parameter filtert nach dem Pfad der Ziel-Landingpage.

Die Parameter `earliestUpdatedAt` und `latestUpdatedAt` legen die niedrigen und hohen Datums-/Uhrzeitgrenzen fest. Der Endpunkt gibt Regeln zurück, die innerhalb des Bereichs erstellt oder aktualisiert wurden.

```http
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Erstellen

Rufen Sie [ Endpunkt „Umleitungsregel für Landingpage erstellen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) mit einer `application/x-www-form-urlencoded` POST-Anfrage auf. Die Anfrage verfügt über drei erforderliche Parameter.

Der `hostname` gibt den Host-Namen der Landingpage an. Sie muss zu einer Branding-Domain oder einem Alias gehören und darf 255 Zeichen nicht überschreiten.

Der `redirectFrom` gibt die Quell-Landingpage als JSON-Objekt mit einem Typ-Wert-Paar an. Das `type`-Attribut kann für eine Marketo-Landingpage oder `path` für eine Nicht-Marketo-Seite `landingPageId` werden.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| &#39;GET&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;Besucher&#39; | Erforderlich | String | Methodenname. |
| Rückruf | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst werden soll. |

Der `redirectTo` gibt das Ziel als JSON-Objekt mit einem Typ-Wert-Paar an. Das `type`-Attribut kann für eine Marketo-Landingpage oder `url` für eine Nicht-Marketo-Seite `landingPageId` werden.

| Landingpage-Typ | redirectTo-Typ | Beispiel |
| --- | --- | --- |
| Marketo | landingPageId | {„type“:„landingPageId“,„value“:„1774“} |
| Nicht-Marketo | URL | {„type“:„url“,„value“:“www.contactLogs.com&quot;} |

Weitere Informationen finden Sie unter &quot;[ einer Marketo-Landingpage zu einer anderen Seite ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```http
POST /rest/asset/v1/redirectRules.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Aktualisierung

Der Endpunkt [Aktualisierung der Umleitungsregeln für ](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) Landingpage“ benötigt einen `id`. Senden Sie die Aktualisierung als `application/x-www-form-urlencoded` POST-Anfrage.

Übergeben Sie einen oder mehrere dieser Parameter, um die zu aktualisierenden Attribute auszuwählen: `hostname`, `redirectFrom` oder `redirectTo`.

Die Antwort gibt den aktualisierten Datensatz für die Umleitungsregel zurück.

```http
POST /rest/asset/v1/redirectRule/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Löschen

Der Endpunkt [Umleitungsregel der Landingpage nach ID löschen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) benötigt einen `id` für die Umleitungsregel.

```http
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Durchsuchen von Landingpage-Domains

Der Endpunkt [Landingpage-Domains abrufen](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) gibt Domäneneinträge der Landingpage zurück.

Verwenden Sie zwei optionale Abfrageparameter zum Filtern der Ergebnisse.

Der `offset` ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt (standardmäßig 20, maximal 200).

Der `maxReturn` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann zusammen mit `offset` verwendet werden (der Standardwert lautet 0).

```http
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
