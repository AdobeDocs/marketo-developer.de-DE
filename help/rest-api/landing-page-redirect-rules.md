---
title: "Umleitungsregeln für Landingpages"
feature: REST API, Landing Pages
description: "Konfigurieren Sie die Umleitungsregeln für Landingpages über die API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 3%

---


# Zielseiten-Umleitungsregeln

[Endpunkt-Referenz zu Umleitungsregeln für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo bietet eine Reihe von REST-APIs zum Ausführen von CRUD-Vorgängen für Landingpage-Umleitungs-URLs. Diese APIs entsprechen dem Standardmuster der Benutzeroberfläche für Asset-APIs, die Optionen zum Abfragen, Erstellen, Aktualisieren und Löschen bereitstellen.

Mit Umleitungsregeln für Landingpages können Sie eine Landingpage-URL zu einer anderen Seiten-URL umleiten. Sie können Marketo-Landingpages, Nicht-Marketo-Landingpages oder Kombinationen davon umleiten. Weitere Informationen zu Regeln für die Umleitung von Landingpages finden Sie unter [here](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=de).

## Anfrage

Die Abfrage von Umleitungsregeln für Landingpages folgt den standardmäßigen Abfragetypen für Assets von [by id](#by_id), und [Browsen](#browse).

### Nach ID

Die [Abrufen von Umleitungsregeln für Landingpages nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) Endpunkt benötigt eine Umleitung von einer einzelnen Landingpage-Regel `id` Pfadparameter und gibt einen einzelnen Landingpage-Umleitungsregeldatensatz zurück.

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

Die [Landingpage-Umleitungsregeln abrufen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) endpoint gibt eine Liste von Landingpage-Umleitungsregeldatensätzen zurück.

Es gibt mehrere optionale Abfrageparameter, die zum Filtern von Ergebnissen übergeben werden können.

Die `offset` -Parameter ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt (der Standardwert ist 20). Maximal 200. Die `maxReturn` -Parameter ist eine Ganzzahl, die angibt, wo Einträge abgerufen werden sollen. Kann zusammen mit dem Versatz verwendet werden (standardmäßig 0).

Die `hostname` kann verwendet werden, um nach dem Hostnamen der Landingpages zu filtern.

Die `redirectToLandingPageId` ist eine Ganzzahl, die zum Filtern der Kennung der Landingpage verwendet werden kann, zu der Sie umleiten. Die `redirectToPath` kann verwendet werden, um nach dem Pfad der Landingpages zu filtern, zu denen Sie umleiten.

Die `earliestUpdatedAt` und `latestUpdatedAt` Mit -Parametern können Sie Wasserzeichen mit niedrigem und hohem Datum für die Rückgabe von Landingpage-Umleitungsregeln festlegen, die entweder innerhalb des angegebenen Bereichs aktualisiert oder ursprünglich erstellt wurden.

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

Die [Erstellen einer Umleitungsregel für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) Endpunkt wird mit einer application/x-www-form-urlencoded -POST ausgeführt, die über die folgenden drei erforderlichen Parameter verfügt.

Die `hostname` gibt den Hostnamen für die Landingpage an. Dies sollte zu einer Branding-Domäne oder einem Alias gehören. Die maximale Länge beträgt 255 Zeichen.

Die `redirectFrom` gibt die Quell-Landingpage an. Dies ist ein JSON-Objekt, das ein Typ/Wert-Paar enthält, das bestimmt, ob es sich bei der Quelle um eine Marketo-Landingpage oder eine Nicht-Marketo-Landingpage handelt. Die `type` -Attribut kann entweder &quot;landingPageId&quot;oder &quot;path&quot;lauten.

| Parameter | Optional/Erforderlich | Typ | Beschreibung |
|---|---|---|---|
| &#39;get&#39; | Erforderlich | Zeichenfolge | Methodenaktion |
| &#39;visitor&#39; | Erforderlich | Zeichenfolge | Name der Methode. |
| callback | Erforderlich | Funktion | Callback-Funktion, die für jede zurückgegebene Kampagne ausgelöst wird. |


Die `redirectTo` -Parameter gibt die Ziel-Landingpage an. Dies ist ein JSON-Objekt, das ein Typ/Wert-Paar enthält, das bestimmt, ob es sich bei der Quelle um eine Marketo-Landingpage oder eine Nicht-Marketo-Landingpage handelt. Die `type` -Attribut kann entweder &quot;landingPageId&quot;oder &quot;url&quot;lauten.

| Landingpage-Typ | redirectTo-Typ | Beispiel |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Nicht Marketo | URL | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Weitere Informationen zum Erstellen von Umleitungsregeln für Landingpages finden Sie unter [here](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

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

## Aktualisierung

Die [Aktualisieren von Umleitungsregeln für Landingpages](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) Endpunkt verwendet eine einzige Umleitungsregel für Landingpages `id` Pfadparameter. Dieser Endpunkt wird mit einer application/x-www-form-urlencoded -POST ausgeführt.

Wie beim oben beschriebenen Erstellungsaufruf werden mindestens ein der folgenden Abfrageparameter übergeben, um anzugeben, welches Attribut der Regel aktualisiert werden soll: `hostname`, `redirectFrom`, `redirectTo`.

In der Antwort wird der aktualisierte Datensatz der Umleitungsregel der Landingpage zurückgegeben.

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

Die [Löschen der Umleitungsregel der Einstiegsseite nach ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) Endpunkt benötigt eine Umleitung von einer einzelnen Landingpage-Regel `id` Pfadparameter.

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

## Domänen von Landingpages durchsuchen

Die [Abrufen von Landingpage-Domänen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) endpoint gibt eine Liste von Landingpage-Domäneneinträgen zurück.

Es gibt zwei optionale Abfrageparameter, die zum Filtern von Ergebnissen übergeben werden können.

Die `offset` -Parameter ist eine Ganzzahl, die die maximale Anzahl der zurückzugebenden Einträge angibt (standardmäßig 20, maximal 200).

Die `maxReturn` -Parameter ist eine Ganzzahl, die angibt, wo Einträge abgerufen werden sollen. Kann zusammen mit `offset` (Standardwert ist 0).

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
