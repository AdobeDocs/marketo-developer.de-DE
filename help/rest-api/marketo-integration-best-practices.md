---
title: "Best Practices für die Marketo-Integration"
feature: REST API
description: "Best Practices für die Verwendung von Marketo-APIs."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---


# Best Practices für die Integration von Marketo

## API-Beschränkungen

- **Tägliche Quote:** Die meisten Abonnements erhalten täglich 50.000 API-Aufrufe (die täglich um 12.00 Uhr CST zurückgesetzt werden). Sie können Ihr tägliches Kontingent über Ihren Kundenbetreuer erhöhen.
- **Ratenlimit:** API-Zugriff pro Instanz auf 100 Aufrufe pro 20 Sekunden begrenzt.
- **Concurrency Limit:**  Maximal zehn gleichzeitige API-Aufrufe.
- **Stapelgröße:** Lead DB - 300 Datensätze; Asset-Abfrage - 200 Datensätze
- **REST API-Payload-Größe:** 1 MB
- **Größe der Massenimportdatei:** 10 MB
- **SOAP Max Batch Size:** 300 Datensätze
- **Massenextraktionsvorgänge:** 2 Ausführung; 10 Warteschlange (einschließlich)

## Quick Tips

- Nehmen Sie an, dass Ihre Anwendung mit anderen Anwendungen um Quota-, Rate- und Parallelitätsressourcen konkurriert und konservative Nutzungsbeschränkungen festlegt.
- Verwenden Sie die Bulk- und Batch-Methoden von Marketo, sofern verfügbar und angemessen. Verwenden Sie bei Bedarf nur einzelne Datensatz- oder einzelne Ergebnisaufrufe.
- Verwendung [exponentieller Backoff](https://en.wikipedia.org/wiki/Exponential_backoff) um API-Aufrufe erneut auszuführen, die aufgrund von Ratenbeschränkungen oder Parallelitätsbeschränkungen fehlschlagen.
- Vermeiden Sie gleichzeitige API-Aufrufe, wenn Ihr Anwendungsfall davon nicht profitiert.

## Batch

Um die beste Leistung für Ihre Integrationen sicherzustellen, sollten Datensätze bei der Durchführung von Einfügungen oder Aktualisierungen so wenig Transaktionen wie möglich zusammengefasst werden. Beim Abrufen von Datensätzen aus einem Datenspeicher zur Übermittlung sollten die Datensätze immer vor der Übermittlung aggregiert werden, anstatt für jede einzelne Änderung eine Anfrage zu senden.

## Zulässige Latenz

Die Bestimmung der Latenztoleranzen oder der maximalen Zeitdauer, die vor dem Senden eines API-Aufrufs vergehen kann, informiert viele, wenn nicht die meisten Entscheidungen, die Sie bei der Entwicklung Ihrer Integration in Marketo treffen. Marketo bietet viele verschiedene Methoden und Konfigurationsoptionen, die für verschiedene Anwendungsfälle und unterschiedliche Latenzklassen geeignet sind. Beispielsweise kann eine Echtzeit-Integration, um einen Verkäufer darüber zu informieren, dass sich ein Benutzer für eine Testphase registriert, nur Batches von einem senden, wenn eine sofortige Nachverfolgung erforderlich ist. In den meisten Fällen ist dies jedoch nicht erforderlich und kann zusätzliche Latenzzeiten tolerieren und durch Warteschlangen- und Batch-Aufrufe effizienter verwaltet werden.

| Zulässige Latenz | Bevorzugte Methoden | Hinweise |
|---|---|---|
| Niedrig (&lt;10 s) | Synchrone APIs (gestapelt oder nicht gestapelt) | Stellen Sie sicher, dass dies für Ihren Anwendungsfall erforderlich ist. Das Senden sofortiger und synchroner Aufrufe für Anwendungsfälle mit hohem Volumen kann schnell ein tägliches API-Kontingent nutzen und möglicherweise sowohl die Rate- als auch die Parallelitätsbeschränkungen überschreiten. |
| Mittel(10 s - 60 m) | Synchrone APIs (Batch) | Bei eingehenden Datenintegrationen in Marketo wird dringend empfohlen, eine Warteschlange mit einer Altersgrenze und einer Größenbeschränkung zu verwenden. Wenn eine der beiden Beschränkungen erreicht ist, leeren Sie die Warteschlange und senden Sie Ihre API-Anfrage mit den gesammelten Datensätzen. Dies ist ein starker Kompromiss zwischen Geschwindigkeit und Effizienz, der sicherstellt, dass Ihre Anforderungen in der gewünschten Warteschlange auftreten, während die Stapelverarbeitung von so vielen Datensätzen erfolgt, wie das Alter der Warteschlange zulässt. |
| High(>60m) | Massenimport/-export (falls unterstützt) | Bei eingehenden Datenintegrationen sollten Datensätze in die Warteschlange gestellt und, wann immer verfügbar, über Marketo Bulk-APIs übermittelt werden. |

## Tägliche Beschränkungen

Jede API-fähige Instanz von Marketo weist täglich mindestens 10.000 REST-API-Aufrufe zu, jedoch häufiger 50.000 oder mehr und 500 MB oder mehr der Massenextraktionskapazität. Während im Rahmen eines Marketo-Abonnements zusätzliche tägliche Kapazität erworben werden kann, sollten bei Ihrem Anwendungsdesign die üblichen Beschränkungen für Marketo-Abonnements berücksichtigt werden.

Da die Kapazität von allen API-Diensten und Benutzern in einer Instanz gemeinsam genutzt wird, empfiehlt es sich, redundante Aufrufe zu eliminieren und Datensätze in so wenige Aufrufe wie möglich aufzuteilen. Die aufrufeffizienteste Methode zum Importieren von Datensätzen ist die Verwendung der Massenimport-APIs von Marketo, die für [Leads/Personen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) und [Benutzerdefinierte Objekte](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo bietet auch die Massenextraktion für [Leads](bulk-lead-extract.md) und [Tätigkeiten](bulk-activity-extract.md).

### Zwischenspeicherung

Die Ergebnisse der folgenden Vorgänge können normalerweise für einen oder mehrere Tage clientseitig zwischengespeichert werden, da sie sich selten ändern:

- Ergebnisse aus beschreibenden Vorgängen
- [Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partitionen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

Das Zwischenspeichern bestimmter Asset-Typen, wie Programme, E-Mails und Ordner, ist auch für bestimmte Anwendungsfälle geeignet, z. B. die Anreicherung von Daten für Lead- oder Aktivitätsdatensätze.

## Ratenbeschränkung

Jede Marketo-Instanz hat eine Ratenbegrenzung von 100 Aufrufen pro 20 Sekunden, die von allen Drittanbieter-API-Diensten gemeinsam genutzt wird. Wenn diese Grenze überschritten wird, antwortet die API mit einem Fehlercode 606, der angibt, dass die Ratenbegrenzung überschritten wurde. Im Allgemeinen sollten Drittanbieterintegrationen ihre Nutzung auf 50 Aufrufe pro 20 Sekunden oder weniger beschränken, um eine gerechte Nutzung der Ratenbeschränkungen durch mehrere API-Integrationen und Benutzer zu ermöglichen. Obwohl es in bestimmten Fällen angebracht sein kann, diese Grenze zu überschreiben, sind Anwendungen, die die Stapelverarbeitung verwenden und ihren Durchsatz auf weniger als diese Grenze ausrichten, im Allgemeinen responsiver und konsistenter in ihrem Betrieb, was mit einer geringen Latenzzeit verbunden ist.

## Begrenzung der Parallelität

Jede Marketo-Instanz hat eine gemeinsame Beschränkung von zehn gleichzeitig ausgeführten REST-API-Aufrufen. Wie bei den täglichen Kontingenten und Quotenbegrenzungen wird auch diese Grenze gemeinsam verwendet. Daher sollten Sie nicht davon ausgehen, dass Ihre Anwendung der ausschließliche Verbraucher dieser Grenze ist. Marketo zählt die Anzahl gleichzeitiger Aufrufe als solche, die verarbeitet werden und noch nicht zurückgegeben wurden. Wenn also ein Aufruf zurückgegeben wird, wird er nicht mehr mit dem Grenzwert für gleichzeitige Aufrufe gezählt.

Bei den meisten Anwendungsfällen für die Integration profitieren Sie nicht von gleichzeitigen Aufrufen. Überlegen Sie daher, ob Ihre Anwendung davon profitiert, bevor Sie entscheiden, gleichzeitige Anfragen an Marketo zu senden. Wenn Sie Parallelität implementieren möchten, sollten Sie die Anzahl gleichzeitiger Anforderungen in Ihrem anfänglichen Design auf maximal fünf begrenzen und diese Anzahl erst erhöhen, nachdem Sie festgestellt haben, dass Ihre Anwendung mehr erfordert.

## Fehler

Mit Ausnahme einiger seltener Fälle geben API-Anfragen den HTTP-Status-Code 200 zurück. Geschäftslogikfehler geben auch den Wert 200 zurück, enthalten jedoch detaillierte Informationen im Text der Antwort. Siehe [Fehlercodes](error-codes.md) für eine ausführliche Erläuterung. Der HTTP-Begründungssatz sollte nicht ausgewertet werden, da er optional ist und geändert werden kann.
