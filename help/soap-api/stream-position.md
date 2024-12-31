---
title: Stromposition
feature: SOAP
description: Dampfposition - Übersicht
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Stromposition

Stream-Positionselemente enthalten eine Positionsreferenz für einen oder mehrere logische Streams von zeitsequenzierten Daten. Die Positionsreferenz kann eine ungefähre externe Spezifikation wie ein Zeitstempel oder eine undurchsichtige interne Spezifikation der Position sein, die von einem vorherigen API-Aufruf zurückgegeben wird. Stream-Positionen können als komplexer, mehrelementarer Typ definiert sein oder eine Zeichenfolge sein.

Die Stream-Position wird verwendet, um Daten in Batches abzurufen, und ermöglicht es dem Aufrufer, durch das Ergebnis zu paginieren. Die innerhalb einer API-Anfrage übergebene Stream-Position ist der Wert der Stream-Position, die in der vorherigen Antwort zurückgegeben wurde. Das Ändern der Stream-Position, die vom vorherigen API-Aufruf zurückgegeben wurde, wird nicht empfohlen und kann zu unerwartetem API-Verhalten führen.

## APIs zur Unterstützung der Stream-Position

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Einfache Stream-Position

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## komplexe Fließposition

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
