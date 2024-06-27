---
title: Swagger-Definitionen herunterladen
feature: REST API, Programs
description: Laden Sie die Swagger-Definitionsdateien für den lokalen Gebrauch herunter.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Swagger-Definitionen herunterladen

[Swagger](https://swagger.io/) oder [OpenAPI](https://www.openapis.org/) -Definitionen sind Datendateien, die alle Methoden und Parameter einer REST-API beschreiben. Sie können diese Datendateien lokal als Ihre persönliche API-Referenz herunterladen und verwenden.

Um die Marketo Engage Swagger/OpenAPI-Definitionen zu verwenden, muss die `host` -Feld jedes Dokuments muss aktualisiert werden, um mit dem REST-API-Hostnamen aus Ihrer Marketo Engage-Instanz übereinzustimmen.

Laden Sie zunächst die gewünschte OpenAPI-Definition herunter:

* [Asset](assets/swagger-asset.json)
* [Lead](assets/swagger-mapi.json)
* [Identität        ](assets/swagger-identity.json)
* [Benutzerverwaltung](assets/swagger-user.json)

Rufen Sie als Nächstes Ihren REST-API-Hostnamen vom Marketo-Administrator ab. Navigieren Sie zu _Admin_-> _Web-Services_ -Menü in Marketo Engage und kopieren Sie den Hostnamen aus dem Endpunktfeld. Die `hostname` die Zeichenfolge zwischen dem Protokoll, `https://`, und `/rest`, der aussieht wie `AAA-999-AAA.mktorest.com`

Öffnen Sie Ihre OpenAPI-Datei in einem Texteditor und suchen Sie die `host` im JSON-Feld ein und ändern Sie den Wert in Ihren REST-API-Hostnamen: `"host":"localhost:8080"` nach `"host":"AAA-999-AAA.mktorest.com"`.

Nach dem Speichern können Sie die Definitionsdatei in Ihrer Swagger-UI-Instanz oder einer anderen OpenAPI-Software ausführen.
