---
title: Zielseiten-Umleitungsregeln
feature: REST API, Landing Pages
description: Konfigurieren von Umleitungsregeln für Landingpages über die API.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 4%

---

# Zielseiten-Umleitungsregeln

[Endpunkt-Referenz für Umleitungsregeln für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von CRUD-Vorgängen für Landingpage-Umleitungs-URLs. Diese APIs folgen dem Standard-Schnittstellenmuster für Asset-APIs und bieten Optionen zum Abfragen, Erstellen, Aktualisieren und Löschen.

Regeln für die Umleitung von Landingpages bieten die Möglichkeit, eine Landingpage-URL zu einer anderen Seiten-URL umzuleiten. Sie können Marketo-Landingpages, Landingpages von Drittanbietern von Marketo oder Kombinationen daraus umleiten. Weitere Informationen zu den Regeln für Umleitungs-Landingpages finden Sie [hier](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=de).

## Abfrage

Die Abfrage von Umleitungsregeln für Landingpages folgt den Standardabfragetypen für Assets von [nach ID](#by_id) und [Browsen](#browse).

### Nach ID

Der Endpunkt [Regeln für die Landingpage-Umleitung nach ID abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) nimmt `id` Pfadparameter eine einzelne Landingpage-Regel-Umleitung und gibt einen einzelnen Landingpage-Umleitungsregel-Datensatz zurück.

```
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

Der Endpunkt [Regeln für Landingpage-Umleitung abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) gibt eine Liste von Datensätzen für Landingpage-Umleitungsregeln zurück.

Es gibt mehrere optionale Abfrageparameter, die an die Filterergebnisse übergeben werden können.

Der `offset` ist eine Ganzzahl, die die maximale Anzahl an zurückzugebenden Einträgen angibt (der Standardwert ist 20). Maximal 200. Der `maxReturn` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann zusammen mit dem Versatz verwendet werden (der Standardwert ist 0).

Der `hostname` Parameter kann verwendet werden, um nach dem Host-Namen der Landingpages zu filtern.

Die `redirectToLandingPageId` ist eine Ganzzahl, die zum Filtern nach der ID der Landingpage verwendet werden kann, zu der Sie umleiten. Die `redirectToPath` kann verwendet werden, um nach dem Pfad der Landingpages zu filtern, zu denen Sie umleiten.

Mit den Parametern `earliestUpdatedAt` und `latestUpdatedAt` können Sie Wasserzeichen für niedrige und hohe Datums- und Uhrzeitwerte für zurückgegebene Umleitungsregeln für Landingpages festlegen, die entweder aktualisiert oder innerhalb des angegebenen Bereichs anfänglich erstellt wurden.

```
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

Der Endpunkt [Umleitungsregel für Landingpage erstellen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) wird mit einem application/x-www-form-urlencoded POST ausgeführt, der die folgenden drei erforderlichen Parameter aufweist.

Der Parameter `hostname` gibt den Host-Namen für die Landingpage an. Dieser sollte zu einer Branding-Domain oder einem Alias gehören. Die maximale Länge beträgt 255 Zeichen.

Der `redirectFrom` gibt die Quell-Landingpage an. Dies ist ein JSON-Objekt, das ein Typ-Wert-Paar enthält, das bestimmt, ob es sich bei der Quelle um eine Marketo-Landingpage oder eine Nicht-Marketo-Landingpage handelt. Das `type`-Attribut kann entweder „landingPageId“ oder „path“ sein.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| &#39;GET&#39; | Erforderlich | String | Aktion der Methode. |
| &#39;Besucher&#39; | Erforderlich | String | Methodenname. |
| Rückruf | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst werden soll. |

Der `redirectTo` gibt die Ziel-Landingpage an. Dies ist ein JSON-Objekt, das ein Typ-Wert-Paar enthält, das bestimmt, ob es sich bei der Quelle um eine Marketo-Landingpage oder eine Nicht-Marketo-Landingpage handelt. Das `type`-Attribut kann entweder „landingPageId“ oder „url“ sein.

| Landingpage-Typ | redirectTo-Typ | Beispiel |
|---|---|---|
| Marketo | landingPageId | {„type“:„landingPageId“,„value“:„1774“} |
| Nicht-Marketo | URL | {„type“:„url“,„value“:“www.contactLogs.com&quot;} |

Weitere Informationen zum Erstellen von Umleitungsregeln für Landingpages finden Sie [hier](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

## Update

Der Endpunkt [Aktualisierung der Umleitungsregeln für ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) Landingpage“ akzeptiert eine einzige Umleitungsregel für Landingpages `id` den Pfadparameter . Dieser Endpunkt wird mit einem application/x-www-form-urlencoded POST ausgeführt.

Wie beim oben beschriebenen create-Aufruf werden einer oder mehrere der folgenden Abfrageparameter übergeben, um anzugeben, welches Attribut der Regel aktualisiert werden soll: `hostname`, `redirectFrom`, `redirectTo`.

Der aktualisierte Datensatz für die Umleitungsregel der Landingpage wird in der Antwort zurückgegeben.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Der Endpunkt [Umleitungsregel für Landingpage nach ID löschen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) verwendet einen einzelnen Parameter für die Umleitung `id` Landingpage-Regeln.

```
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

Der Endpunkt [Landingpage-Domains abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) gibt eine Liste von Landingpage-Domain-Einträgen zurück.

Es gibt zwei optionale Abfrageparameter, die zum Filtern von Ergebnissen übergeben werden können.

Der `offset` ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt (standardmäßig 20, maximal 200).

Der `maxReturn` ist eine Ganzzahl, die angibt, wo mit dem Abrufen von Einträgen begonnen werden soll. Kann zusammen mit `offset` verwendet werden (der Standardwert lautet 0).

```
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
