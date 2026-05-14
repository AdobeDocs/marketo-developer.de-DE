---
title: Aktivitäten
feature: SOAP
description: Erfahren Sie, wie Sie mit Aktivitäten mithilfe von SOAP interagieren, Lead-Aktivitäten abrufen und Lead-Änderungen mit getLeadActivities und getLeadChanges verfolgen können
exl-id: fd695ab6-e7be-4ced-89c9-c4cd2d4c2ab0
TQID: https://experienceleague.adobe.com/6zUkvoDCqlRmblFDPWzLjdwITsyWxcXrJBKbLux76WI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: 79
ht-degree: 2%

---

# Aktivitäten

Die folgenden SOAP-Aufrufe können zur Interaktion mit -Aktivitäten verwendet werden.

- [getLeadAktivitäten](getleadactivity.md)
- [getLeadChanges](getleadchanges.md)

>[!CAUTION]
>
>Ab dem 30.12.2026 schlagen Aufrufe an die `Get Lead Activities`- und `Get Lead Changes`-Endpunkte, die den `listId` enthalten, fehl (Fehlercode 1003), wenn die Target-Listen 10.000 oder mehr Leads enthalten. Um Service-Unterbrechungen zu vermeiden, stellen Sie sicher, dass -Aufrufe einen ordnungsgemäßen Umfang haben, um diese Beschränkung zu vermeiden. Siehe [Migrationshandbuch](migration.md).
