---
title: "Fehlercodes"
feature: REST API
description: "Beschreibungen des Marketo-Fehlercodes."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '2272'
ht-degree: 3%

---


# Fehlercodes

Im Folgenden finden Sie eine Liste der REST-API-Fehlercodes und eine Erläuterung, wie Fehler an Anwendungen zurückgegeben werden.

## Handhabung und Protokollierung von Ausnahmen

Bei der Entwicklung für Marketo ist es wichtig, dass Anforderungen und Antworten protokolliert werden, wenn eine unerwartete Ausnahme auftritt. Während bestimmte Arten von Ausnahmen, wie z. B. die abgelaufene Authentifizierung, sicher durch eine erneute Authentifizierung verarbeitet werden können, erfordern andere möglicherweise Supportinteraktionen, und Anfragen und Antworten werden in diesem Szenario immer angefordert.

## Fehlertypen

Die Marketo REST-API kann im Normalbetrieb drei verschiedene Fehlertypen zurückgeben:

* HTTP-Ebene: Diese Fehler werden durch eine `4xx` Code.
* Antwortebene: Diese Fehler sind im Array &quot;errors&quot;der JSON-Antwort enthalten.
* Record-Level: Diese Fehler sind im &quot;result&quot;-Array der JSON-Antwort enthalten und werden auf einer einzelnen Datensatzbasis mit dem &quot;status&quot;-Feld und dem &quot;reasons&quot;-Array angezeigt.

Für die Fehlertypen &quot;Response-Level&quot;und &quot;Record-Level&quot;wird ein HTTP-Statuscode von 200 zurückgegeben. Bei allen Fehlertypen sollte der HTTP-Vernunftausdruck nicht ausgewertet werden, da er optional ist und geändert werden kann.

### Fehler auf HTTP-Ebene

Unter normalen Betriebsbedingungen sollte Marketo nur zwei HTTP-Status-Code-Fehler zurückgeben. `413 Request Entity Too Large`, und `414 Request URI Too Long`. Diese sind beide durch Auffangen des Fehlers, Ändern der Anforderung und Wiederholen wiederherstellbar, aber mit intelligenten Kodierungspraktiken sollten Sie diese niemals in der Wildnis finden.

Marketo gibt 413 zurück, wenn die Anfrage-Payload 1 MB oder im Fall von Import-Lead 10 MB überschreitet. In den meisten Szenarien ist es unwahrscheinlich, dass diese Beschränkungen erreicht werden. Wenn Sie jedoch eine Überprüfung der Anforderungsgröße vornehmen und Datensätze verschieben, wodurch die Beschränkung auf eine neue Anforderung überschritten wird, sollten keine Umstände mehr möglich sein, die dazu führen, dass dieser Fehler von allen Endpunkten zurückgegeben wird.

