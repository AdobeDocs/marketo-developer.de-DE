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

Die REST-API und SOAP-API verwenden unterschiedliche Benennungskonventionen für Lead-Felder.

## Liste von Feldnamen abrufen

Rufen Sie die Liste aller unterstützten Feldnamen ab, die in Ihren Lead-Datensätzen verfügbar sind, indem Sie den REST-Endpunkt &quot;Lead beschreiben&quot;verwenden.

## Wo wird der Feldnamentyp verwendet?

Manchmal ist es schwierig zu wissen, welchen Feldnamentyp Sie verwenden müssen, wenn Sie eine bestimmte integrationsbezogene Funktion nutzen. Im Folgenden finden Sie eine Kurzübersicht darüber, für welche Funktionen REST- oder SOAP Feldnamentypen verwendet werden.

| Funktion | Feldname Zu verwendender Typ |
|--- |--- |
| Lead-Tracking-API (Munchkin) | SOAP |
| Forms 2.0-API | SOAP |
| Listenimport (Benutzeroberfläche) | SOAP |
| Listenimport (REST-API) | REST |
| Webhook-Antwortzuordnungen | SOAP |
| Email Scripting (Velocity) | SOAP |
| SOAP-API | SOAP |
| REST-API | REST |

### Warum gibt das REST-API-Feld sfdcId immer den Wert null zurück?

Das Feld `sfdcId` ist ein Formelfeld, das fälschlicherweise in der ursprünglichen Feldzuordnung für die REST-API enthalten war. Über die REST-API abgerufene Datensätze berechnen nicht den Wert der Formelfelder, daher ist der Wert immer null. Um die tatsächliche SFDC-ID zu erfassen, sollten Sie die Felder `sfdcLeadId` und `sfdcContactId` verwenden.
