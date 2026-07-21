---
title: Best Practices für die Marketo-Integration
feature: REST API
description: Best Practices für Marketo-API-Integrationen, einschließlich Kontingenten, Rate- und Gleichzeitigkeitsbeschränkungen, Batching, Massenimport und -export, Caching und Latenzplanung.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
TQID: https://experienceleague.adobe.com/Ld-rmFCwKSx-0W2-ceYICu0FQHK8BKAC1QgqtiOWDn4
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 882
ht-degree: 0%

---

# Best Practices für die Marketo-Integration

Entwerfen Sie Integrationen um die freigegebenen API-Beschränkungen für Ihre Marketo-Instanz. Verwenden Sie Batch-Verarbeitung, Caching und konservative Anfrageraten, um den Durchsatz und die Zuverlässigkeit zu verbessern.

## API-Beschränkungen

- **Tägliches Kontingent:** Die meisten Abonnements erhalten 50.000 API-Aufrufe pro Tag. Das Kontingent wird täglich um 0:00 Uhr CST zurückgesetzt. Wenden Sie sich an Ihren Account Manager, um das tägliche Kontingent zu erhöhen.
- **Ratenlimit:** Jede Instanz ist auf 100 API-Aufrufe pro 20 Sekunden beschränkt.
- **Parallelitätslimit:** Jede Instanz ermöglicht maximal zehn gleichzeitige API-Aufrufe.
- **Batch-Größe:** Lead-Datenbank unterstützt 300 Datensätze; Asset-Abfrage unterstützt 200 Datensätze.
- **REST API Payload-Größe:** 1 MB.
- **Größe der Massenimportdatei:** 10 MB.
- Maximale Batch-Größe von **SOAP:** 300 Datensätze.
- **Massenextraktionsaufträge:** Zwei werden ausgeführt und zehn in die Warteschlange gestellt, einschließlich.

## Schnelltipps