414 wird zurückgegeben, wenn der URI einer GET-Anfrage 8 KB überschreitet. Um dies zu vermeiden, prüfen Sie anhand der Länge Ihrer Abfragezeichenfolge, ob sie diesen Grenzwert überschreitet. Wenn Ihre Anforderung in eine POST-Methode geändert wird, geben Sie Ihre Abfragezeichenfolge als Anfrageinhalt mit dem zusätzlichen Parameter ein. `_method=GET`. Dadurch wird die Beschränkung für URIs vergessen. In den meisten Fällen ist es selten, diese Grenze zu erreichen, aber sie kommt eher häufig vor, wenn große Mengen von Datensätzen mit langen individuellen Filterwerten wie einer GUID abgerufen werden.
Die [Identität](https://developer.adobe.com/marketo-apis/api/identity/) -Endpunkt kann einen Fehler vom Typ 401 Nicht autorisiert zurückgeben. Dies liegt normalerweise an einer ungültigen Client-ID oder einem ungültigen Client-Geheimnis. Fehlercodes auf HTTP-Ebene

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
      <td>Die Payload überschreitet die Grenze von 1 MB.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>Anfrage-URI zu lang</td>
      <td>Der URI der Anfrage überschreitet 8.000. Die Anfrage sollte als POST mit param `_method=GET` in der URL und dem Rest der Abfragezeichenfolge im Text der Anfrage wiederholt werden.</td>
    </tr>
  </tbody>
</table>


#### Fehler auf Antwortebene

Fehler der Antwortebene sind vorhanden, wenn die Variable `success` -Parameter der Antwort auf &quot;false&quot;festgelegt ist und wie folgt strukturiert sind:

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

Jedes Objekt im Array &quot;errors&quot;hat zwei Mitglieder. `code`, was eine zitierte Ganzzahl von 601 bis 799 und eine `message` Geben Sie den Klartext-Grund für den Fehler an. 6xx-Codes weisen immer darauf hin, dass eine Anfrage vollständig fehlgeschlagen ist und nicht ausgeführt wurde. Ein Beispiel ist ein 601, &quot;Zugriffstoken ungültig&quot;, das durch erneutes Authentifizieren und Übergeben des neuen Zugriffstokens mit der Anfrage abgerufen werden kann. 7xx-Fehler weisen darauf hin, dass die Anfrage fehlgeschlagen ist, entweder weil keine Daten zurückgegeben wurden oder die Anfrage falsch parametrisiert wurde, z. B. weil ein ungültiges Datum angegeben wurde oder ein erforderlicher Parameter fehlt.

#### Fehlercodes auf Antwortebene

Ein API-Aufruf, der diesen Antwort-Code zurückgibt, wird nicht mit Ihrem täglichen Kontingent oder Ihrem Ratenlimit gezählt.

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
      <td><a name="502"></a>502</td>
      <td>Fehlerhaftes Gateway</td>
      <td>Der Remote-Server hat einen Fehler zurückgegeben. Wahrscheinlich eine Zeitüberschreitung. Die Anfrage sollte mit exponentiellem Backoff wiederholt werden.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Zugriffstoken ungültig</td>
      <td>Ein Zugriffstoken -Parameter war in der Anfrage enthalten, aber der Wert war kein gültiges Zugriffstoken.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Zugriffstoken abgelaufen</td>
      <td>Das im Aufruf enthaltene Zugriffstoken ist aufgrund des Ablaufs nicht mehr gültig.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Zugriff verweigert </td>
      <td>Die Authentifizierung ist erfolgreich, der Benutzer verfügt jedoch nicht über die nötigen Berechtigungen, um diese API aufzurufen. [Zusätzliche Berechtigungen](custom-services.md) müssen möglicherweise der Benutzerrolle zugewiesen werden, oder <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">Zulassungsliste für IP-basierten API-Zugriff</a> aktiviert sein.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Zeitlimit für Anfragen</td>
      <td>Die Anfrage wurde zu lange ausgeführt (z. B. weil ein Datenbankkonflikt auftrat) oder die in der Kopfzeile des Aufrufs angegebene Zeitüberschreitungsdauer überschritten.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>HTTP-Methode wird nicht unterstützt</td>
      <td>GET wird für den Endpunkt "Leads synchronisieren"nicht unterstützt. POST muss verwendet werden.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Maximale Ratenbegrenzung "%s"; überschritten mit in "%s"-Sekunden</td>
      <td>Die Anzahl der Aufrufe in den letzten 20 Sekunden war größer als 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Tägliche Quote erreicht</td>
      <td>Die Anzahl der Anrufe hat heute das Abonnementkontingent überschritten (wird täglich um 12:00 Uhr CST zurückgesetzt).&gt;Ihr Kontingent finden Sie im Menü Admin-&gt;Web-Services . Sie können Ihr Kontingent über Ihren Kundenbetreuer erhöhen.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API vorübergehend nicht verfügbar</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>Ungültige JSON</td>
      <td>Der in der Anfrage enthaltene Text ist keine gültige JSON.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Angeforderte Ressource nicht gefunden</td>
      <td>Der URI im Aufruf entsprach nicht dem REST-API-Ressourcentyp. Dies liegt oft an einem falsch geschriebenen oder falsch formatierten Anforderungs-URI</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Systemfehler</td>
      <td>Alle nicht verarbeiteten Ausnahmen</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Ungültiger Content-Typ</td>
      <td>Wenn dieser Fehler auftritt, fügen Sie Ihrer Anfrage eine Kopfzeile vom Inhaltstyp hinzu, in der das JSON-Format angegeben wird. Versuchen Sie beispielsweise, den Inhaltstyp "application/json"zu verwenden. <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Siehe diese StackOverflow-Frage</a> für weitere Details.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Ungültige Multipart-Anfrage</td>
      <td>Der mehrteilige Inhalt der POST wurde nicht korrekt formatiert</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Ungültiges Abonnement</td>
      <td>Das Ziel-Abonnement kann nicht gefunden werden oder ist nicht erreichbar. Dies weist in der Regel auf eine vorübergehende Unzugänglichkeit hin.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Gleichzeitige Zugriffsbeschränkung erreicht</td>
      <td>Anfragen werden höchstens von jedem Abonnement 10 gleichzeitig verarbeitet. Dies wird zurückgegeben, wenn bereits 10 aktuelle Anfragen vorliegen.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Ungültiger Abonnementtyp</td>
      <td>Der entsprechende Marketo-Abonnementtyp ist für den Zugriff auf die API für benutzerdefinierte Objektmetadaten erforderlich. Weitere Informationen finden Sie in Ihrem CSM.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s kann nicht leer sein</td>
      <td>Das gemeldete Feld darf in der Anfrage nicht leer sein.</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Keine Daten für ein bestimmtes Suchszenario gefunden</td>
      <td>Es wurden keine Datensätze mit den angegebenen Suchparametern abgeglichen.
        Hinweis: Viele fehlgeschlagene Suchvorgänge geben "success = true"zurück und keine Fehler und legen eine Warnungsinformations-Zeichenfolge fest.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>Die Funktion ist für das Abonnement nicht aktiviert.</td>
      <td>Eine Beta-Funktion, die in einem Abonnement eines Benutzers nicht aktiviert wurde</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Ungültiges Datumsformat</td>
      <td><ul>
          <li>Es wurde ein Datum angegeben, das nicht im richtigen Format war</li>
          <li>Es wurde eine ungültige ID für den dynamischen Inhalt angegeben.</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Verletzung von Geschäftsregeln</td>
      <td>Der Aufruf kann nicht ausgeführt werden, da er gegen eine Anforderung verstößt, ein Asset zu erstellen oder zu aktualisieren, z. B. beim Versuch, eine E-Mail ohne Vorlage zu erstellen. Es ist auch möglich, diesen Fehler zu erhalten, wenn Sie versuchen:
        <ul>
          <li>Rufen Sie Inhalte für Landingpages ab, die soziale Inhalte enthalten.</li>
          <li>Klonen Sie ein Programm, das bestimmte Asset-Typen enthält (siehe <a href="programs.md#clone">Programmklon</a> für weitere Informationen).</li>
          <li>Genehmigen Sie ein Asset ohne Entwurf (d. h. wurde bereits genehmigt).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Übergeordneter Ordner nicht gefunden</td>
      <td>Der angegebene übergeordnete Ordner konnte nicht gefunden werden</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Inkompatibler Ordnertyp</td>
      <td>Der angegebene Ordner hatte nicht den richtigen Typ, um die Anfrage zu erfüllen</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>Vorgang "Merge to person Account"ist ungültig</td>
      <td>Ein ZusammenführungsLeads -Aufruf ist aufgrund eines Versuchs zum Zusammenführen von Leads fehlgeschlagen, bei denen es sich um Salesforce-Personenkonten handelt.  Salesforce-Personenkonten müssen in Salesforce zusammengeführt werden.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Übergangsfehler</td>
      <td>Zum Zeitpunkt des API-Aufrufs war eine Systemressource vorübergehend nicht verfügbar. Wenn dieser Fehler auftritt, wird empfohlen, auf einen bestimmten Zeitraum zu warten und die Anfrage dann erneut auszuführen.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Standard-Datensatztyp kann nicht gefunden werden</td>
      <td>Ein Lead-Aufruf zum Zusammenführen schlug fehl, da kein standardmäßiger Datensatztyp gefunden werden konnte.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID nicht gefunden</td>
      <td>Es wurde ein Synchronisierungsangebotsaufruf mit einem nicht vorhandenen "ExternalSalesPersonID"-Wert durchgeführt.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Wartezeitüberschreitungsausnahme sperren</td>
      <td>Ein Clone Program -Aufruf wurde durchgeführt und die Zeit abgelaufen, bis auf eine Sperre gewartet wurde.</td>
    </tr>
  </tbody>
</table>

### Record Level {#record_level_errors}

Fehler auf Datensatzebene zeigen an, dass ein Vorgang für einen einzelnen Datensatz nicht abgeschlossen werden konnte, die Anfrage selbst jedoch gültig war. Eine Antwort mit Fehlern auf Datensatzebene folgt diesem Muster:


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

Datensätze, die im Ergebnis-Array von Aufrufen enthalten sind, werden auf die gleiche Weise wie das Eingabe-Array einer Anforderung sortiert.
Jeder Datensatz in einer erfolgreichen Anfrage kann einzeln erfolgreich sein oder fehlschlagen, was durch das Statusfeld jedes Datensatzes im Ergebnisarray einer Antwort angegeben wird. Das &quot;status&quot;-Feld dieser Datensätze wird &quot;übersprungen&quot; und ein &quot;reasons&quot;-Array ist vorhanden. Jeder Grund enthält ein &quot;code&quot;-Mitglied und ein &quot;message&quot;-Mitglied. Der Code ist immer 1xxx und die Meldung zeigt an, warum der Datensatz übersprungen wurde. Ein Beispiel wäre, wenn für eine SynchronisierungsLeads-Anfrage &quot;action&quot;auf &quot;createOnly&quot;gesetzt ist, aber für einen der Schlüssel in den gesendeten Datensätzen bereits ein Lead vorhanden ist. In diesem Fall wird der Code 1005 zurückgegeben und die Meldung &quot;Lead ist bereits vorhanden&quot;wie oben dargestellt angezeigt.

#### Fehlercodes auf Datensatzebene

<table>
  <tbody>
    <tr>
      <td>Antwortcode</td>
      <td>Beschreibung</td>
      <td>Kommentar</td>
    </tr>
    <tr>
      <td><a name="1001"></a>1001</td>
      <td>Ungültiger Wert "%s". Erforderlich für Typ '%s'</td>
      <td>Fehler wird immer dann generiert, wenn ein Parameterwert eine Typabweichung aufweist. Beispielsweise der für einen Ganzzahlparameter angegebene Zeichenfolgenwert.</td>
    </tr>
    <tr>
      <td><a name="1002"></a>1002</td>
      <td>Fehlender Wert für den erforderlichen Parameter "%s"</td>
      <td>Fehler wird generiert, wenn ein erforderlicher Parameter in der Anforderung fehlt</td>
    </tr>
    <tr>
      <td><a name="1003"></a>1003</td>
      <td>Ungültige Daten</td>
      <td>Wenn die gesendeten Daten kein gültiger Typ für den angegebenen Endpunkt oder Modus sind, z. B. wenn die ID für einen Lead mit der Aktion createOnly gesendet wird oder wenn die Anforderungskampagne für eine Batch-Kampagne verwendet wird.</td>
    </tr>
    <tr>
      <td><a name="1004"></a>1004</td>
      <td>Lead nicht gefunden</td>
      <td>Wenn für syncLead die Aktion "updateOnly"lautet und kein Lead gefunden wird</td>
    </tr>
    <tr>
      <td><a name="1005"></a>1005</td>
      <td>Lead ist bereits vorhanden</td>
      <td>Wenn für syncLead die Aktion "createOnly"lautet und wenn bereits ein Lead vorhanden ist</td>
    </tr>
    <tr>
      <td><a name="1006"></a>1006</td>
      <td>Feld '%s' nicht gefunden</td>
      <td>Ein im Aufruf enthaltenes Feld ist kein gültiges Feld.</td>
    </tr>
    <tr>
      <td><a name="1007"></a>1007</td>
      <td>Mehrere Leads entsprechen den Suchkriterien</td>
      <td>Mehrere Leads entsprechen den Suchkriterien. Aktualisierungen können nur durchgeführt werden, wenn der Schlüssel mit einem einzelnen Datensatz übereinstimmt</td>
    </tr>
    <tr>
      <td><a name="1008"></a>1008</td>
      <td>Zugriff verweigert, um '%s' zu partitionieren</td>
      <td>Der Benutzer für den benutzerdefinierten Dienst hat keinen Zugriff auf einen Arbeitsbereich mit der Partition, in der der Datensatz vorhanden ist.</td>
    </tr>
    <tr>
      <td><a name="1009"></a>1009</td>
      <td>Der Partitionsname muss angegeben werden</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="1010"></a>1010</td>
      <td>Partition Update ist nicht erlaubt</td>
      <td>Der angegebene Datensatz ist bereits in einer separaten Lead-Partition vorhanden.</td>
    </tr>
    <tr>
      <td><a name="1011"></a>1011</td>
      <td>Feld '%s' nicht unterstützt</td>
      <td>Beim Nachschlagen von Feld oder `filterType` mit nicht unterstützten Standardfeldern (z. B. firstName, lastName)</td>
    </tr>
    <tr>
      <td><a name="1012"></a>1012</td>
      <td>Ungültiger Cookie-Wert "%s"</td>
      <td>Kann auftreten, wenn die <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">Lead verknüpfen</a> mit einem ungültigen Wert für den Cookie-Parameter.
        Dies geschieht auch beim Aufruf von <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Abrufen von Leads nach Filtertyp</a> mit filterType=cookies und ungültigem gültigen Wert für den Parameter filterValues .</td>
    </tr>
    <tr>
      <td><a name="1013"></a>1013</td>
      <td>Objekt nicht gefunden</td>
      <td>Objekt (Liste, Kampagne) nach ID abrufen gibt diesen Fehlercode zurück</td>
    </tr>
    <tr>
      <td><a name="1014"></a>1014</td>
      <td>Objekt konnte nicht erstellt werden</td>
      <td>Objekt konnte nicht erstellt werden (Liste)</td>
    </tr>
    <tr>
      <td><a name="1015"></a>1015</td>
      <td>Lead nicht in Liste</td>
      <td>Der angegebene Lead ist kein Mitglied der Zielliste</td>
    </tr>
    <tr>
      <td><a name="1016"></a>1016</td>
      <td>Zu viele Einfuhren</td>
      <td>Zu viele Importe stehen in der Warteschlange. Maximal 10 ist zulässig</td>
    </tr>
    <tr>
      <td><a name="1017"></a>1017</td>
      <td>Objekt ist bereits vorhanden</td>
      <td>Erstellung fehlgeschlagen, da der Datensatz bereits vorhanden ist</td>
    </tr>
    <tr>
      <td><a name="1018"></a>1018</td>
      <td>CRM aktiviert</td>
      <td>Die Aktion konnte nicht ausgeführt werden, da für die Instanz eine native CRM-Integration aktiviert ist.</td>
    </tr>
    <tr>
      <td><a name="1019"></a>1019</td>
      <td>Importvorgang wird durchgeführt</td>
      <td>Die Zielliste ist bereits in</td>
    </tr>
    <tr>
      <td><a name="1020"></a>1020</td>
      <td>Zu viele Klone zu programmieren</td>
      <td>Das Abonnement hat die zulässige Verwendung von "cloneToProgramName"im Zeitplan-Programm für den Tag erreicht</td>
    </tr>
    <tr>
      <td><a name="1021"></a>1021</td>
      <td>Firmenaktualisierung nicht zulässig</td>
      <td>Firmenaktualisierung während der SynchronisierungLead nicht zulässig</td>
    </tr>
    <tr>
      <td><a name="1022"></a>1022</td>
      <td>Verwendetes Objekt</td>
      <td>Löschen ist nicht zulässig, wenn ein Objekt von einem anderen Objekt verwendet wird</td>
    </tr>
    <tr>
      <td><a name="1025"></a>1025</td>
      <td>Programmstatus nicht gefunden</td>
      <td>Der Status Lead-Programm-Status wurde unter Ändern des Status festgelegt, der nicht mit dem für den Kanal des Programms verfügbaren Status übereinstimmt.</td>
    </tr>
    <tr>
      <td><a name="1026"></a>1026</td>
      <td>Benutzerdefiniertes Objekt nicht aktiviert</td>
      <td>Die Aktion konnte nicht ausgeführt werden, da die Integration benutzerdefinierter Objekte in der Instanz nicht aktiviert ist.</td>
    </tr>
    <tr>
      <td><a name="1027"></a>1027</td>
      <td>Maximale Aktivitätstypbegrenzung erreicht</td>
      <td>Das Abonnement hat die maximale Anzahl der verfügbaren benutzerdefinierten Aktivitätstypen erreicht.</td>
    </tr>
    <tr>
      <td><a name="1028"></a>1028</td>
      <td>Maximale Feldbegrenzung erreicht</td>
      <td>Benutzerdefinierte Aktivitäten haben maximal 20 sekundäre Attribute.</td>
    </tr>
    <tr>
      <td><a name="1029"></a>1029</td>
      <td><ul>
          <li>Zu viele Aufträge in der Warteschlange</li>
          <li>Überschreitung des Tageskontingents</li>
        </ul></td>
      <td><ul>
          <li>Abonnements sind jeweils auf maximal 10 Massenextraktionsaufträge in der Warteschlange beschränkt.</li>
          <li>Standardmäßig sind die Extraktionsaufträge auf 500 MB pro Tag beschränkt (wird täglich um 12:00 Uhr CST zurückgesetzt).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="1035"></a>1035</td>
      <td>Nicht unterstützter Filtertyp</td>
      <td>In einigen Abonnements werden die folgenden Filtertypen für die Massenextraktion nicht unterstützt: updatedAt, smartListId, smartListName.</td>
    </tr>
    <tr>
      <td><a name="1036"></a>1036</td>
      <td>Dupliziertes Objekt in Eingabe gefunden</td>
      <td>Es wurde ein Aufruf gesendet, zwei oder mehr Datensätze mit demselben Fremdschlüssel zu aktualisieren. Beispiel: Ein Synchronisierungsunternehmen-Aufruf mit derselben externalCompanyId für mehrere Unternehmen.</td>
    </tr>
    <tr>
      <td><a name="1037"></a>1037</td>
      <td>Lead wurde übersprungen</td>
      <td>Das Lead wurde übersprungen, da es sich bereits in diesem Status befindet oder diesen Status überschritten hat.</td>
    </tr>
    <tr>
      <td><a name="1042"></a>1042</td>
      <td>Ungültiges runAt-Datum</td>
      <td>Das für Zeitplan-Kampagnen angegebene runAt-Datum war zu weit in die Zukunft (maximal 2 Jahre).</td>
    </tr>
    <tr>
      <td><a name="1048"></a>1048</td>
      <td>Benutzerdefiniertes Objekt - Entwurf fehlgeschlagen</td>
      <td>Es wurde ein Aufruf zum Verwerfen der Entwurfsversion eines benutzerdefinierten Objekts durchgeführt.</td>
    </tr>
    <tr>
      <td><a name="1049"></a>1049</td>
      <td>Aktivität konnte nicht erstellt werden</td>
      <td>Attribute-Array zu lang.
        Das Array von Attributen, die an den Datensatz übergeben werden, überschreitet die maximale Länge von 65536 Byte</td>
    </tr>
    <tr>
      <td><a name="1076"></a>1076</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a> Aufruf mit der Markierung mergeInCRM ist 4.</td>
      <td>Sie erstellen einen doppelten Datensatz. Es wird empfohlen, stattdessen einen vorhandenen Datensatz zu verwenden.
        Dies ist die Fehlermeldung, die Marketo beim Zusammenführen in Salesforce erhält.</td>
    </tr>
    <tr>
      <td><a name="1077"></a>1077</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a> Aufruf fehlgeschlagen aufgrund der "SFDC-Feldlänge"</td>
      <td>Ein Leads-Zusammenführungsaufruf mit mergeInCRM, der auf "true"gesetzt ist, ist fehlgeschlagen, da "SFDC Field"die zulässige Zeichenbeschränkung überschreitet. Um dies zu korrigieren, reduzieren Sie die Länge von "SFDC Field"oder setzen Sie mergeInCRM auf "false".</td>
    </tr>
    <tr>
      <td><a name="1078"></a>1078</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Leads zusammenführen</a> -Aufruf aufgrund der gelöschten Entität fehlgeschlagen, die Kriterien für Lead-/Kontakt- oder Feldfilter stimmen nicht überein.</td>
      <td>Zusammenführungsfehler, keine Zusammenführungsoperation im nativ synchronisierten CRM durchführen Dies ist die Fehlermeldung, die Marketo beim Zusammenführen in Salesforce erhält.</td>
    </tr>
  </tbody>
</table>
