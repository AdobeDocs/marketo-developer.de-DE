---
title: Best Practices für die Marketo-Integration
feature: REST API
description: Best Practices für die Verwendung von Marketo-APIs.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# Best Practices für die Marketo-Integration

## API-Beschränkungen

- **Tägliches Kontingent:** Die meisten Abonnements erhalten 50.000 API-Aufrufe pro Tag (die täglich um 0:00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Account Manager erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz beschränkt auf 100 Aufrufe pro 20 Sekunden.
- **Parallelitätslimit:**  Maximal zehn gleichzeitige API-Aufrufe.
- **Batch-Größe:** Lead-DB - 300 Datensätze; Asset-Abfrage - 200 Datensätze
- **REST-API-Payload-Größe:** 1 MB
- **Größe der Massenimportdatei:** 10 MB
- Max. Batch-Größe für **SOAP:** 300 Datensätze
- **Massenextraktionsaufträge:** 2 werden ausgeführt; 10 in die Warteschlange gestellt (einschließlich)

## Schnelltipps

- Nehmen wir an, dass Ihre Anwendung um Kontingent-, Rate- und Parallelitätsressourcen mit anderen Anwendungen konkurriert und konservative Nutzungsbeschränkungen festlegt.
- Verwenden Sie die Bulk- und Batch-Methoden von Marketo, sofern verfügbar und angemessen. Verwenden Sie nur einzelne Datensätze oder einzelne Ergebnisaufrufe, wenn erforderlich.
- Verwenden Sie [exponentielles Backoff](https://en.wikipedia.org/wiki/Exponential_backoff), um API-Aufrufe erneut auszuführen, die aufgrund von Ratenbeschränkungen oder Parallelitätsbeschränkungen fehlschlagen.
- Vermeiden Sie gleichzeitige API-Aufrufe, wenn Ihr Anwendungsfall davon nicht profitiert.

## Batching

Um eine optimale Leistung Ihrer Integrationen bei der Durchführung von Einfügungen oder Aktualisierungen sicherzustellen, sollten Datensätze in so wenig Transaktionen wie möglich gruppiert werden. Beim Abrufen von Datensätzen aus einem Datenspeicher zur Übermittlung sollten die Datensätze immer vor der Übermittlung aggregiert werden, anstatt für jede einzelne Änderung eine Anfrage zu senden.

## Akzeptable Latenz

Die Bestimmung Ihrer Latenztoleranzen oder der maximalen Zeit, die vor dem Senden eines API-Aufrufs vergehen kann, wird viele, wenn nicht die meisten Entscheidungen beeinflussen, die Sie beim Entwerfen Ihrer Integration in Marketo treffen. Marketo bietet viele verschiedene Methoden und Konfigurationsoptionen, die für verschiedene Anwendungsfälle und verschiedene Latenzklassen geeignet sind. Beispielsweise kann eine Echtzeit-Integration, die einen Vertriebsmitarbeiter über einen Benutzer benachrichtigt, der sich für eine Testversion anmeldet, Batches nur dann senden, wenn eine sofortige Nachverfolgung erforderlich ist. In den meisten Fällen ist dies jedoch nicht erforderlich und sie können zusätzliche Latenzen vertragen sowie durch Warteschlangen- und Batch-Aufrufe effizienter verwaltet werden.

| Akzeptable Latenz | Bevorzugte Methoden | Hinweise |
|---|---|---|
| Niedrig (&lt;10s) | Synchrone APIs (Batch oder Unbatch) | Stellen Sie sicher, dass dies für Ihren Anwendungsfall erforderlich ist. Das Senden sofortiger und synchroner Aufrufe für Anwendungsfälle mit hohem Volumen kann schnell ein tägliches API-Kontingent beanspruchen und möglicherweise sowohl die Rate als auch die Gleichzeitigkeitsbeschränkungen überschreiten. |
| Medium (10 - 60 Mio.) | Synchrone APIs (in Batches) | Für eingehende Datenintegrationen in Marketo wird die Verwendung einer Warteschlange mit sowohl einem Alter als auch einer Größenbeschränkung dringend empfohlen. Wenn eines der beiden Limits erreicht ist, leeren Sie die Warteschlange und senden Sie Ihre API-Anfrage mit den gesammelten Datensätzen. Dies ist ein starker Kompromiss zwischen Geschwindigkeit und Effizienz, der sicherstellt, dass Ihre Anfragen mit der erforderlichen Kadenz auftreten, während gleichzeitig so viele Datensätze in Batches aufgenommen werden, wie das Alter der Warteschlange es zulässt. |
| Hoch (>60m) | Massenimport/-export (falls unterstützt) | Bei Integrationen eingehender Daten sollten Datensätze, sofern verfügbar, in die Warteschlange gestellt und über die Massen-APIs von Marketo übermittelt werden. |

## Tägliche Beschränkungen

Jede API-fähige Instanz von Marketo verfügt über eine tägliche Zuordnung von mindestens 10.000 REST-API-Aufrufen pro Tag, aber häufiger 50.000 oder mehr, und 500 MB oder mehr der Massenextraktionskapazität. Während zusätzliche tägliche Kapazität im Rahmen eines Marketo-Abonnements erworben werden kann, sollte Ihr Anwendungsdesign die allgemeinen Beschränkungen von Marketo-Abonnements berücksichtigen.

Da die Kapazität von allen API-Services und Benutzern in einer Instanz gemeinsam genutzt wird, empfiehlt es sich, redundante Aufrufe zu eliminieren und Datensätze in so wenige Aufrufe wie möglich zu stapeln. Die aufrufeffizienteste Möglichkeit zum Importieren von Datensätzen besteht in der Verwendung der Massenimport-APIs von Marketo, die für [Leads/Personen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) und [benutzerdefinierte Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST) verfügbar sind. Marketo bietet auch Massenextraktion für [Leads](bulk-lead-extract.md) und [Aktivitäten](bulk-activity-extract.md).

### Caching

Die Ergebnisse aus den folgenden Vorgängen können in der Regel einen Tag oder länger Client-seitig zwischengespeichert werden, da sie sich selten ändern:

- Ergebnisse von Describe Operations
- [Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partitionen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

Das Zwischenspeichern bestimmter Asset-Typen wie Programme, E-Mails und Ordner ist auch für bestimmte Anwendungsfälle geeignet, z. B. die Anreicherung von Daten für Lead- oder Aktivitätsdatensätze.

## Ratenlimit

Jede Marketo-Instanz ist auf 100 Aufrufe pro 20 Sekunden beschränkt, die von allen Drittanbieter-API-Services gemeinsam genutzt werden. Wenn diese Grenze überschritten wird, antwortet die API mit einem 606-Fehler-Code, der angibt, dass die Ratengrenze überschritten wurde. Im Allgemeinen sollten Drittanbieterintegrationen ihre Nutzung auf 50 Aufrufe pro 20 Sekunden oder weniger beschränken, um eine faire Nutzung der Ratenbeschränkungen durch mehrere API-Integrationen und Benutzer zu ermöglichen. Obwohl es in bestimmten Fällen möglicherweise angebracht ist, diese Grenze zu überlasten, sind Anwendungen, die Batch-Verarbeitung verwenden und ihren Durchsatz auf einen Wert unterhalb dieser Grenze ausrichten, im Allgemeinen reaktionsschneller und konsistenter in ihrem Betrieb, was geringe Kosten für eine erhöhte Latenz mit sich bringt.

## Parallelitätslimit

Jede Marketo-Instanz verfügt über ein gemeinsames Limit von zehn gleichzeitigen REST-API-Aufrufen. Wie die täglichen Kontingent- und Quotenbegrenzungen wird es freigegeben, sodass Sie nicht davon ausgehen sollten, dass Ihre Anwendung der exklusive Verbraucher dieses Limits sein wird. Marketo zählt die Anzahl der gleichzeitigen Aufrufe als solche, die verarbeitet werden und noch nicht zurückgegeben wurden. Wenn ein Aufruf also zurückgegeben wird, wird er nicht mehr auf das Limit für gleichzeitige Aufrufe angerechnet.

Die meisten Anwendungsfälle für die Integration profitieren nicht von gleichzeitigen Aufrufen. Daher sollten Sie überlegen, ob Ihre Anwendung davon profitiert, bevor Sie sich entscheiden, gleichzeitige Anfragen an Marketo zu senden. Wenn Sie gleichzeitige Anwendungen implementieren möchten, sollten Sie die Anzahl der gleichzeitigen Anfragen in Ihrem ursprünglichen Design auf fünf oder weniger begrenzen und diese Anzahl nur erhöhen, nachdem Sie festgestellt haben, dass für Ihre Anwendung mehr erforderlich ist.

## Fehler

Mit Ausnahme einiger seltener Fälle geben API-Anfragen den HTTP-Status-Code 200 zurück. Business-Logikfehler geben ebenfalls eine 200 zurück, enthalten jedoch detaillierte Informationen im Hauptteil der Antwort. Eine ausführliche Erläuterung finden [ unter ](error-codes.md)Fehlercodes“. Die HTTP-Ursachenphrase sollte nicht ausgewertet werden, da sie optional ist und sich ändern kann.
