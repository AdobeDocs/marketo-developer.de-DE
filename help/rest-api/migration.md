---
title: Lead-API-Aktualisierungen abrufen
feature: REST API
description: Erfahren Sie mehr über Änderungen an den Beschränkungen für die Endpunkte „Lead-Aktivitäten abrufen“ und „Lead-Änderungen abrufen“.
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Lead-API-Aktualisierungen abrufen

Ab dem 30. September 2026 schlagen Aufrufe der Endpunkte [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) oder [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) mit dem `listId`-Parameter fehl, wenn die Ziellisten 10.000 oder mehr Leads enthalten. Die Endpunkte geben einen 1003-Fehler-Code zurück, der angibt, dass die statische Zielliste zu viele Datensätze enthält.

Diese Änderung würde sich auf einen oder mehrere aktuelle API-Aufrufe auswirken. Um Service-Unterbrechungen zu vermeiden, müssen Sie die Integration Ihrer Programme mit Marketo möglicherweise bis zum 30. September 2026 aktualisieren.

Diese Abfragen erstellen oft Suchvorgänge, die keine potenziellen Ergebnisse aufweisen, oder eine Zeitüberschreitung, bevor Ergebnisse gefunden werden. Durch die Begrenzung der eingestellten Größe wird die Reaktionsfähigkeit der Abfrage verbessert und Suchvorgänge können zeitnah abgeschlossen werden.

## Wie kann ich feststellen, ob ich betroffen bin?

Diese Änderung betrifft nur eine geringe Anzahl von Marketo Engage-Instanzen. Administratoren betroffener Abonnements erhalten eine Benachrichtigung im Programm, bevor die Änderung angewendet wird.

## Was muss ich tun?

Geben Sie dieses Dokument für die Personen oder das Team frei, die für Ihre Marketo Engage-Integrationen verantwortlich sind.

Verwenden Sie je nach Anwendungsfall eine der folgenden Migrationsoptionen:

* Begrenzen Sie statische Listen, die für die Aktivitätsextraktion verwendet werden, auf 10.000 Mitglieder. Teilen Sie bestehende Listen in kleinere Listen auf, um dieselbe Zielgruppe weiterhin für Aktivitäten abzurufen.
* Extrahieren von Aktivitäten oder Datenwertänderungen mithilfe von Massenaktivitäten oder Extrahieren von Datenströmen. Verbinden Sie die Ergebnisse mit der statischen Listenmitgliedschaft mit [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) oder [Bulk Lead Extract](https://experienceleague.adobe.com/de/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract).

## Was passiert, wenn ich nichts tue?

Ihre API-Integrationen werden möglicherweise durch nicht behandelte Fehler unterbrochen, wenn Aktivitäten aus statischen Listen mit einer großen Anzahl von Mitgliedern abgefragt werden.
