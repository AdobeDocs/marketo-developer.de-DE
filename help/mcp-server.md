---
title: MCP-Server für Marketo Engage
description: Erfahren Sie, wie Sie einen KI-Assistenten mithilfe des Marketo Engage MCP-Servers mit Marketo verbinden. Konfigurieren Sie Claude Desktop, Cursor, Claude Code oder VS Code mit Ihren Marketo-Anmeldeinformationen.
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
    internal-label: Integrations
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
    internal-label: Configuration
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
    internal-label: Templates
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '2620'
ht-degree: 3%
---

# [!DNL Marketo Engage] MCP-Server

Das Model Context Protocol (MCP) ist ein offener Standard, der KI-Tools mit externen Services verbindet. Der [!DNL Marketo] MCP-Server verbindet Ihren KI-Assistenten mit [!DNL Marketo]. Es unterstützt Vorgänge in den Bereichen Asset-Management, Lead-Daten und Kampagnenautomatisierung. Siehe [Unterstützte MCP](mcp-server-operations.md)Vorgänge für Ausnahmen und Verfügbarkeitsanforderungen.

Wenn Ihr KI-Tool den MCP-Server aufruft, verwendet der Server die Anmeldeinformationen in dieser Anfrage, um den entsprechenden REST-API-Aufruf auszuführen. Sie müssen keine Server-seitige Software installieren, bereitstellen oder ausführen.

