---
title: Fehler-Codes
feature: REST API
description: Erfahren Sie mehr über die Fehlerbehandlung in der Marketo REST-API mit HTTP 413 und 414, Antwort 6xx 7xx, Status auf Datensatzebene, Best Practices für die Protokollierung, Wiederholungen und Einschränkungen.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
TQID: https://experienceleague.adobe.com/-bV6fjqJ8RkIBGX6gpVKMjGX1qYXR2g7VK3efpEQLDM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: cc72dcf1-72e1-48cc-b434-e7c27d62d67c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2255
ht-degree: 4%

---

# Fehler-Codes

Marketo-REST-APIs geben Fehler auf HTTP-, Antwort- oder Datensatzebene zurück. Auf dieser Seite werden die einzelnen Fehlertypen erläutert und die zugehörigen Fehler-Codes aufgelistet.

## Behandeln und Protokollieren von Ausnahmen

Anfragen und Antworten protokollieren, wenn bei Ihrer Integration eine unerwartete Ausnahme auftritt. Einige Ausnahmen, z. B. abgelaufene Authentifizierungen, können durch eine erneute Authentifizierung behandelt werden. Für andere Ausnahmen ist möglicherweise die Unterstützung des Supports erforderlich, der die zugehörigen Anfrage- und Antwortdetails anfordert.

## Fehlertypen

Die Marketo-REST-API kann während des normalen Betriebs drei Fehlertypen zurückgeben:

- **HTTP-Ebene:** Wird durch einen `4xx`-Code angezeigt.
- **Response-Level:** im Array „errors“ der JSON-Antwort enthalten.
- **Record-Level:** Im Array „result“ der JSON-Antwort enthalten und für jeden Datensatz durch das Feld „status“ und das Array „reason“ angezeigt.

Fehler auf Antwort- und Datensatzebene geben den HTTP-Status-Code 200 zurück. Bewerten Sie die HTTP-Ursachenphrase nicht für alle Fehlertypen, da sie optional ist und sich ändern kann.

### Fehler auf HTTP-Ebene

Während des normalen Betriebs gibt Marketo zwei HTTP-Status-Code-Fehler zurück: `413 Request Entity Too Large` und `414 Request URI Too Long`. Um einen der Fehler zu beheben, ändern Sie die Anfrage und versuchen Sie es erneut. Sie können diese Fehler verhindern, indem Sie die Anfragengrößen vor der Übermittlung überprüfen.

Marketo gibt 413 zurück, wenn die Anfrage-Payload 1 MB oder 10 MB für den Import-Lead überschreitet. Überprüfen Sie die Anfragengröße vor der Übermittlung. Wenn die Anfrage aufgrund von Datensätzen das Limit überschreitet, verschieben Sie diese Datensätze in eine andere Anfrage.

Marketo gibt 414 zurück, wenn der URI einer GET-Anfrage 8 KB überschreitet. Überprüfen Sie die Länge der Abfragezeichenfolge vor der Übermittlung. Wenn es das Limit überschreitet, ändern Sie die Anfragemethode in POST, fügen Sie die Abfragezeichenfolge in den Anfragetext ein und fügen Sie den `_method=GET` Parameter hinzu. Lange URIs sind am häufigsten beim Abrufen großer Datensatz-Batches mit langen Filterwerten, wie z. B. einer GUID.

