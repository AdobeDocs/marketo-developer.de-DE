---
title: "Streamposition"
feature: SOAP
description: "Übersicht über die Dampfposition"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# Stream-Position

Stream-Positionselemente enthalten eine Positionsreferenz für einen oder mehrere logische Streams von Daten mit einer Sequenzierung. Die Positionsreferenz kann eine ungefähre externe Spezifikation wie ein Zeitstempel oder eine undurchsichtige interne Positionsspezifikation sein, die von einem vorherigen API-Aufruf zurückgegeben wird. Streampositionen können als komplexer Typ mit mehreren Elementen oder als Zeichenfolge definiert werden.

Die Stream-Position wird zum Abrufen von Daten in Stapeln verwendet und ermöglicht es dem Aufrufer, durch das Ergebnis zu paginieren. Die in einer API-Anfrage übergebene Stream-Position ist der Wert der Stream-Position, die in der vorherigen Antwort zurückgegeben wurde. Eine Änderung der Stream-Position, die vom vorherigen API-Aufruf zurückgegeben wird, wird nicht empfohlen und kann zu einem unerwarteten API-Verhalten führen.

## APIs, die die Streamposition unterstützen

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Einfache Streamposition

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Komplexe Stream-Position

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
