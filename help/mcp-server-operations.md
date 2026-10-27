---
title: Marketo Engage MCP-Vorgänge
description: Erfahren Sie, welche Marketo Engage-MCP-Vorgänge für die Verwendung mit KI-Assistenten verfügbar sind.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%
---

# [!DNL Marketo Engage] MCP-Vorgänge

Der [!DNL Marketo Engage] MCP-Server unterstützt 182 Vorgänge in den Bereichen Asset-Management, Lead-Daten und Kampagnenautomatisierung.



## Unterstützte Vorgänge

Sie können verfügbare Vorgänge über Ihren Connected AI-Assistenten ermitteln.

Wenn Ihre Verbindung die Erkennungs-Tools verfügbar macht, verwenden Sie die folgenden Eingabeaufforderungen:

* „Verwenden Sie das Marketo MCP-`list_tool_categories`-Tool, um die verfügbaren Vorgangskategorien aufzulisten.“
* „Verwenden Sie das Marketo MCP-`search_tools`-Tool, um Vorgänge zum Verwalten von Smart-Kampagnen zu finden.“

Ersetzen Sie die Aufgabe in der zweiten Eingabeaufforderung durch den gewünschten Vorgang. Wenn die Erkennungs-Tools nicht verfügbar sind, bitten Sie Ihren KI-Assistenten, die vom [!DNL Marketo] MCP-Server verfügbaren Tools aufzulisten.

Die Ergebnisse spiegeln die Berechtigungen und die Verfügbarkeit der Funktionen Ihrer Verbindung wider. Sie beschreiben die verfügbaren MCP-Tools, nicht die vollständige [!DNL Marketo] REST-API.

Für Verbindungen, die [!DNL Marketo] Client-Anmeldeinformationen verwenden, werden die folgenden Funktionen nicht unterstützt:

* SMS-Tools
* Push-Benachrichtigungs-Tools
* In-App-Nachrichten- und In-App-Programm-Tools
* Planen von Smart-Kampagnen mit `schedule_smart_campaign`
* Tools für Benutzerverwaltung und Massendienst

Der `schedule_campaign` wird für Verbindungen mit Client-Anmeldeinformationen unterstützt.

Die verfügbaren Vorgänge hängen von Ihren API-Benutzerberechtigungen, den für Ihre [!DNL Marketo]-Instanz aktivierten Funktionen und allen Einschränkungen in Ihrem MCP-Client ab. Einige Vorgänge, die Daten ändern oder löschen, erfordern eine Bestätigung und zusätzlichen Zugriff. Unterstützung für eine Kategorie bedeutet nicht, dass auf jeden REST-Endpunkt zugegriffen werden kann.

Anweisungen zur Verbindung und Beispielaufforderungen finden Sie auf der Seite [MCP Server](mcp-server.md#available-operations). Informationen zum Umgang mit Daten mit [!DNL Marketo] AI und dem [!DNL Marketo Engage] MCP-Server finden Sie auf der Seite [Dateninformationen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).