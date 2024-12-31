---
title: Swagger-Definitionen herunterladen
feature: REST API, Programs
description: Laden Sie Swagger-Definitionsdateien für den lokalen Gebrauch herunter.
exl-id: c2bed094-36f9-47e7-a6d5-c237e425966a
source-git-commit: fb95ac67e7fbabe772341d78af7c5cbf3721f66d
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Swagger-Definitionen herunterladen

[Swagger](https://swagger.io/) oder [OpenAPI](https://www.openapis.org/) sind Datendateien, die alle Methoden und Parameter einer REST-API beschreiben. Sie können diese Datendateien lokal als persönliche API-Referenz herunterladen und nutzen.

Um die Marketo Engage-Swagger-/OpenAPI-Definitionen zu verwenden, muss das `host` jedes Dokuments aktualisiert werden, damit es dem REST-API-Hostnamen Ihrer Marketo Engage-Instanz entspricht.

Laden Sie zunächst die OpenAPI-Definition herunter, die Sie verwenden möchten:

* [Asset](assets/swagger-asset.json)
* [Lead](assets/swagger-mapi.json)
* [Identität](assets/swagger-identity.json)
* [Benutzerverwaltung](assets/swagger-user.json)

Rufen Sie als Nächstes Ihren REST-API-Hostnamen vom Marketo-Administrator ab. Gehen Sie zum Menü _Admin_> _Web-Services_ in Marketo Engage und kopieren Sie den Host-Namen aus dem Feld Endpunkt . Der `hostname` ist die Zeichenfolge zwischen Protokoll, `https://` und `/rest`. Sie sieht wie `AAA-999-AAA.mktorest.com` aus

Öffnen Sie Ihre OpenAPI-Datei in einem Texteditor. Suchen Sie das `host` Feld in der JSON-Datei und ändern Sie seinen Wert in Ihren REST API-Hostnamen: `"host":"localhost:8080"` in `"host":"AAA-999-AAA.mktorest.com"`.

Nach dem Speichern können Sie die Definitionsdatei in Ihrer Instanz der Swagger-Benutzeroberfläche oder einer anderen OpenAPI-Software ausführen.
