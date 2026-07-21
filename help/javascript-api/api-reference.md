---
title: Munchkin-API-Referenz
description: Verwenden Sie die Munchkin-JavaScript-API, um Seitenbesuche, Link-Klicks und benutzerdefinierte Ereignisse mit den Methoden init, createTrackingCookie und munchkinFunction zu verfolgen.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
TQID: https://experienceleague.adobe.com/s97x6wVZijnnxZwS7HMIkQAKlxXkcfPXuSZG4KjXGoc
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 414
ht-degree: 9%

---

# Munchkin-API-Referenz

Munchkin bietet JavaScript-Funktionen zum benutzerdefinierten Tracking von Browser-Ereignissen. Sie können beispielsweise Videowiedergaben oder Klicks auf Elemente verfolgen, die keine Links sind.

## Funktionen

Die Munchkin-API umfasst die folgenden Funktionen:

- `init`
- `createTrackingCookie`
- `munchkinFunction`

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` muss vor allen anderen Funktionen aufgerufen werden. Dadurch wird Munchkin auf der aktuellen Seite so eingerichtet, dass Aktivitäten an eine bestimmte Instanz gesendet werden, und es wird für die aktuelle Seite die Aktivität „Besuche auf Web-Seiten“ generiert.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| Munchkin-ID | Erforderlich | String | Die Munchkin-Konto-ID befindet sich im Menü Admin > Integration > Munchkin . Legt die Zielinstanz zum Senden von Aktivitäten an fest. |
| [Konfigurationseinstellungen](configuration.md) | Optional | Objekt | Aktiviert alternative Verhaltenseinstellungen für Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

`Munchkin.createTrackingCookie()` prüft, ob ein `_mkto_trk` Cookie im Browser vorhanden ist. Wenn das Cookie nicht vorhanden ist, erstellt die Funktion ein Cookie.

Wenn `cookieAnon` auf „false“ gesetzt ist, können Sie mit dieser Funktion Benutzer während bestimmter Aktionen verfolgen, z. B. beim Registrieren oder Herunterladen eines Assets.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| forceCreate | Erforderlich | Boolesch | Erstellen Sie ein Cookie, selbst wenn `cookieAnon` auf „false“ gesetzt ist. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Verwenden Sie `Munchkin.munchkinFunction()`, um benutzerdefinierte Tracking-Verhaltensweisen zu erstellen. Beispielsweise können Sie die Aktivität des Video-Players oder Seitenbesuche über nicht standardmäßige Navigation wie Hash-Änderungen verfolgen.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| Funktionstyp | Erforderlich | String | Bestimmt die aufzuzeichnende Aktivität. Zulässige Werte: `visitWebPage`, `clickLink`, `associateLead` |
| Daten | Erforderlich | Objekt | Enthält Daten für die aufzuzeichnende Aktivität. |

#### visitWebPage

Der Aufruf von `munchkinFunction()` mit `visitWebPage` sendet eine Aktivität des Typs „Besuch“ für den aktuellen Benutzer an Marketo. Verwenden Sie das Datenobjekt im zweiten Argument, um die URL und die `querystring` anzupassen.

| Name der Dateneigenschaft | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| URL | Erforderlich | String | Der zum Aufzeichnen eines Seitenbesuchs verwendete URL-Dateipfad.  Dieser Wert wird an den aktuellen Domain-Namen angehängt, um den vollständigen Seitennamen zu erstellen. Wenn beispielsweise die URL `/index.html` und der Domain-Name `www.example.com` ist, wird die besuchte Seite als `www.example.com/index.html` aufgezeichnet. |
| params | Optional | String | Eine Abfragezeichenfolge der gewünschten aufzuzeichnenden Parameter. |

Beispiel: `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Der Aufruf von `munchkinFunction()` mit `clickLink` sendet eine Klick-Aktivität für den aktuellen Benutzer an Marketo. Verwenden Sie die `href`-Eigenschaft im Datenobjekt, um die Klick-URL anzupassen.

| Name der Dateneigenschaft | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| href | Erforderlich | String | Der zur Aufzeichnung eines Link-Klicks verwendete URL-Dateipfad. Dieser Wert wird an den aktuellen Domain-Namen angehängt, um einen vollständigen Link zu erstellen. |

Wenn beispielsweise href `/index.html` und der Domain-Name `www.example.com` ist, wird der Link-Klick als `www.example.com/index.html` aufgezeichnet.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### AssociateLead

Diese Methode ist veraltet und nicht mehr verfügbar.