- Legen Sie konservative Nutzungsbeschränkungen fest, da Ihre Anwendung Kontingent-, Rate- und Parallelitätsressourcen mit anderen Anwendungen teilt.
- Verwenden Sie Massen- und Batch-Methoden von Marketo, sofern verfügbar. Verwenden Sie Aufrufe mit einem Datensatz oder mit einem Ergebnis nur bei Bedarf.
- Verwenden Sie [exponentielles Backoff](https://en.wikipedia.org/wiki/Exponential_backoff), um API-Aufrufe erneut auszuführen, die aufgrund von Ratenbeschränkungen oder Parallelitätsbeschränkungen fehlschlagen.
- Vermeiden Sie gleichzeitige API-Aufrufe, es sei denn, sie kommen Ihrem Anwendungsfall zugute.

## Batching

Um Datensätze einzufügen und zu aktualisieren, gruppieren Sie sie in so wenig Transaktionen wie möglich. Wenn Sie Datensätze aus einem Datenspeicher abrufen, aggregieren Sie sie vor der Übermittlung, anstatt für jede Änderung eine Anfrage zu senden.

## Akzeptable Latenz

Definieren Sie beim Entwerfen einer Integration die akzeptable Latenz (die maximale Zeit vor dem Senden eines API-Aufrufs). Diese Auswahl bestimmt, welche Marketo-Methoden und Konfigurationsoptionen für den Anwendungsfall geeignet sind.

Eine Echtzeit-Integration, die einen Verkäufer benachrichtigt, wenn ein Benutzer eine Testversion startet, kann beispielsweise Batches einer Testversion senden, wenn eine sofortige Nachverfolgung erforderlich ist. Die meisten Anwendungsfälle tolerieren mehr Latenz und arbeiten effizienter, indem sie Warteschlangen- und Batch-Aufrufe verarbeiten.

| Akzeptable Latenz | Bevorzugte Methoden | Hinweise |
| --- | --- | --- |
| Niedrig (&lt;10s) | Synchrone APIs (Batch oder Unbatch) | Stellen Sie sicher, dass dies für Ihren Anwendungsfall erforderlich ist. Das Senden sofortiger und synchroner Aufrufe für Anwendungsfälle mit hohem Volumen kann schnell ein tägliches API-Kontingent beanspruchen und möglicherweise sowohl die Rate als auch die Gleichzeitigkeitsbeschränkungen überschreiten. |
| Medium (10 - 60 Mio.) | Synchrone APIs (in Batches) | Für eingehende Datenintegrationen in Marketo wird die Verwendung einer Warteschlange mit sowohl einem Alter als auch einer Größenbeschränkung dringend empfohlen. Wenn eines der beiden Limits erreicht ist, leeren Sie die Warteschlange und senden Sie Ihre API-Anfrage mit den gesammelten Datensätzen. Dies ist ein starker Kompromiss zwischen Geschwindigkeit und Effizienz, der sicherstellt, dass Ihre Anfragen mit der erforderlichen Kadenz auftreten, während gleichzeitig so viele Datensätze in Batches aufgenommen werden, wie das Alter der Warteschlange es zulässt. |
| Hoch (>60m) | Massenimport/-export (falls unterstützt) | Bei Integrationen eingehender Daten sollten Datensätze, sofern verfügbar, in die Warteschlange gestellt und über die Massen-APIs von Marketo übermittelt werden. |

## Tägliche Beschränkungen

Jede API-fähige Marketo-Instanz verfügt über eine tägliche Zuordnung von mindestens 10.000 REST-API-Aufrufen, obwohl 50.000 oder mehr häufig sind. Jede Instanz verfügt außerdem über eine Massenextraktionskapazität von mindestens 500 MB. Im Rahmen eines Marketo-Abonnements können zusätzliche tägliche Kapazitäten erworben werden. Bei der Entwicklung von Anwendungen sollten jedoch die üblichen Abonnementbeschränkungen berücksichtigt werden.

Die Kapazität wird von allen API-Services und Benutzern in einer Instanz gemeinsam genutzt. Beseitigen Sie redundante Aufrufe und Batch-Datensätze in so wenig Aufrufe wie möglich.

Die aufrufeffizienteste Importmethode ist die Marketo-Massenimport-API, die für „Leads[/Personen“ ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) &quot;[ Objekte“ ](https://developer.adobe.com/marketo-apis/api/mapi#tag/Snippets/operation/createSnippetUsingPOST). Marketo bietet auch Massenextraktion für [Leads](bulk-lead-extract.md) und [Aktivitäten](bulk-activity-extract.md).

### Caching

Die Ergebnisse aus den folgenden Vorgängen können in der Regel einen Tag oder länger Client-seitig zwischengespeichert werden, da sie sich selten ändern:

- Ergebnisse von Describe Operations
- [Aktivitätstypen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partitionen](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadPartitionsUsingGET)

Für Anwendungsfälle wie die Anreicherung von Lead- oder Aktivitätsdaten können Sie auch Asset-Typen wie Programme, E-Mails und Ordner zwischenspeichern.

## Ratenlimit

Jede Marketo-Instanz verfügt über ein Limit für die freigegebene Rate von 100 Aufrufen pro 20 Sekunden für alle Drittanbieter-API-Services. Wenn -Aufrufe dieses Limit überschreiten, gibt die API einen 606-Fehler-Code zurück.

Beschränken Sie im Allgemeinen jede Drittanbieterintegration auf 50 Aufrufe pro 20 Sekunden oder weniger, damit mehrere API-Integrationen und Benutzer die verfügbare Kapazität gemeinsam nutzen können. Einige Anwendungsfälle benötigen möglicherweise das volle Limit. Anwendungen, die Batch verwenden und einen niedrigeren Durchsatz anstreben, sind jedoch im Allgemeinen reaktionsschneller und konsistenter und weisen einen geringen Latenzanstieg auf.

## Parallelitätslimit

Jede Marketo-Instanz verfügt über ein gemeinsames Limit von zehn gleichzeitigen REST-API-Aufrufen. Gehen Sie nicht davon aus, dass Ihre Anwendung der einzige Benutzer dieses Limits ist.

Marketo zählt Aufrufe, die verarbeitet werden und noch nicht zurückgegeben wurden. Wenn ein Aufruf zurückgegeben wird, wird er nicht mehr für das Limit für gleichzeitige Aufrufe gezählt.

Die meisten Integrationen profitieren nicht von gleichzeitigen Aufrufen. Wenn Sie parallele Verarbeitung implementieren, beschränken Sie die Anwendung zunächst auf fünf oder weniger gleichzeitige Anfragen. Erhöhen Sie das Limit erst, nachdem Sie festgestellt haben, dass die Anwendung mehr erfordert.

## Fehler

Außer in seltenen Fällen geben API-Anfragen den HTTP-Status-Code 200 zurück. Business-Logikfehler geben auch 200 zurück, enthalten jedoch Details im Antworttext. Siehe [Fehlercodes](error-codes.md) für weitere Informationen.

Bewerten Sie die HTTP-Ursachenphrase nicht, da sie optional ist und sich ändern kann.
