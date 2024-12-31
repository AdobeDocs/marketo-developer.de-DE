---
title: Fehler-Codes
feature: SOAP
description: Fehlercodes für SOAP-Aufrufe
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 11%

---

# Fehler-Codes

Bei der Entwicklung für Marketo ist es sehr wichtig, dass Anfragen und Antworten protokolliert werden, wenn eine unerwartete Ausnahme auftritt.  Während bestimmte Arten von Ausnahmen, z. B. abgelaufene Authentifizierungen, sicher durch erneute Authentifizierung verarbeitet werden können, erfordern andere möglicherweise Support-Interaktionen, und Anfragen und Antworten werden immer in diesem Szenario angefordert.

Nachfolgend finden Sie eine Liste der SOAP-API-Fehler-Codes.

| Code | Nachricht | Hinweise |
|--- |--- |--- |
| 10001 | Interner Fehler | Schwerwiegender Systemausfall |
| 20011 | Interner Fehler | API-Dienstfehler |
| 20012 | Anfrage nicht verstanden | Unerwartete SOAP-Nachricht |
| 20013 | Zugriff verweigert | Client ist vom API-Zugriff blockiert |
| 20014 | Authentifizierung fehlgeschlagen | Client hat keine gültigen Anmeldeinformationen angegeben |
| 20015 | Anfragelimit überschritten | Die Anzahl der Aufrufe hat heute das Kontingent des Abonnements überschritten. Das Standardabonnement-Kontingent beträgt 10.000/Tag. |
| 20016 | Anfrage abgelaufen | Signaturanfrage ist zu alt. Der angegebene Zeitstempel und die Anfragesignatur liegen in der Vergangenheit und sind nicht mehr gültig. Die Anfrage kann mit einem neu generierten Zeitstempel und einer neu generierten Signatur erneut versucht werden. |
| 20017 | Ungültige Anfrage | Bei der Anfrage fehlt ein erwarteter Parameter |
| 20019 | Nicht unterstützter Vorgang | Der aufgerufene Vorgang ist nicht in der Marketo-API-WSDL definiert. |
| 20022 | Im Abfragefilter angegebene Zeitspanne hat das Limit überschritten | Die Anzahl der Tage zwischen den Feldern „oldestUpdatedAt“ und „latestUpdatedAt“ war größer als 30 |
| 20023 | Ratenlimit überschritten | Die Anzahl der Aufrufe in den letzten 20 Sekunden war größer als 100 |
| 20024 | Parallelitätslimit überschritten | Die Anzahl der gleichzeitigen Aufrufe war größer als 10 |
| 20101 | Lead-Schlüssel erforderlich | LeadKey ist erforderlich, wurde aber nicht angegeben |
| 20102 | Lead-Schlüssel schlecht | LeadKeyType ist ungültig |
| 20103 | Lead nicht gefunden | LeadKey-Wert stimmte mit keinem Lead überein |
| 20104 | Lead-Detail erforderlich | LeadRecord ist erforderlich, wurde aber nicht angegeben |
| 20105 | Lead-Attribut ungültig | LeadRecord enthält ein Attribut mit einem ungültigen Namen |
| 20106 | Lead-Synchronisation fehlgeschlagen | LeadRecord konnte nicht aktualisiert oder erstellt werden |
| 20107 | Aktivitätsschlüssel ungültig | LeadActivityFilter enthält einen ungültigen Aktivitätstyp |
| 20108 | Lead-Besitzer nicht gefunden | LeadKey gibt einen Lead-Inhaber an, der nicht existiert |
| 20109 | Parameter erforderlich | Parameterwert war null oder fehlt |
| 20110 | Ungültiger Parameter | Ein Parameterwert ist ungültig |
| 20111 | Liste nicht gefunden | ListKey gibt eine Liste an, die nicht vorhanden ist |
| 20113 | Kampagne nicht gefunden | Kampagne existiert nicht |
| 20114 | Ungültiger Parameter | Parameterwert ist ungültig |
| 20122 | Fehlerhafte Stream-Position | Stream-Position ist schlecht |
| 20123 | Stream am Ende | Stream-Position gibt an, dass keine Datensätze mehr verfügbar sind |
