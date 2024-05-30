---
title: "Benutzerdefinierte Objekte"
feature: SOAP
description: "Erstellen von benutzerdefinierten Objekten."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Benutzerdefinierte Objekte

Ein benutzerdefiniertes Marketo-Objekt ermöglicht die Erstellung einer 1:n-Beziehung zwischen Ihren Marketo-Leads und den benutzerdefinierten Objektdatensätzen. Sie können beispielsweise alle Roadshows verfolgen, die von einem Lead besucht werden. Da Leads an einer Reihe von Roadshows (über mehrere Jahre) teilnehmen können, sind benutzerdefinierte Objekte besser geeignet, diese Informationen zu speichern.

Benutzerdefinierte Objekte werden in allen Marketo-Editionen außer Spark unterstützt. Wenn das benutzerdefinierte Marketo-Objekt in Ihrem Konto erfolgreich bereitgestellt wurde, können Sie

- Erstellen/Lesen/Aktualisieren/Löschen von Datensätzen im benutzerdefinierten Objekt über die Marketo SOAP API
- Verwenden Sie den Smart-List-Trigger, wenn dem benutzerdefinierten Objekt neue Datensätze hinzugefügt werden
- Verwenden der benutzerdefinierten Objektdaten als Filter in Smart-Listen
- Verwenden der benutzerdefinierten Objektdaten in E-Mails mithilfe von Marketo Email Scripting

## Struktur von benutzerdefinierten Objekten

Benutzerdefinierte Objekte bestehen aus:

- Ein kleiner Satz fester Attribute, die für alle benutzerdefinierten Objekte gelten:
   - Objektname (auch Objekttyp-Name genannt)
   - Objektbeschreibung
   - Benutzerdefiniertes Objekt zum Marketo-Lead-Link-Feldnamen - dies ist ein Feld im Lead-Objekt (Person), auf das das benutzerdefinierte Objekt verweist
   - Object Key Field Name - der von Ihrem Objekt verwendete Primärschlüssel
- Ein oder mehrere objektspezifische Felder (maximal 50 solche Felder werden unterstützt)

## Benutzerdefinierte Objektoperationen

Die folgenden Aufrufe können zur Interaktion mit COs verwendet werden.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
