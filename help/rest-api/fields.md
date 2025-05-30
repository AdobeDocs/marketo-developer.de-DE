---
title: Felder
feature: REST API, Field Management
description: Eine Liste der unterstützten Feldnamen.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 6%

---

# Felder

Die REST-API und die SOAP-API verwenden unterschiedliche Benennungskonventionen für Lead-Felder.

## Abrufen der Liste von Feldnamen

Rufen Sie die Liste aller unterstützten Feldnamen ab, die in Ihren Lead-Datensätzen verfügbar sind, indem Sie den REST-Endpunkt „Lead beschreiben“ verwenden.

## Wo welcher Feldnamenstyp verwendet werden soll?

Manchmal ist es schwierig zu wissen, welchen Feldnamenstyp Sie bei der Verwendung einer bestimmten integrationsbezogenen Funktion verwenden müssen. Im Folgenden finden Sie eine Kurzübersicht darüber, welche Funktionen REST- oder SOAP-Feldnamenstypen verwenden.

| Funktion | Zu verwendender Feldnamenstyp |
|--- |--- |
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