Der [Identity](https://developer.adobe.com/marketo-apis/api/identity/)-Endpunkt kann einen 401-Fehler „Nicht autorisiert“ zurückgeben, in der Regel weil die Client-ID oder das Client-Geheimnis ungültig ist. Die folgende Tabelle listet Fehler-Codes auf HTTP-Ebene auf.

<table>
  <thead>
    <tr>
      <th> Antwortcode </th>
      <th> Beschreibung </th>
      <th colspan="1"> Kommentar </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Anfrageentität zu groß</td>
      <td>Die Payload hat das Limit von 1 MB überschritten.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>Anfrage-URI zu lang</td>
      <td>Der URI der Anfrage überschritt 8k. Die Anfrage sollte als POST-Anfrage mit dem Parameter „_method=GET“ in der URL wiederholt werden und der Rest der Abfragezeichenfolge im Hauptteil der Anfrage.</td>
    </tr>
  </tbody>
</table>

#### Fehler auf Antwortebene

Fehler auf Antwortebene treten auf, wenn in der Antwort der `success` auf „false“ gesetzt wird. Sie verwenden die folgende Struktur:

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Jedes Objekt im Array „errors“ enthält zwei Elemente:

- `code`: Eine angegebene Ganzzahl von 601 bis 799.
- `message`: Der Klartext-Grund für den Fehler.

Ein 6xx-Code gibt an, dass die gesamte Anfrage fehlgeschlagen ist und nicht ausgeführt wurde. Stellen Sie beispielsweise einen 601-Fehler „Ungültiges Zugriffstoken“ wieder her, indem Sie sich erneut authentifizieren und das neue Zugriffstoken mit der Anfrage übergeben.

Ein 7xx-Code gibt an, dass die Anfrage fehlgeschlagen ist, da keine Daten zurückgegeben wurden oder die Anfrageparameter ungültig waren. Zu den Ursachen gehören ein ungültiges Datum oder ein fehlender erforderlicher Parameter.

#### Fehlercodes auf Antwortebene

>[!NOTE]
>
>Ein API-Aufruf, der diesen Antwort-Code zurückgibt, wird nicht auf Ihr tägliches Kontingent oder Ihr Ratenlimit angerechnet.

<table>
  <thead>
    <tr>
      <th> Antwortcode </th>
      <th> Beschreibung </th>
      <th> Kommentar </th>
    </tr>
  </thead>
  <tbody>
  <tr>
      <td><a name="500"></a>500</td>
      <td>Interner Server-Fehler</td>
      <td>Der Server ist auf eine unerwartete Bedingung gestoßen, die ihn daran hindert, die Anfrage zu erfüllen.  Innerhalb von Marketo kann dies falsch formatierte REST-API-Anfrage-URLs umfassen.</td>
    </tr>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Fehlerhaftes Gateway</td>
      <td>Der Remoteserver hat einen Fehler zurückgegeben. Wahrscheinlich eine Zeitüberschreitung. Die Anfrage sollte mit einem exponentiellen Backoff erneut versucht werden.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Zugriffstoken ungültig</td>
      <td>In der Anfrage war ein Zugriffs-Token-Parameter enthalten, der Wert war jedoch kein gültiges Zugriffs-Token.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Zugriffstoken abgelaufen</td>
      <td>Das im Aufruf enthaltene Zugriffs-Token ist aufgrund des Ablaufs nicht mehr gültig.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Zugriff verweigert</td>
      <td>Die Authentifizierung ist erfolgreich, aber der Benutzer verfügt nicht über ausreichende Berechtigungen, um diese API aufzurufen. [Zusätzliche Berechtigungen](custom-services.md) müssen der Benutzerrolle möglicherweise zugewiesen werden, oder <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">Zulassungsliste für IP-basierten API-Zugriff</a> muss aktiviert werden.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Zeitüberschreitung der Anfrage</td>
      <td>Die Anfrage wurde zu lange ausgeführt (z. B. aufgrund eines Datenbankkonflikts) oder überschritt den in der -Kopfzeile des Aufrufs angegebenen Timeout-Zeitraum.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>HTTP-Methode nicht unterstützt</td>
      <td>GET wird für den Endpunkt Leads synchronisieren nicht unterstützt. POST muss verwendet werden.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Maximales Ratenlimit "%s“; überschritten mit in "%s“ Sekunden</td>
      <td>Die Anzahl der Aufrufe in den letzten 20 Sekunden war größer als 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Tägliches Kontingent erreicht</td>
      <td>Die Anzahl der Anrufe heute hat das Kontingent des Abonnements überschritten (wird täglich um 0:00 Uhr CST zurückgesetzt).&gt;Ihr Kontingent finden Sie im Menü Admin-&gt;Web-Services . Sie können Ihr Kontingent über Ihren Account Manager erhöhen.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API vorübergehend nicht verfügbar</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>Ungültige JSON</td>
      <td>Der in der Anfrage enthaltene Text ist kein gültiges JSON.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Angefragte Ressource nicht gefunden</td>
      <td>Der URI im Aufruf stimmte nicht mit einem REST-API-Ressourcentyp überein. Dies ist häufig auf eine falsch geschriebene oder falsch formatierte Anfrage-URI zurückzuführen</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Systemfehler</td>
      <td>Alle nicht behandelten Ausnahmen</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Ungültiger Inhaltstyp</td>
      <td>Wenn dieser Fehler angezeigt wird, fügen Sie Ihrer Anfrage eine Kopfzeile für den Inhaltstyp hinzu, die das JSON-Format angibt. Versuchen Sie beispielsweise, „Content-Typ: application/json“ zu verwenden. <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Weitere Informationen finden Sie </a> dieser StackOverflow-Frage.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Ungültige mehrteilige Anfrage</td>
      <td>Der mehrteilige Inhalt des POST-Berichts war nicht korrekt formatiert.</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Ungültiges Abonnement</td>
      <td>Das Zielabonnement wurde nicht gefunden oder ist nicht erreichbar. Dies weist normalerweise auf eine vorübergehende Unzugänglichkeit hin.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Limit für gleichzeitigen Zugriff erreicht</td>
      <td>Anfragen werden von maximal einem Abonnement 10 gleichzeitig verarbeitet. Dies wird zurückgegeben, wenn bereits 10 Anforderungen laufen.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Ungültiger Abonnementtyp</td>
      <td>Für den Zugriff auf die Metadaten-API für benutzerdefinierte Objekte ist der entsprechende Marketo-Abonnementtyp erforderlich. Einzelheiten finden Sie in Ihrem CSM.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s darf nicht leer sein</td>
      <td>Das gemeldete Feld darf in der Anfrage nicht leer sein</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Keine Daten für ein bestimmtes Suchszenario gefunden</td>
      <td>Keine Datensätze für die angegebenen Suchparameter.
        Hinweis: Viele fehlgeschlagene Suchvorgänge geben „success = true“ und keine Fehler zurück und legen eine Informationszeichenfolge zu Warnungen fest.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>Die Funktion ist für das Abonnement nicht aktiviert</td>
      <td>Eine Beta-Funktion, die im Abonnement eines Benutzers nicht aktiviert wurde</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Ungültiges Datumsformat</td>
      <td><ul>
          <li>Es wurde ein Datum angegeben, das nicht im richtigen Format war</li>
          <li>Es wurde eine ungültige ID für dynamische Inhalte angegeben</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Verletzung von Geschäftsregeln</td>
      <td>Der Aufruf kann nicht ausgeführt werden, da er gegen eine Anforderung verstößt, ein Asset zu erstellen oder zu aktualisieren, z. B. wenn versucht wird, eine E-Mail ohne Vorlage zu erstellen. Dieser Fehler kann auch ausgegeben werden, wenn versucht wird:
        <ul>
          <li>Rufen Sie Inhalte für Landingpages ab, die Social-Media-Inhalte enthalten.</li>
          <li>Klonen Sie ein Programm, das bestimmte Asset-Typen enthält <a href="programs.md#clone">weitere Informationen finden Sie unter </a>Programm-Klon„).</li>
          <li>Genehmigen eines Assets, das keinen Entwurf hat (also bereits genehmigt wurde).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Übergeordneter Ordner nicht gefunden</td>
      <td>Der angegebene übergeordnete Ordner wurde nicht gefunden</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Inkompatibler Ordnertyp</td>
      <td>Der angegebene Ordner war nicht vom richtigen Typ, um die Anforderung zu erfüllen</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>Der Vorgang zum Zusammenführen mit dem Personenkonto ist ungültig</td>
      <td>Ein Aufruf zum Zusammenführen von Leads ist fehlgeschlagen, da versucht wurde, Leads zusammenzuführen, die Salesforce-Personenkonten sind.  Salesforce-Personenkonten müssen in Salesforce zusammengeführt werden.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>vorübergehender Fehler</td>
      <td>Eine Systemressource war zum Zeitpunkt des API-Aufrufs vorübergehend nicht verfügbar. Wenn dieser Fehler auftritt, wird empfohlen, auf Zeit zu warten und dann die Anfrage erneut zu stellen.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Der standardmäßige Datensatztyp wurde nicht gefunden</td>
      <td>Ein Leads-Zusammenführungsaufruf ist fehlgeschlagen, da kein standardmäßiger Datensatztyp gefunden wurde.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID nicht gefunden</td>
      <td>Ein Aufruf für Synchronisierungsmöglichkeiten wurde mit einem nicht vorhandenen Wert für „ExternalSalesPersonID“ durchgeführt.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Timeout-Ausnahme für Sperrwartezeit</td>
      <td>Ein Klon-Programmaufruf wurde ausgeführt und beim Warten auf eine Sperre ist eine Zeitüberschreitung aufgetreten.</td>
    </tr>
  </tbody>