Weitere Informationen zum Umgang mit Daten mit Marketo AI und dem Marketo Engage MCP-Server finden Sie auf der Seite [Dateninformationen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>Das Model Context Protocol (MCP) ist ein aufstrebender Open-Source-Standard, der Sicherheits- oder Zuverlässigkeitsrisiken mit sich bringen kann. Adobe MCP-Server-Integrationen und die zugehörige Dokumentation werden ohne Mängelgewähr und ohne Gewährleistung jeglicher Art bereitgestellt.
>Die Verbindung von MCP-Clients oder -Servern mit Adobe-Produkten ist eine vom Kunden gewählte Konfiguration, und die Kunden sind dafür verantwortlich, die Sicherheit und Eignung jeder MCP-Integration zu bewerten. Adobe übernimmt keine Verantwortung für Probleme, die sich aus einer Fehlkonfiguration, einer fehlerhaften Verwendung des MCP, Sicherheitslücken in Drittanbieterimplementierungen oder unbeabsichtigten Aktionen ergeben, die über MCP-fähige Workflows ausgeführt werden.
>Um Risiken zu reduzieren, empfiehlt Adobe, Integrationen vor der produktiven Verwendung in einer Sandbox-Umgebung zu testen und alle MCP-initiierten Aktionen und Antworten sorgfältig zu überprüfen und zu validieren, bevor sie bestätigt oder sich auf sie verlassen.

## MCP-Grundlagen

>Stellen Sie sich MCP wie einen USB-C-Port für KI-Anwendungen vor. USB-C bietet eine standardisierte Möglichkeit, Ihre Geräte mit verschiedenen Peripheriegeräten und Zubehör zu verbinden, und MCP bietet eine standardisierte Möglichkeit, KI-Modelle mit Datenquellen und Tools zu verbinden. — [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP ermöglicht einem KI-Tool, gleichzeitig eine Verbindung zu mehreren externen Services herzustellen. Ein KI-Assistent kann beispielsweise:

* Herstellen einer Verbindung zu einem Textverarbeitungsprogramm für die KI-gestützte Dokumenterstellung
* Verbinden mit Animations-Tools wie Blender für Build-Visualisierungen
* Herstellen einer Verbindung zu Adobe After Effects für die Videobearbeitung

Jede Anwendung kann MCP implementieren, um Daten und Aktionen für KI-Tools verfügbar zu machen.

## Was [!DNL Marketo Engage] MCP tut und was nicht

Wenn Sie den Umfang von MCP verstehen, können Sie Erwartungen definieren, bevor Sie Ihr KI-Tool verbinden.

**MCP macht:**

* Ermöglichen des Zugriffs auf [!DNL Marketo] Daten und Funktionen über standardmäßige REST-APIs
* Ausführen von API-Aufrufen in Ihrem Namen mit den Anmeldedaten, die Sie bei jeder Anfrage angeben
* Unterstützung mehrerer gleichzeitiger Benutzer, von denen jeder mit seinen eigenen Anmeldeinformationen verbunden ist
* Aktualisierung des OAuth-Tokens automatisch verarbeiten. Sie müssen den Ablauf von Token nicht verwalten
* Arbeiten Sie in Umgebungen, die durch Mandanten isoliert sind, sodass sich Ihre Daten nie mit der Sitzung eines anderen Benutzers überschneiden

**MCP nicht:**

* Verwenden, Hosten oder Ausführen von KI- oder maschinellen Lernmodellen. Die gesamte KI-Verarbeitung erfolgt in Ihrem KI-Tool, nicht im MCP
* Trainieren Sie mit beliebigen Daten, einschließlich Ihrer Kundendaten, oder lernen Sie daraus
* Erzeugt Vorhersagen, Empfehlungen oder Entscheidungen. Die Entscheidungsfindung liegt in der Verantwortung des nachgelagerten KI-Tools oder -Benutzers
* Speichern oder Speichern von Anmeldeinformationen, Anfragedaten oder Sitzungsstatus zwischen Anfragen
* Installation, Bereitstellung und Verwaltung von Server-seitiger Software erforderlich

Je nach API-Nutzung kann MCP Daten übertragen, einschließlich potenziell sensibler Felder, aber B2B-Daten umfassen Kundengeschäftsdaten und keine personenbezogenen Daten.

## Voraussetzungen

* Eine [!DNL Marketo] mit aktiviertem REST-API-Zugriff
* Administratorzugriff zum Erstellen von API-Anmeldeinformationen in [!DNL Marketo] LaunchPoint
* Netzwerkzugriff auf die MCP-Server-URL: `https://marketo-mcp.adobe.io/mcp`

## Marketo-Anmeldedaten abrufen

Sie benötigen die folgenden Werte aus Ihrer [!DNL Marketo]:

* **Client-ID**
* **Client Secret** (Client-Geheimnis)
* **Munchkin-Konto-ID**

Wenn Sie bereits über diese verfügen, fahren Sie mit [KI-Tool konfigurieren](#configure-your-ai-tool) fort.

### Client-ID und Client-Geheimnis

1. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Wählen Sie Ihren API-Service aus. Wenn Sie noch keinen haben, wählen Sie **[!UICONTROL Neu]** > **[!UICONTROL Neuer Service]**, wählen Sie **[!UICONTROL Benutzerdefiniert]** als Service-Typ und weisen Sie einen dedizierten API-Benutzer zu.
1. Wählen Sie **[!UICONTROL Details anzeigen]** und kopieren Sie die Werte **[!UICONTROL Client-ID]** und **[!UICONTROL Client-]**).

### Munchkin-Konto-ID

1. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Kopieren Sie die **[!UICONTROL Munchkin-Konto-ID]**. Das Format ist `XXX-XXX-XXX` und entspricht dem Präfix Ihrer Instanz-URL.

## Konfigurieren Ihres KI-Tools

Die Konfigurationen unterscheiden sich je nach Tool geringfügig. In diesem Abschnitt finden Sie Verbindungsbeispiele für gängige Tools.

>[!TIP]
>
>Um eine Verbindung zu mehreren [!DNL Marketo]-Instanzen herzustellen, fügen Sie separate Einträge in Ihrer MCP-Konfiguration mit eindeutigen Namen hinzu: `marketo-prod` und `marketo-staging`, jeweils mit den entsprechenden Anmeldeinformationen.

>[!BEGINTABS]

>[!TAB Claude Desktop]

### Claude Desktop {#claude-desktop}

Um eine Verbindung zu Claude Desktop herzustellen, laden Sie [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) herunter und entpacken Sie sie. Legen Sie `marketo-mcp-bridge.mjs` an einem bekannten Speicherort ab und verwenden Sie den absoluten Pfad in der folgenden Konfiguration.

Außerdem muss eine unterstützte LTS-Version von Node.js auf Ihrem Computer installiert sein. Die Brücke erfordert keine zusätzlichen npm-Pakete.

1. Öffnen Sie Claude Desktop.
1. Öffnen Sie `claude_desktop_config.json`. Auf macOS befindet sich die Datei im `~/Library/Application Support/Claude/`. Unter Windows ist es in `%APPDATA%\Claude\`. Wählen Sie in Versionen mit dem Menü Entwicklereinstellungen die Option **[!UICONTROL Einstellungen]** > **[!UICONTROL Entwickler]** > **[!UICONTROL Konfiguration bearbeiten]**, um sie zu öffnen.
1. Fügen Sie den folgenden Eintrag unter `mcpServers` hinzu, wobei bestehende Einstellungen beibehalten werden. Ersetzen Sie die Platzhalter für Pfad und Anmeldeinformationen durch Ihre Werte:

```json
{
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/marketo-mcp-bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Starten Sie Claude Desktop neu.

Wenn Claude Desktop Node.js nicht finden kann, ersetzen Sie `node` in `command` durch den absoluten Pfad zur ausführbaren Datei Node.js. Weitere Informationen finden Sie unter [Verbindung zu lokalen MCP-Servern herstellen](https://modelcontextprotocol.io/docs/develop/connect-local-servers){target="_blank"}.

>[!TAB Cursor]

### Cursor {#cursor}

Fügen Sie den `marketo` Eintrag unter `mcpServers` in `~/.cursor/mcp.json` für Ihr Benutzerkonto oder `.cursor/mcp.json` in Ihrem Projektverzeichnis hinzu. Vorhandene Server beibehalten und die Platzhalter für Anmeldeinformationen durch Ihre Werte ersetzen:

```json
{
  "mcpServers": {
    "marketo": {
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Öffnen Sie **[!UICONTROL Anpassen]** im Cursor und überprüfen Sie, ob der Server eingeschaltet ist. Weitere Informationen finden Sie unter [Cursor-MCP-Konfiguration](https://cursor.com/docs/context/mcp){target="_blank"}.

>[!TAB Claude-Code-CLI]

### Claude-Code (CLI) {#claude-code}

Führen Sie den folgenden Befehl an Ihrem Terminal aus und ersetzen Sie dabei Ihre Anmeldeinformationen:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

Der Befehl fügt den Server für das aktuelle Projekt und Ihr Benutzerkonto hinzu. Um sie projektübergreifend verfügbar zu machen, fügen Sie `--scope user` hinzu. Führen Sie in Claude Code `/mcp` aus, um die Verbindung zu überprüfen. Weitere Informationen finden Sie unter [Claude Code MCP-Konfiguration](https://code.claude.com/docs/en/mcp){target="_blank"}.

>[!TAB OpenAI-Codex]

### OpenAI-Code {#codex}

Fügen Sie Folgendes zu `~/.codex/config.toml` hinzu, wobei eine vorhandene Konfiguration beibehalten wird:

```toml
[mcp_servers.marketo]
url = "https://marketo-mcp.adobe.io/mcp"

[mcp_servers.marketo.env_http_headers]
X-Marketo-Client-Id = "MARKETO_CLIENT_ID"
X-Marketo-Client-Secret = "MARKETO_CLIENT_SECRET"
X-Marketo-Munchkin-Id = "MARKETO_MUNCHKIN_ID"
```

Legen Sie `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` und `MARKETO_MUNCHKIN_ID` in der Umgebung des Prozesses fest, der Codex ausführt. Die Werte in `env_http_headers` sind Umgebungsvariablennamen, keine Anmeldeinformationen.

Starten Sie eine neue Codex-Sitzung und führen Sie `/mcp` aus, um die Verbindung zu überprüfen. Weitere Informationen finden Sie unter [Codex-MCP-Konfiguration](https://developers.openai.com/codex/mcp){target="_blank"}.

>[!TAB Gemini CLI]

### Gemini-CLI

Um den [!DNL Marketo Engage] MCP-Server zur Gemini-CLI hinzuzufügen, fügen Sie den folgenden `mcpServers` Eintrag zu `.gemini/settings.json` in Ihrem Projektverzeichnis oder `~/.gemini/settings.json` für Ihr Benutzerkonto hinzu. Vorhandene Einstellungen beibehalten und die Platzhalter für Berechtigungen durch Ihre Werte ersetzen.

```json
{
  "mcpServers": {
    "marketo": {
      "httpUrl": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Alternativ können Sie `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` und `MARKETO_MUNCHKIN_ID` in Ihrer Shell-Umgebung festlegen und den folgenden Befehl ausführen. Die Shell erweitert die Variablen, bevor die CLI die Header-Werte in Ihrer Benutzerkonfiguration speichert:

```bash
gemini mcp add --transport http -s user marketo https://marketo-mcp.adobe.io/mcp \
  -H "X-Marketo-Client-Id: $MARKETO_CLIENT_ID" \
  -H "X-Marketo-Client-Secret: $MARKETO_CLIENT_SECRET" \
  -H "X-Marketo-Munchkin-Id: $MARKETO_MUNCHKIN_ID"
```

Starten Sie die Sitzung neu und führen Sie `/mcp` aus, um die Verbindung zu überprüfen. Weitere Informationen finden Sie unter [Gemini CLI-MCP-Konfiguration](https://geminicli.com/docs/tools/mcp-server/){target="_blank"}.

>[!TAB VS-Code mit GitHub Copilot]

### VS-Code mit GitHub Copilot {#vscode}

Verwenden Sie die Konfiguration des tragbaren Copilot für die neuesten Versionen des VS-Codes. Fügen Sie den folgenden Eintrag unter `mcpServers` in `~/.copilot/mcp-config.json` oder `$COPILOT_HOME/mcp-config.json` hinzu, wenn Sie `COPILOT_HOME` festlegen. Vorhandene Server beibehalten:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "${MARKETO_CLIENT_ID}",
        "X-Marketo-Client-Secret": "${MARKETO_CLIENT_SECRET}",
        "X-Marketo-Munchkin-Id": "${MARKETO_MUNCHKIN_ID}"
      }
    }
  }
}
```

Legen Sie die drei Umgebungsvariablen in der Umgebung fest, die VS-Code ausführt, oder den Remote-Host, der eine Verbindung mit dem Server herstellt. Starten Sie eine neue Copilot-Sitzung und überprüfen Sie alle Eingabeaufforderungen zur Serververtrauensstellung.

Um stattdessen das geführte Setup zu verwenden, führen Sie **[!UICONTROL MCP: Server hinzufügen]** aus, wählen Sie **[!UICONTROL HTTP (HTTP oder Server-Sent Events)]** aus, geben Sie die Server-URL ein und wählen Sie **[!UICONTROL Global]** > **[!UICONTROL Copilot Global]**. Fügen Sie dann die Header zur gespeicherten Konfiguration hinzu.

Ältere Versionen unterstützen auch **[!UICONTROL MCP: Open User Configuration]** oder `.vscode/mcp.json`. Diese Dateien verwenden `servers` anstelle der `mcpServers` und `${env:MARKETO_CLIENT_ID}` Syntax für Umgebungsvariablen. Die beiden Formate dürfen nicht gemischt werden. Weitere Informationen finden Sie unter [VS Code MCP Configuration](https://code.visualstudio.com/docs/agents/reference/mcp-configuration){target="_blank"}.

>[!TAB Copilot Studio]

### CoPilot Studio

Aktivieren Sie die generative Orchestrierung für Ihren Agenten, bevor Sie MCP-Tools hinzufügen. Verwenden Sie einen benutzerdefinierten Connector, um alle drei erforderlichen [!DNL Marketo] zu definieren.

Wählen Sie auf der Seite **[!UICONTROL Tools]** Ihres Agenten **[!UICONTROL Tool hinzufügen]** > **[!UICONTROL Neues Tool]** > **[!UICONTROL Benutzerdefinierter Connector]**. Erstellen Sie in Power Apps einen benutzerdefinierten Connector und verwenden Sie den **[!UICONTROL Swagger-Editor]**, um die folgende Definition einzugeben:

```yaml
swagger: '2.0'
info:
  title: Marketo MCP Server
  description: Connect to the Marketo MCP server.
  version: 1.0.0
host: marketo-mcp.adobe.io
basePath: /
schemes:
  - https
paths:
  /mcp:
    post:
      summary: Marketo MCP Server
      description: Invoke the Marketo MCP server.
      operationId: InvokeServer
      x-ms-agentic-protocol: mcp-streamable-1.0
      parameters:
        - name: X-Marketo-Client-Id
          in: header
          description: Client ID.
          type: string
          required: true
        - name: X-Marketo-Client-Secret
          in: header
          description: Client secret.
          type: string
          required: true
        - name: X-Marketo-Munchkin-Id
          in: header
          description: Munchkin ID.
          type: string
          required: true
      responses:
        '200':
          description: Immediate Response
securityDefinitions: {}
security: []
```

Speichern oder aktualisieren Sie den Connector, kehren Sie zu Copilot Studio zurück und fügen Sie ihn Ihrem Agenten hinzu. Erstellen Sie eine Verbindung und geben Sie die drei Header-Werte an, wenn Sie dazu aufgefordert werden. Die Anmeldeinformationen dürfen nicht in der OpenAPI-Definition und in den Agentenanweisungen enthalten sein. Weitere Informationen finden Sie unter [Verbinden eines Agenten mit einem vorhandenen MCP-Server](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent){target="_blank"}.

>[!TAB Glean]

### sammeln {#glean}

Um Glean mit dem [!DNL Marketo Engage] MCP-Server zu verbinden, befolgen Sie [Gleans Remote MCP Setup Instructions](https://docs.glean.com/administration/tools/connect-remote-mcp-servers-to-glean){target="_blank"}, um `https://marketo-mcp.adobe.io/mcp` hinzuzufügen. Überprüfen Sie zunächst den **[!UICONTROL Connectoren]**-Katalog. Bei einem benutzerdefinierten oder nicht aufgelisteten Server verwenden Sie **[!UICONTROL Admin Console]** > **[!UICONTROL Platform]** > **[!UICONTROL Tools]** > **[!UICONTROL Tools vom MCP-Server importieren]**.

Das [Glean-Supportteam muss die unten ](https://docs.glean.com/administration/tools/mcp-custom-headers){target="_blank"} benutzerdefinierten Kopfzeilen konfigurieren. Fügen Sie den Server-Namen, die URL und die erforderlichen Kopfzeilennamen in Ihre Support-Anfrage ein und koordinieren Sie die sichere Handhabung der Werte der Anmeldeinformationen. Eine Standard-API-Schlüsselverbindung allein liefert nicht alle drei Kopfzeilen.

| Header | Wert |
| ------ | ----- |
| `X-Marketo-Client-Id` | Ihre Client-ID |
| `X-Marketo-Client-Secret` | Ihr Client-Geheimnis |
| `X-Marketo-Munchkin-Id` | Ihre Munchkin-Konto-ID |

Nachdem Glean bestätigt hat, dass die Header aktiv sind, testen Sie die Verbindung mit einer schreibgeschützten Anfrage, bevor Sie die Tools anderen Benutzern zur Verfügung stellen.

>[!TAB Weitere Tools]

### Weitere Tools {#other-tools}

Adobe hostet den [!DNL Marketo] MCP-Server und stellt ihn unter einer öffentlichen URL bereit. Jeder MCP-Client, der Remote-Server über einen HTTP-Transport unterstützt, kann eine Verbindung mit ihm herstellen.
Clients, die direkte HTTP-Verbindungen unterstützen, benötigen keine Bridge- oder lokal installierte Server-Software. Wenn Ihr Tool oben nicht aufgeführt ist, verwenden Sie die Verbindungsdetails unten, um es manuell zu konfigurieren.

**Verbindungsdetails:**

| Einstellung | Wert |
| ------- | ----- |
| Transport | HTTP (Streamable HTTP) |
| Server-URL | `https://marketo-mcp.adobe.io/mcp` |

**Authentifizierungs-Header:**

Senden Sie bei jeder Anfrage alle drei Header. Wo Sie die Server-URL und die Header eingeben, hängt von Ihrem Tool ab. Konsultieren Sie daher die MCP-Dokumentation.

| Header | Wert |
| ------ | ----- |
| `X-Marketo-Client-Id` | Ihre Client-ID |
| `X-Marketo-Client-Secret` | Ihr Client-Geheimnis |
| `X-Marketo-Munchkin-Id` | Ihre Munchkin-Konto-ID |

Wenn Ihr Tool eine JSON-Konfiguration akzeptiert, beginnen Sie mit den [Cursor](#cursor)- oder [VS-Code](#vscode)-Beispielen und passen Sie die Schlüssel und die Umgebungsvariablensyntax an das Schema Ihres Tools an.

>[!ENDTABS]

Bitten Sie nach dem Verbinden Ihren KI-Assistenten, den [!DNL Marketo] MCP-Server zu verwenden, um verfügbare Kanäle aufzulisten, ohne Daten zu ändern. Überprüfen Sie den Tool-Aufruf und die Antwort, um zu bestätigen, dass die Anfrage den vorgesehenen Server verwendet und Daten ohne Verbindungs- oder Authentifizierungsfehler zurückgibt.

## Verfügbare Vorgänge

Sobald die Verbindung hergestellt ist, können Sie Ihren KI-Assistenten bitten, Vorgänge in den folgenden Kategorien durchzuführen. Ausnahmen und Verfügbarkeitsanforderungen finden Sie unter [Unterstützte MCP-Vorgänge](mcp-server-operations.md).

### Formulare

Formulare durchsuchen, erstellen, klonen und genehmigen. Hinzufügen oder Entfernen von Feldern, Konfigurieren von Regeln für die Feldsichtbarkeit und Ermitteln, wo Formulare eingebettet sind.

Beispiel-Eingabeaufforderungen:

* „Alle genehmigten Formulare anzeigen“
* „Klonen Sie das Formular „Kontakt“ in den Ordner „Q2 Campaign“.
* „Hinzufügen eines Firmenfelds zum Demo-Anfrageformular“

### Intelligente Kampagnen

Erstellen Sie intelligente Kampagnen, konfigurieren Sie Filter für intelligente Listen, fügen Sie Flussschritte hinzu und aktivieren oder deaktivieren Sie Kampagnen.

Beispiel-Eingabeaufforderungen:

* „Welche intelligenten Kampagnen sind derzeit aktiv?“
* „Erstellen Sie im Ordner „Vorgänge“ eine neue intelligente Kampagne mit dem Namen Aktualisierung der Lead-Bewertung.“
* „Flussschritte in der Willkommens-E-Mail-Kampagne anzeigen“

### Leads und Listen

Suchen von Leads nach E-Mail-Adresse, Erstellen von Lead-Datensätzen und Verwalten der statischen Listenmitgliedschaft.

Beispiel-Eingabeaufforderungen:

* „Lead mit E-Mail jane@example.com suchen“
* „Lead-ID 12345 zur MQL-Liste im 2. Quartal hinzufügen“
* „Erstellen Sie eine neue statische Liste mit dem Namen Summer Event Attendees“

### Programme

Erstellen, Klonen und Taggen von Programmen. Programme nach Typ, Kanal oder Datumsbereich durchsuchen.

Beispiel-Eingabeaufforderungen:

* „Klonen Sie das Webinar-Programm für das 4. Quartal in den Ereignisordner für 2026“
* „Erstellen Sie im Ordner „Kampagnen“ ein neues E-Mail-Programm mit dem Namen „Sommerverkauf“.“
* „Alle Programme anzeigen, die als Webinar getaggt sind“

### E-Mails und Snippets

E-Mails durchsuchen, E-Mails aus Vorlagen erstellen, Inhaltsabschnitte aktualisieren und wiederverwendbare Snippets verwalten.

Beispiel-Eingabeaufforderungen:

* „Alle E-Mail-Entwürfe anzeigen“
* „Aktualisieren des Kopfzeilenabschnitts der Begrüßungs-E-Mail“
* „Welche Assets verwenden das Snippet „Holiday Promo“?“

### Instanzstruktur

Um Ihre [!DNL Marketo] zu verstehen, durchsuchen Sie Ordner, Kanäle, Tag-Typen und Aktivitätstypen.

Beispiel-Eingabeaufforderungen:

* „Alle Ordner in Marketo auflisten“
* „Alle verfügbaren Kanäle anzeigen“
* „Welche Tag-Typen sind konfiguriert?“

### Massenvorgänge

Exportieren Sie Lead-Daten in Batches und überprüfen Sie den Import- oder Exportvorgangsstatus.

Beispiel-Eingabeaufforderungen:

* „Erstellen eines Massenexports von Leads, die in den letzten 30 Tagen erstellt wurden“
* „Status des Exportvorgangs xx überprüfen“

## Fehlerbehebung

| Fehler | Ursache | Korrigieren |
| ------- | ------- | ----- |
| &quot;Marketo-Anmeldeinformationen nicht bereitgestellt“ | Mindestens ein `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` oder `X-Marketo-Munchkin-Id` fehlt. | Überprüfen Sie, ob alle Kopfzeilen der Client-Anmeldeinformationen für Marketo in Ihrer Konfiguration vorhanden sind. |
| „401 Nicht autorisiert“ | Ihre Anmeldedaten fehlen, sind ungültig oder abgelaufen. Bei Marketo-Client-Anmeldeinformationen ist die Client-ID oder das Client-Geheimnis falsch. Bei einem IMS-Token ist das Token ungültig oder abgelaufen. | Überprüfen Sie die Anmeldeinformationen für Ihre Authentifizierungsmethode. Überprüfen Sie die Client-Anmeldeinformationen erneut **[!UICONTROL Client-ID]** und **[!UICONTROL Client-]**) unter **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. Generieren Sie für ein IMS-Token ein neues Token und aktualisieren Sie die `Authorization`-Kopfzeile. |
| „403 Verboten“ | Ihre Anmeldedaten sind gültig, aber Ihre [!DNL Marketo]-Instanz ist nicht für den MCP-Zugriff aktiviert. | Wenden Sie sich an Ihren [!DNL Marketo] MCP-Administrator, um den MCP-Zugriff für Ihre Munchkin-Konto-ID zu aktivieren. |
| „Zu viele Anfragen“ (Ratenlimit) | Sie haben zu viele Anfragen innerhalb eines kurzen Zeitraums oder zu viele Anfragen gleichzeitig gesendet und die API-Beschränkungen Ihrer [!DNL Marketo] erreicht. | Reduzieren Sie die Häufigkeit und Anzahl der gleichzeitig gesendeten Anfragen und warten Sie eine kurze Zeit, bevor Sie es erneut versuchen. Verwenden Sie einen dedizierten API-Benutzer, um Ihr Kontingent zu verfolgen und zu verwalten. |
| Verbindungs-Timeout oder Zurückgewiesen | Der MCP-Server ist nicht über das Netzwerk erreichbar. | Bestätigen Sie, dass Sie die Server-URL aus Ihrer Umgebung erreichen können. Überprüfen Sie ggf. die VPN-Anforderungen. |
| Tool-Aufrufe geben leere Ergebnisse zurück | Der API-Benutzer verfügt nicht über die erforderlichen Berechtigungen für den angeforderten Asset-Typ. | Bitten Sie Ihren [!DNL Marketo], die Rolle und die Berechtigungen des API-Benutzers zu überprüfen. |

## Sicherheitsüberlegungen

>[!IMPORTANT]
>
>Verwenden Sie einen dedizierten API-Benutzer in [!DNL Marketo], der nur über die für Ihre Arbeit erforderlichen Berechtigungen verfügt. Verwenden Sie keine Administratorberechtigungen für den API-Zugriff erneut.

* **Anmeldeinformationen pro Anfrage.** Bei jeder Anfrage werden die Client-ID, das Client-Geheimnis, die Munchkin-ID und der REST-API-Endpunkt in HTTP-Headern übertragen. Der Server speichert oder speichert sie nicht im Cache.
* **Isolierung mehrerer Mandanten.** Jede Anfrage verwendet einen eigenen Satz von Anmeldeinformationen. Ihre Daten überschneiden sich nicht mit der Sitzung eines anderen Benutzers.
* **Munchkin ID-Zulassungsliste.** Der Server akzeptiert nur Anfragen für genehmigte [!DNL Marketo]. Anfragen, die eine nicht autorisierte Munchkin-ID verwenden, werden mit einem 403-Fehler zurückgewiesen.
* **API-Ratenbeschränkungen.** Der MCP-Server übernimmt die API-Ratenbeschränkungen Ihrer [!DNL Marketo]. Verwenden Sie einen dedizierten API-Benutzer, um die Kontingentnutzung zu verfolgen und zu verwalten.
* **Halten Sie Anmeldeinformationen außerhalb der Versionskontrolle.** Verwenden Sie die Umgebungsvariableninterpolation (`${MARKETO_CLIENT_SECRET}`), wenn Ihr KI-Tool sie unterstützt, sodass Anmeldeinformationen nicht im Klartext in Repository-Dateien gespeichert werden.

## Governance und Datenaufbewahrung

### Umgang mit Anmeldeinformationen

* Die Kundenanmeldeinformationen werden nicht Server-seitig gespeichert, sondern vom Client pro Anfrage bereitgestellt, wodurch die Offenlegung von Anmeldeinformationen innerhalb des Services eingeschränkt wird.

### API-Interaktionsmodell

* Agent-Nutzung: Agenten können den MCP-Server verwenden, um unterstützte Marketo-APIs aufzurufen.
* Ausrichtung des Authentifizierungsmodells: Der Service verwendet dasselbe externe API-Authentifizierungsmodell, das auch für Marketo-APIs dokumentiert ist.

### Authentifizierung und Autorisierung

* Geringste Rechte: Effektive Berechtigungen werden von dem Marketo-API-basierten Benutzer übernommen, der dem LaunchPoint-Service des Kunden zugewiesen ist, wodurch die Verwaltung der geringsten Rechte innerhalb der Marketo-Konfiguration des Kunden ermöglicht wird.
* Keine Server-seitige Token-Persistenz: Der Service vermeidet weiterhin die Server-seitige Speicherung von Kunden-Anmeldeinformationen oder Token.  

### Protokollierung und Überwachung

* Sicherheitsprotokollierung: Strukturierte JSON-Protokolle werden über die Flüssig-Bit-API an Splunk weitergeleitet, wobei Masken für vertrauliche Daten sowie zusätzliche Filter zur Unterstützung der Compliance-Anforderungen bereitgestellt werden.
* Audit-Unterstützung: Diese Kontrollen unterstützen die laufende Überwachung der Service-Verfügbarkeit, sicherheitsrelevanter Ereignisse und der Betriebsqualität.
* Kein Server-seitiger geheimer Speicher: Kundenanmeldeinformationen werden von der MCP-Bereitstellung nicht gespeichert und müssen von Clients pro Anfrage bereitgestellt werden.
* Token-Verarbeitung: Zugriffstoken sind kurzlebig, Token-Antworten sind als nicht gespeichert gekennzeichnet, und Token werden über standardmäßige Autorisierungsmechanismen und nicht über die Übertragung von Abfragezeichenfolgen akzeptiert.
* Rollenbasierter betrieblicher Zugriff: Der Zugriff auf die administrative Bereitstellung wird über Adobe-Infrastrukturrollen und gruppenbasierte Steuerelemente gesteuert, während die Berechtigungen für die Datenebene von der Marketo-API-Benutzerkonfiguration des Kunden übernommen werden.
* Audit und Beobachtbarkeit: Die Sicherheitsprotokollierung, -maskierung, -überwachung und -warnungen werden aktiviert, um Ermittlungen, die Verfolgung des Service-Status und Betriebsaufsichten zu unterstützen.
