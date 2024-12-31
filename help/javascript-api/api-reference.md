---
title: Munchkin-API-Referenz
description: Verwenden Sie die Munchkin-JavaScript-API, um Ihre Munchkin-Daten anzupassen.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 9%

---

# Munchkin-API-Referenz

Munchkin bietet mehrere Funktionen, die manuell über JavaScript aufgerufen werden können. Diese können eine benutzerdefinierte Nachverfolgung von Browser-Ereignissen wie Videowiedergaben oder Klicks auf Nicht-Links ermöglichen.

## Funktionen

Die Munchkin-API besteht aus den folgenden Funktionen: `init`, `createTrackingCookie`, `munchkinFunction`.

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

Beim Aufruf von wird überprüft, ob ein `_mkto_trk`-Cookie im Browser vorhanden ist. Andernfalls wird ein Cookie erstellt. Dies ist nützlich, um Benutzende während bestimmter Aktionen wie der Registrierung oder dem Herunterladen eines Assets zu verfolgen, wenn `cookieAnon` auf „false“ gesetzt ist.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| forceCreate | Erforderlich | Boolesch | Erstellen Sie ein Cookie, selbst wenn `cookieAnon` auf „false“ gesetzt ist. |


```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Wird zum Generieren benutzerdefinierter Tracking-Verhaltensweisen verwendet, z. B. Wiedergaben und Pausen von Video-Playern oder Seitenbesuche für nicht standardmäßige Navigation, z. B. Hash-Codes.

| Parametername | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| Funktionstyp | Erforderlich | String | Bestimmt die aufzuzeichnende Aktivität. Zulässige Werte: `visitWebPage`, `clickLink`, `associateLead` |
| Daten | Erforderlich | Objekt | Enthält Daten für die aufzuzeichnende Aktivität. |

#### visitWebPage

Der Aufruf von `munchkinFunction()` mit `visitWebPage` sendet eine Aktivität des Typs „Besuch“ für den aktuellen Benutzer an Marketo. Sie können die URL und `querystring` anpassen, die mit dem Datenobjekt im zweiten Argument gesendet werden.

| Name der Dateneigenschaft | Optional/Erforderlich | Typ | Beschreibung |
| --- | --- | --- | --- |
| URL | Erforderlich | String | Der zum Aufzeichnen eines Seitenbesuchs verwendete URL-Dateipfad.  Dieser Wert wird an den aktuellen Domain-Namen angehängt, um den vollständigen Seitennamen zu erstellen. Wenn beispielsweise die URL `/index.html` und der Domain-Name `www.example.com` ist, wird die besuchte Seite als `www.example.com/index.html` aufgezeichnet. |
| Parameter | Optional | String | Eine Abfragezeichenfolge der gewünschten aufzuzeichnenden Parameter. |

Beispiel: `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Der Aufruf von `munchkinFunction()` mit `clickLink` sendet eine Klick-Aktivität für den aktuellen Benutzer an Marketo. Sie können die Klick-URL mit der Eigenschaft `href` im Datenobjekt anpassen.

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