</table>

### auf Rekordebene {#record_level_errors}

Fehler auf Datensatzebene zeigen an, dass die Anfrage gültig war, der Vorgang für einen einzelnen Datensatz jedoch nicht abgeschlossen werden konnte. Eine Antwort mit Fehlern auf Datensatzebene folgt diesem Muster:

#### Antwort

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "status":"skipped",
         "reasons":[
            {
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

Die Datensätze im Ergebnis-Array werden in der gleichen Reihenfolge wie die Datensätze im Anforderungseingabe-Array angezeigt. Jeder Datensatz kann unabhängig erfolgreich sein oder fehlschlagen, wie durch sein Statusfeld angegeben.

Bei einem fehlgeschlagenen Datensatz wird das Feld „Status“ „übersprungen“ und der Datensatz enthält ein Array „Gründe“. Jeder Grund enthält ein Element des Typs „Code“ und ein Element des Typs „Nachricht“. Der Code ist immer 1xxx. In der Meldung wird erläutert, warum der Datensatz übersprungen wurde.

Wenn beispielsweise eine Anfrage zur Lead-Synchronisierung „Aktion“ auf „createOnly“ setzt und bereits ein Lead für einen der gesendeten Schlüssel vorhanden ist, gibt die Antwort den Code 1005 und die Nachricht „Lead existiert bereits“ zurück, wie oben gezeigt.

#### Fehlercodes auf Datensatzebene

>[!NOTE]
>
><table>
><tbody>
>    <tr>
>      <td>Antwortcode</td>
>      <td>Beschreibung</td>
>      <td>Kommentar</td>
>    </tr>
>    <tr>
>      <td><a name="1001"></a>1001</td>
>      <td>Ungültiger Wert "%s“. Erforderlich vom Typ "%s“</td>
>      <td>Ein Fehler wird immer dann generiert, wenn ein Parameterwert nicht mit dem Typ übereinstimmt. Beispiel: der für einen ganzzahligen Parameter angegebene Zeichenfolgenwert.</td>
>    </tr>
>    <tr>
>      <td><a name="1002"></a>1002</td>
>      <td>Fehlender Wert für den erforderlichen Parameter '%s'</td>
>      <td>Fehler wird generiert, wenn in der Anfrage ein erforderlicher Parameter fehlt</td>
>    </tr>
>    <tr>
>      <td><a name="1003"></a>1003</td>
>      <td>Ungültige Daten</td>
>      <td>Wenn die übermittelten Daten für den angegebenen Endpunkt oder Modus keinen gültigen Typ haben; z. B. wenn die ID für einen Lead mit der Aktion „createOnly“ gesendet wird oder wenn die Anfragekampagne in einer Batch-Kampagne verwendet wird.</td>
>    </tr>
>    <tr>
>      <td><a name="1004"></a>1004</td>
>      <td>Lead nicht gefunden</td>
>      <td>Für syncLead, wenn die Aktion „updateOnly“ ist und wenn kein Lead gefunden wird</td>
>    </tr>
>    <tr>
>      <td><a name="1005"></a>1005</td>
>      <td>Lead existiert bereits</td>
>      <td>Für syncLead, wenn die Aktion „createOnly“ lautet und bereits ein Lead vorhanden ist</td>
>    </tr>
>    <tr>
>      <td><a name="1006"></a>1006</td>
>      <td>Feld "%s“ nicht gefunden</td>
>      <td>Ein im Aufruf eingeschlossenes Feld ist kein gültiges Feld.</td>
>    </tr>
>    <tr>
>      <td><a name="1007"></a>1007</td>
>      <td>Mehrere Leads entsprechen den Suchkriterien</td>
>      <td>Mehrere Leads entsprechen den Suchkriterien. Aktualisierungen können nur durchgeführt werden, wenn der Schlüssel mit einem einzelnen Datensatz übereinstimmt.</td>
>    </tr>
>    <tr>
>      <td><a name="1008"></a>1008</td>
>      <td>Zugriff auf Partition '%s' verweigert</td>
>      <td>Der Benutzer für den benutzerdefinierten Dienst hat keinen Zugriff auf einen Arbeitsbereich mit der Partition, auf der der Datensatz vorhanden ist.</td>
>    </tr>
>    <tr>
>      <td><a name="1009"></a>1009</td>
>      <td>Partitionsname muss angegeben werden</td>
>      <td></td>
>    </tr>
>    <tr>
>      <td><a name="1010"></a>1010</td>
>      <td>Partitionsaktualisierung nicht zulässig</td>
>      <td>Der angegebene Datensatz existiert bereits in einer separaten Lead-Partition.</td>
>    </tr>
>    <tr>
>      <td><a name="1011"></a>1011</td>
>      <td>Feld "%s“ wird nicht unterstützt</td>
>      <td>Wenn das Suchfeld oder „filterType“ mit nicht unterstützten Standardfeldern angegeben wird (z. B.: firstName, lastName)</td>
>    </tr>
>    <tr>
>      <td><a name="1012"></a>1012</td>
>      <td>Ungültiger Cookie-Wert "%s“</td>
>      <td>Kann auftreten, wenn der Aufruf <a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/associateLeadUsingPOST">Lead verknüpfen</a> mit einem ungültigen Wert für den Parameter „cookie“ erfolgt.
>        Dies tritt auch auf, wenn <a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET">Leads nach Filtertyp abrufen</a> mit „filterType=cookies“ und einem ungültigen Wert für den Parameter „filterValues“ aufgerufen wird.</td>
>    </tr>
>    <tr>
>      <td><a name="1013"></a>1013</td>
>      <td>Objekt nicht gefunden</td>
>      <td>Objekt abrufen (Liste, Kampagne) nach ID gibt diesen Fehlercode zurück</td>
>    </tr>
>    <tr>
>      <td><a name="1014"></a>1014</td>
>      <td>Objekt konnte nicht erstellt werden</td>
>      <td>Objekt (Liste) konnte nicht erstellt werden</td>
>    </tr>
>    <tr>
>      <td><a name="1015"></a>1015</td>
>      <td>Lead nicht in Liste</td>
>      <td>Der angegebene Lead ist kein Mitglied der Zielliste</td>
>    </tr>
>    <tr>
>      <td><a name="1016"></a>1016</td>
>      <td>Zu viele Importe</td>
>      <td>Es gibt zu viele Importe in der Warteschlange. Es sind maximal 10 zulässig</td>
>    </tr>
>    <tr>
>      <td><a name="1017"></a>1017</td>
>      <td>Objekt bereits vorhanden</td>
>      <td>Erstellung fehlgeschlagen, da der Datensatz bereits existiert</td>
>    </tr>
>    <tr>
>      <td><a name="1018"></a>1018</td>
>      <td>CRM aktiviert</td>
>      <td>Die Aktion konnte nicht ausgeführt werden, da für die Instanz eine native CRM-Integration aktiviert ist.</td>
>    </tr>
>    <tr>
>      <td><a name="1019"></a>1019</td>
>      <td>Importvorgang wird durchgeführt</td>
>      <td>Die Zielliste wird bereits in importiert.</td>
>    </tr>
>    <tr>
>      <td><a name="1020"></a>1020</td>
>      <td>Zu viele Klone zum Programmieren</td>
>      <td>Das Abonnement hat die zugewiesene Verwendung von „cloneToProgramName“ im Zeitplan-Programm für den Tag erreicht</td>
>    </tr>
>    <tr>
>      <td><a name="1021"></a>1021</td>
>      <td>Update der Firma nicht zulässig</td>
>      <td>Update des Unternehmens während der Synchronisierung nicht zulässig</td>
>    </tr>
>    <tr>
>      <td><a name="1022"></a>1022</td>
>      <td>Verwendetes Objekt</td>
>      <td>Löschen ist nicht zulässig, wenn ein Objekt von einem anderen Objekt verwendet wird</td>
>    </tr>
>    <tr>
>      <td><a name="1025"></a>1025</td>
>      <td>Programmstatus nicht gefunden</td>
>      <td>Zum Ändern des Status des Lead-Programms wurde ein Status angegeben, der nicht mit einem Status übereinstimmt, der für den Kanal des Programms verfügbar ist.</td>
>    </tr>
>    <tr>
>      <td><a name="1026"></a>1026</td>
>      <td>Benutzerdefiniertes Objekt nicht aktiviert</td>
>      <td>Die Aktion konnte nicht ausgeführt werden, da für die Instanz die Integration benutzerdefinierter Objekte nicht aktiviert ist.</td>
>    </tr>
>    <tr>
>      <td><a name="1027"></a>1027</td>
>      <td>Maximales Limit für Aktivitätstyp erreicht</td>
>      <td>Das Abonnement hat die maximale Anzahl der verfügbaren benutzerdefinierten Aktivitätstypen erreicht.</td>
>    </tr>
>    <tr>
>      <td><a name="1028"></a>1028</td>
>      <td>Maximale Feldgrenze erreicht</td>
>      <td>Benutzerdefinierte Aktivitäten haben maximal 20 sekundäre Attribute.</td>
>    </tr>
>    <tr>
>      <td><a name="1029"></a>1029</td>
>      <td><ul>
>          <li>Zu viele Aufträge in der Warteschlange</li>
>          <li>Tägliches Exportkontingent überschritten</li>
>          <li>Auftrag bereits in der Warteschlange</li>
>        </ul></td>
>      <td><ul>
>          <li>Abonnements dürfen zu einem bestimmten Zeitpunkt maximal 10 Massenextraktionsaufträge in der Warteschlange enthalten.</li>
>          <li>Standardmäßig sind Extraktionsaufträge auf 500 MB pro Tag beschränkt (wird täglich um 0:00 Uhr CST zurückgesetzt).</li>
>          <li>Die Export-ID wurde bereits in die Warteschlange gestellt.</li>
>        </ul></td>
>    </tr>
>    <tr>
>      <td><a name="1035"></a>1035</td>
>      <td>Nicht unterstützter Filtertyp</td>
>      <td>In einigen Abonnements werden die folgenden Filtertypen für die Lead-Massenextraktion nicht unterstützt: updatedAt, smartListId, smartListName.</td>
>    </tr>
>    <tr>
>      <td><a name="1036"></a>1036</td>
>      <td>Doppeltes Objekt in Eingabe gefunden</td>
>      <td>Es wurde ein Aufruf ausgeführt, zwei oder mehr Datensätze mit demselben Fremdschlüssel zu aktualisieren. Beispiel: Ein Aufruf vom Typ „Unternehmen synchronisieren“, der dieselbe externalCompanyId für mehr als ein Unternehmen verwendet.</td>
>    </tr>
>    <tr>
>      <td><a name="1037"></a>1037</td>
>      <td>Lead wurde übersprungen</td>
>      <td>Der Lead wurde übersprungen, da er sich bereits in oder hinter diesem Status befindet.</td>
>    </tr>
>    <tr>
>      <td><a name="1042"></a>1042</td>
>      <td>Ungültiges Ausführungsdatum</td>
>      <td>Das für „Kampagne planen“ angegebene „runAt“-Datum lag zu weit in der Zukunft (maximal 2 Jahre).</td>
>    </tr>
>    <tr>
>      <td><a name="1048"></a>1048</td>
>      <td>Entwurf zum Verwerfen benutzerdefinierter Objekte fehlgeschlagen</td>
>      <td>Es wurde ein Aufruf ausgeführt, um die Entwurfsversion eines benutzerdefinierten -Objekts zu verwerfen.</td>
>    </tr>
>    <tr>
>      <td><a name="1049"></a>1049</td>
>      <td>Aktivität konnte nicht erstellt werden</td>
>      <td>Attributarray zu lang.
>        Das Array von Attributen, die an den Datensatz übergeben werden, hat die maximale Länge von 65536 Byte überschritten</td>
>    </tr>
>    <tr>
>      <td><a name="1076"></a>1076</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a> Aufruf mit mergeInCRM-Flag ist 4.</td>
>      <td>Es wird ein doppelter Eintrag erstellt. Es wird empfohlen, stattdessen einen vorhandenen Datensatz zu verwenden.
>        Dies ist die Fehlermeldung, die Marketo beim Zusammenführen in Salesforce erhält.</td>
>    </tr>
>    <tr>
>      <td><a name="1077"></a>1077</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a>-Aufruf aufgrund der Länge des "SFDC-Felds“ fehlgeschlagen</td>
>      <td>Ein Aufruf zum Zusammenführen von Leads, bei dem mergeInCRM auf „true“ festgelegt ist, ist fehlgeschlagen, da das "SFDC-Feld“ die zulässige Zeichenbeschränkung überschreitet. Um dies zu korrigieren, reduzieren Sie die Länge von "SFDC Field“ oder setzen Sie mergeInCRM auf „false“.</td>
>    </tr>
>    <tr>
>      <td><a name="1078"></a>1078</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a>-Aufruf fehlgeschlagen, da die gelöschte Entität, kein Lead/Kontakt oder die Feldfilterkriterien nicht übereinstimmen.</td>
>      <td>Zusammenführungsfehler, Zusammenführungsvorgang kann im nativ synchronisierten CRM nicht durchgeführt werden
>        Dies ist die Fehlermeldung, die Marketo beim Zusammenführen in Salesforce erhält.</td>
>    </tr>
>    <tr>
>      <td><a name="1079"></a>1079</td>
>      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/mergeLeadsUsingPOST">Lead zusammenführen</a>-Aufruf aufgrund eines personalisierten URL-Konflikts in doppelten Einträgen fehlgeschlagen</td>
>      <td>Bei einem Aufruf zum Zusammenführen von Leads wurden viele Leads mit derselben personalisierten URL angegeben. Verwenden Sie zum Auflösen die Marketo Engage-Benutzeroberfläche, um diese Datensätze zusammenzuführen.</td>
>    </tr>
>  </tbody>
></table>
