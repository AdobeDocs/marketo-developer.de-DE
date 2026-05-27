---
title: Felder
feature: REST API, Field Management
description: Erfahren Sie mehr über die Benennung von REST- und SOAP-Lead-Feldern, Listenfeldern über REST, beschreiben Sie den Lead, die Funktionszuordnung, warum die sfdcId null ist, und verwenden Sie die sfdcLeadId oder die sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 6%

---

# Felder

Die REST-API und die SOAP-API verwenden unterschiedliche Benennungskonventionen für Lead-Felder.

## Abrufen der Liste von Feldnamen

Rufen Sie die Liste aller unterstützten Feldnamen ab, die in Ihren Lead-Datensätzen verfügbar sind, indem Sie den REST-Endpunkt „Lead beschreiben“ verwenden.

## Wo welcher Feldnamenstyp verwendet werden soll?

Manchmal ist es schwierig zu wissen, welchen Feldnamenstyp Sie bei der Verwendung einer bestimmten integrationsbezogenen Funktion verwenden müssen. Im Folgenden finden Sie eine Kurzübersicht darüber, welche Funktionen REST- oder SOAP-Feldnamenstypen verwenden.

| Funktion | Zu verwendender Feldnamenstyp |
| --- | --- |
| Lead-Tracking-API (Munchkin) | SOAP |
| Forms 2.0-API | SOAP |
| Listen-Import (Benutzeroberfläche) | SOAP |
| Listen-Import (REST-API) | RUHE |
| Webhook-Antwortzuordnungen | SOAP |
| E-Mail-Scripting (Velocity) | SOAP |
| SOAP-API | SOAP |
| REST-API | RUHE |

### Warum gibt das REST-API-Feld sfdcId immer den Wert null zurück?

Das Feld `sfdcId` ist ein Formelfeld, das fälschlicherweise in die ursprüngliche Feldzuordnung für die REST-API aufgenommen wurde. Die über die REST-API abgerufenen Datensätze berechnen den Wert der Formelfelder nicht, sodass der Wert immer null ist. Um die tatsächliche SFDC-ID zu erfassen, sollten Sie die Felder `sfdcLeadId` und `sfdcContactId` verwenden.
