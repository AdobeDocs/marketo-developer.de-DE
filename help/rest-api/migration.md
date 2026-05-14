---
title: Lead-API-Aktualisierungen abrufen
feature: REST API
description: Erfahren Sie mehr über Änderungen an den Beschränkungen für die Endpunkte „Lead-Aktivitäten abrufen“ und „Lead-Änderungen abrufen“.
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Lead-API-Aktualisierungen abrufen

Ab dem 30. September 2026 schlagen Aufrufe der Endpunkte [Lead-Aktivitäten abrufen](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) oder [Lead-Änderungen abrufen](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) fehl, die den `listId`-Parameter enthalten, wenn die Ziellisten 10.000 oder mehr Leads enthalten, wobei der Fehler-Code 1003 angibt, dass die statische Zielliste zu viele Datensätze enthält. Vor Kurzem wurden ein oder mehrere API-Aufrufe durchgeführt, die von dieser Änderung betroffen wären. Um Service-Unterbrechungen zu vermeiden, müssen Sie die Art und Weise, wie Ihre Programme mit Marketo integriert werden, möglicherweise bis zum 30. September 2026 aktualisieren.

Diese Abfragetypen erstellen häufig Suchvorgänge, die entweder keine potenziellen Ergebnisse aufweisen oder eine Zeitüberschreitung aufweisen, bevor Ergebnisse gefunden werden. Durch die Begrenzung der Größe des Datensatzes wird die Reaktionsfähigkeit dieser Abfragetypen verbessert und sichergestellt, dass eine Suche nach dem Datensatz zeitnah abgeschlossen werden kann.

## Wie kann ich feststellen, ob ich betroffen bin?

Diese Änderung betrifft nur eine geringe Anzahl von Marketo Engage-Instanzen. Administratoren der betroffenen Abonnements werden innerhalb der Anwendung benachrichtigt, bevor die Änderung angewendet wird.

## Was muss ich tun?

Sie sollten dieses Dokument für die Personen oder das Team freigeben, die für Ihre Marketo Engage-Integrationen verantwortlich sind.

Je nach Anwendungsfall gibt es zwei grundlegende Optionen, um Ihre Anwendung zu migrieren:

* Begrenzen Sie statische Listen, aus denen Sie Aktivitäten extrahieren, auf maximal 10.000 Mitglieder. Sie können jede Ihrer vorhandenen Listen in kleinere Listen aufteilen, um dieselbe Zielgruppe weiterhin für Aktivitäten abzurufen.
* Extrahieren Sie Ihre Aktivitäten oder Datenwertänderungen mithilfe der Massenaktivität „Extrahieren“ oder „Datenströme“ und verbinden Sie diese Ergebnisse mit der statischen Listenmitgliedschaft mit [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) oder [Massenlead-Extraktion](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract)

## Was passiert, wenn ich nichts tue?

Bei der Abfrage nach Aktivitäten aus statischen Listen mit einer großen Anzahl von Mitgliedern kann es aufgrund nicht behandelter Fehler zu Funktionsstörungen Ihrer API-Integrationen kommen.
