---
title: Felder
feature: REST API, Field Management
description: Erfahren Sie mehr über die Benennung von REST- und SOAP-Lead-Feldern, Listenfeldern über REST, beschreiben Sie den Lead, die Funktionszuordnung, warum die sfdcId null ist, und verwenden Sie die sfdcLeadId oder die sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 194
ht-degree: 6%

---

# Felder

Die REST-API und die SOAP-API verwenden unterschiedliche Benennungskonventionen für Lead-Felder. Verwenden Sie die für jede Integrationsfunktion erforderliche Feldnamenskonvention.

## Abrufen der Liste von Feldnamen

Verwenden Sie den REST-Endpunkt „Lead beschreiben“, um alle unterstützten Feldnamen für Lead-Datensätze abzurufen.

## Wo welcher Feldnamenstyp verwendet werden soll?

Der erforderliche Feldnamenstyp hängt von der Integrationsfunktion ab. In der folgenden Tabelle wird angegeben, ob jede Funktion REST- oder SOAP-Feldnamen verwendet.

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

Das `sfdcId` Feld ist ein Formelfeld, das in die ursprüngliche Feldzuordnung für die REST-API aufgenommen wurde. Die über die REST-API abgerufenen Datensätze berechnen keine Formelfeldwerte, sodass `sfdcId` immer null zurückgibt.

Um die SFDC-ID abzurufen, verwenden Sie die Felder `sfdcLeadId` und `sfdcContactId` .
