---
title: Benutzerdefinierte Objekte
feature: SOAP
description: Erfahren Sie, wie benutzerdefinierte Marketo-Objekte einen Lead mit vielen Datensätzen verknüpfen, mit Struktur, Beschränkungen und SOAP-API-Aufrufen für GET, SYNC, DELETE sowie die Verwendung von Smart List und E-Mail.
exl-id: 29d65841-4b44-4d94-b14e-c583d433d015
TQID: https://experienceleague.adobe.com/Fy3h8qfKs8gizeakXkhoj5mJhib-QA2kYOu41YMIEZs
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 286
ht-degree: 2%

---

# Benutzerdefinierte Objekte

Ein benutzerdefiniertes Marketo-Objekt ermöglicht die Erstellung einer 1:n-Beziehung zwischen Ihren Marketo-Leads und den benutzerdefinierten Objektdatensätzen. Sie können beispielsweise alle Roadshows verfolgen, an denen ein Lead teilgenommen hat. Da Leads möglicherweise an einer Reihe von Roadshows (über mehrere Jahre) teilnehmen, eignen sich benutzerdefinierte Objekte besser zum Speichern dieser Informationen.

Benutzerdefinierte Objekte werden in allen Marketo-Editionen mit Ausnahme von Spark unterstützt. Wenn das benutzerdefinierte Marketo-Objekt erfolgreich in Ihrem Konto bereitgestellt wurde, können Sie

- Erstellen/Lesen/Aktualisieren/Löschen von Datensätzen im benutzerdefinierten Objekt über die Marketo SOAP-API
- Smart-Listen-Trigger verwenden, wenn dem benutzerdefinierten Objekt neue Datensätze hinzugefügt werden
- Verwenden der benutzerdefinierten Objektdaten als Filter in Smart Lists
- Verwenden der benutzerdefinierten Objektdaten in E-Mails mit Marketo Email Scripting

## Struktur benutzerdefinierter Objekte

Benutzerdefinierte Objekte bestehen aus:

- Ein kleiner Satz fester Attribute, die allen benutzerdefinierten Objekten gemeinsam sind:
   - Objektname (auch als Objekttyp-Name bezeichnet)
   - Objektbeschreibung
   - Benutzerdefiniertes Objekt für Marketo-Lead-Link-Feldname : Dies ist ein Feld auf dem Lead (Person)-Objekt, auf das das benutzerdefinierte Objekt verweist
   - Feldname für Objektschlüssel - der von Ihrem Objekt verwendete Primärschlüssel
- Ein oder mehrere objektspezifische Felder (wir unterstützen maximal 50 solcher Felder)

## Vorgänge bei benutzerdefinierten Objekten

Die folgenden Aufrufe können zur Interaktion mit COs verwendet werden.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
