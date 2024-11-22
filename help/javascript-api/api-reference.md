---
title: Munchkin API-Referenz
description: Verwenden Sie die Munchkin-JavaScript-API, um Ihre Munchkin-Daten anzupassen.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 9%

---

# Munchkin API-Referenz

Munchkin bietet mehrere Funktionen, die über JavaScript manuell aufgerufen werden können. Diese ermöglichen eine benutzerdefinierte Verfolgung von Browserereignissen, wie z. B. Videowiedergaben oder Klicks auf Nicht-Links.

## Funktionen

Die Munchkin-API umfasst die folgenden Funktionen: `init`, `createTrackingCookie`, `munchkinFunction`.

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` muss vor allen anderen Funktionen aufgerufen werden. Sie richtet Munchkin auf der aktuellen Seite ein, um Aktivitäten an eine bestimmte Instanz zu senden, und generiert für die aktuelle Seite die Aktivität &quot;Besuche der Webseite&quot;.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| Munchkin-ID | Erforderlich | String | Munchkin-Konto-ID gefunden unter Admin > Integration > Munchkin . Legt die Zielinstanz zum Senden von Aktivitäten fest. |
| [Konfigurationseinstellungen](configuration.md) | Optional | Objekt | Aktiviert alternative Verhaltenseinstellungen für Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Wenn aufgerufen, wird überprüft, ob ein `_mkto_trk` -Cookie im Browser vorhanden ist. Wenn nicht, wird eines erstellt. Dies ist nützlich, um Benutzer bei bestimmten Aktionen zu verfolgen, z. B. bei der Registrierung oder beim Herunterladen eines Assets, wenn `cookieAnon` auf &quot;false&quot;gesetzt ist.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| forceCreate | Erforderlich | Boolesch | Erstellen Sie ein Cookie, selbst wenn `cookieAnon` auf &quot;false&quot;gesetzt ist. |


```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Dient zum Generieren benutzerdefinierter Tracking-Verhaltensweisen wie Wiedergabe und Pausen des Videoplayers oder Seitenbesuche für nicht standardmäßige Navigation wie Hash-Codes.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| Funktionstyp | Erforderlich | String | Bestimmt die aufzuzeichnende Aktivität. Zulässige Werte: `visitWebPage`, `clickLink`, `associateLead` |
| Daten | Erforderlich | Objekt | Enthält Daten zur Aufzeichnung der Aktivität. |

#### visitWebPage

Der Aufruf von `munchkinFunction()` mit `visitWebPage` sendet eine Besuchsaktivität für den aktuellen Benutzer an Marketo. Sie können die URL und `querystring` anpassen, die mit dem Datenobjekt im zweiten Argument gesendet werden.

| Dateneigenschaftsname | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| URL | Erforderlich | String | Der URL-Dateipfad, der zum Aufzeichnen eines Seitenbesuchs verwendet wird.  Dieser Wert wird an den aktuellen Domänennamen angehängt, um den vollständigen Seitennamen zu erstellen. Wenn beispielsweise die URL `/index.html` und der Domänenname `www.example.com` ist, wird die besuchte Seite als `www.example.com/index.html` aufgezeichnet. |
| params | Optional | String | Eine Abfragezeichenfolge der gewünschten Parameter, die aufgezeichnet werden sollen. |

Beispiel: `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Der Aufruf von `munchkinFunction()` mit `clickLink` sendet eine Klickaktivität für den aktuellen Benutzer an Marketo. Sie können die Klick-URL mit der Eigenschaft `href` im Datenobjekt anpassen.

| Dateneigenschaftsname | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| href | Erforderlich | String | Der URL-Dateipfad, der zum Aufzeichnen eines Link-Klicks verwendet wird. Dieser Wert wird an den aktuellen Domänennamen angehängt, um einen vollständigen Link zu erstellen. |

Wenn href beispielsweise `/index.html` und der Domänenname `www.example.com` ist, wird der Link-Klick als `www.example.com/index.html` aufgezeichnet.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### AssociateLead

Diese Methode wird nicht mehr unterstützt und kann nicht mehr verwendet werden.
