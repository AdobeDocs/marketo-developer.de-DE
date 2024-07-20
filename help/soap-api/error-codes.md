---
title: Fehlercodes
feature: SOAP
description: Fehlercodes für SOAP-Aufrufe
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 11%

---

# Fehlercodes

Bei der Entwicklung für Marketo ist es sehr wichtig, dass Anfragen und Antworten protokolliert werden, wenn eine unerwartete Ausnahme auftritt.  Während bestimmte Arten von Ausnahmen, wie z. B. abgelaufene Authentifizierung, sicher durch erneute Authentifizierung verarbeitet werden können, erfordern andere möglicherweise Support-Interaktionen, und Anfragen und Antworten werden in diesem Szenario immer angefordert.

Nachstehend finden Sie eine Liste SOAP API-Fehlercodes.

| Code | Nachricht | Hinweise |
|--- |--- |--- |
| 10001 | Interner Fehler | Schweres Systemversagen |
| 20011 | Interner Fehler | API-Dienstfehler |
| 20012 | Anfrage nicht verstanden | Unerwartete SOAP |
| 20013 | Zugriff verweigert | Der Client wird vom API-Zugriff blockiert |
| 20014 | Authentifizierung fehlgeschlagen | Der Client hat keine gültigen Anmeldeinformationen angegeben. |
| 20015 | Anfragebeschränkung überschritten | Die Anzahl der Anrufe hat heute das Abonnementkontingent überschritten. Das standardmäßige Abonnementkontingent beträgt 10.000/Tag. |
| 20016 | Anfrage abgelaufen | Die Signatur der Anfrage ist zu alt. Der angegebene Zeitstempel und die Anfrageunterschrift sind in der Vergangenheit und nicht mehr gültig. Die Anfrage kann mit einem neu generierten Zeitstempel und einer neu erstellten Signatur wiederholt werden. |
| 20017 | Ungültige Anfrage | Anfrage fehlt ein erwarteter Parameter |
| 20019 | Nicht unterstützter Vorgang | Der aufgerufene Vorgang ist in der Marketo API-WSDL nicht definiert. |
| 20022 | Im Abfragefilter angegebener Zeitraum überschritten Grenzwert | Die Anzahl der Tage zwischen den Feldern &quot;äldestUpdatedAt&quot;und &quot;latestUpdatedAt&quot;war größer als 30 |
| 20023 | Ratenlimit überschritten | Die Anzahl der Aufrufe in den letzten 20 Sekunden war größer als 100 |
| 20024 | Parallelitätslimit überschritten | Die Anzahl der gleichzeitigen Aufrufe war größer als 10 |
| 20101 | Lead-Schlüssel erforderlich | LeadKey ist erforderlich, wurde jedoch nicht bereitgestellt |
| 20102 | Lead Key Bad | LeadKeyType ist nicht gültig |
| 20103 | Blei nicht gefunden | LeadKey-Wert stimmt mit keinem Lead überein |
| 20104 | Lead-Detail erforderlich | LeadRecord ist erforderlich, wurde jedoch nicht bereitgestellt |
| 20105 | Lead-Attribut ungültig | LeadRecord enthält ein Attribut mit einem schlechten Namen |
| 20106 | Lead-Synchronisation fehlgeschlagen | LeadRecord konnte nicht aktualisiert oder erstellt werden |
| 20107 | Aktivitätsschlüssel ungültig | LeadActivityFilter enthält einen ungültigen Aktivitätstyp |
| 20108 | Lead-Eigentümer nicht gefunden | LeadKey gibt einen Lead-Eigentümer an, der nicht vorhanden ist |
| 20109 | Parameter erforderlich | Parameterwert war null oder fehlt |
| 20110 | Ungültiger Parameter | Ein Parameterwert ist ungültig. |
| 20111 | Liste nicht gefunden | ListKey gibt eine Liste an, die nicht vorhanden ist |
| 20113 | Kampagne nicht gefunden | Kampagne existiert nicht |
| 20114 | Ungültiger Parameter | Parameterwert ist ungültig |
| 20122 | Fehlerhafte Stream-Position | Streamposition ist schlecht |
| 20123 | Stream am am Ende | Streamposition gibt an, dass keine Datensätze mehr verfügbar sind |
