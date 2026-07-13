---
title: MCP-Server für Marketo Engage
description: Erfahren Sie, wie Sie einen KI-Assistenten mithilfe des Marketo Engage MCP-Servers mit Marketo verbinden. Konfigurieren Sie Claude Desktop, Cursor, Claude Code oder VS Code mit Ihren Marketo-Anmeldeinformationen.
badgeBeta: label="Eingeschränkte Verfügbarkeit" type="informative" tooltip="Diese Funktion befindet sich derzeit in einer eingeschränkten Beta-Version"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: c2dbad80-0f5c-4d96-a798-2a65f93b8721id: dca84292-69e9-4116-a575-667d31fa060did: e2290edd-b061-4880-9d79-dee306cf5aa9id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: b28708e92f44082eb247d9053d6ebf2306739b38
workflow-type: tm+mt
source-wordcount: 2199
ht-degree: 1%

---

# [!DNL Marketo Engage] MCP-Server

>[!AVAILABILITY]
>
> Diese Funktion ist nur eingeschränkt verfügbar. Um den Zugriff anzufordern, füllen Sie [dieses Formular](https://forms.cloud.microsoft/Pages/ResponsePage.aspx?id=Wht7-jR7h0OUrtLBeN7O4Y-uSf63sAxCmWyqMJg8eMFUMVZSVExSNDA3T0I4SEcwRDFSVTBGWU01Uy4u&origin=QRCode){target="_blank"} aus. Stellen Sie sicher, dass Sie die Munchkin ID Ihres Abonnements bereit haben.

Das Model Context Protocol (MCP) ist ein offener Standard, der es KI-Tools ermöglicht, mit externen Services zu kommunizieren. Der [!DNL Marketo] MCP-Server fungiert als Brücke zwischen Ihrem KI-Assistenten und [!DNL Marketo]. Es stellt mehr als 100 Vorgänge in Formularen, Programmen, intelligenten Kampagnen, Leads, E-Mails, Snippets, Listen und Ordnern bereit.

Wenn Ihr KI-Tool den MCP-Server aufruft, führt der Server den entsprechenden REST-API-Aufruf in Ihrem Namen aus. Dabei werden die Anmeldeinformationen verwendet, die Sie in jeder Anfrage angeben. Sie müssen keine Server-seitige Software installieren, bereitstellen oder ausführen.

Weitere Informationen zum Umgang mit Daten mit Marketo AI und dem Marketo Engage MCP-Server finden Sie auf der Seite [Dateninformationen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>Das Model Context Protocol (MCP) ist ein aufstrebender Open-Source-Standard, der Sicherheits- oder Zuverlässigkeitsrisiken mit sich bringen kann. Adobe MCP-Server-Integrationen und die zugehörige Dokumentation werden ohne Mängelgewähr und ohne Gewährleistung jeglicher Art bereitgestellt.Die Verbindung von MCP-Clients oder -Servern mit Adobe-Produkten ist eine vom Kunden gewählte Konfiguration, und die Kunden sind dafür verantwortlich, die Sicherheit und Eignung jeder MCP-Integration zu bewerten. Adobe übernimmt keine Verantwortung für Probleme, die sich aus einer Fehlkonfiguration, einer fehlerhaften Verwendung des MCP, Sicherheitslücken in Drittanbieterimplementierungen oder unbeabsichtigten Aktionen ergeben, die über MCP-fähige Workflows ausgeführt werden.Um Risiken zu reduzieren, empfiehlt Adobe, Integrationen vor der produktiven Verwendung in einer Sandbox-Umgebung zu testen und alle MCP-initiierten Aktionen und Antworten sorgfältig zu überprüfen und zu validieren, bevor sie bestätigt oder sich auf sie verlassen.

## MCP-Grundlagen

>Stellen Sie sich MCP wie einen USB-C-Port für KI-Anwendungen vor. USB-C bietet eine standardisierte Möglichkeit, Ihre Geräte mit verschiedenen Peripheriegeräten und Zubehör zu verbinden, und MCP bietet eine standardisierte Möglichkeit, KI-Modelle mit Datenquellen und Tools zu verbinden. — [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP ermöglicht einem KI-Tool, gleichzeitig eine Verbindung zu mehreren externen Services herzustellen. Ein KI-Assistent könnte beispielsweise:

* Herstellen einer Verbindung zu einem Textverarbeitungsprogramm für die KI-gestützte Dokumenterstellung
* Verbinden mit Animations-Tools wie Blender für Build-Visualisierungen
* Herstellen einer Verbindung zu Adobe After Effects für die Videobearbeitung

MCP ist ein Kommunikationsprotokoll: ein offener Standard, den jede Anwendung implementieren kann, um ihre Daten und Aktionen KI-Tools zur Verfügung zu stellen.

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
* Eines der folgenden KI-Tools: Claude Desktop, Cursor, Codex, Claude Code (CLI) oder VS Code mit GitHub Copilot
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

Jedes KI-Tool hat ein etwas anderes Setup. Verbindungsbeispiele werden für gängige Tools bereitgestellt.

* [Claude Desktop](#claude-desktop)
* [Cursor](#cursor)
* [Claude Code CLI](#claude-code)
* [OpenAI-Code](#codex)
* [VSCode mit GitHub Copilot](#vscode)
* [sammeln](#glean)
* [Weitere Tools](#other-tools)

>[!TIP]
>
>Um eine Verbindung zu mehreren [!DNL Marketo]-Instanzen herzustellen, fügen Sie separate Einträge in Ihrer MCP-Konfiguration mit eindeutigen Namen hinzu: `marketo-prod` und `marketo-staging`, jeweils mit den entsprechenden Anmeldeinformationen.

### Claude Desktop {#claude-desktop}

Um eine Verbindung zu Claude Desktop herzustellen, laden Sie [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) herunter und entpacken Sie sie. Legen Sie `marketo-mcp-bridge.mjs` an einem bekannten Speicherort ab, damit Sie im nächsten Schritt darauf verweisen können.

Sie benötigen außerdem:

* Node.js v18+
* npm

1. Claude Desktop öffnen
1. Navigieren Sie zu **Einstellungen > Entwickler > Konfiguration bearbeiten**
1. Fügen Sie Folgendes zu `claude_desktop_config.json` hinzu:

```json
{
  "preferences": {
    ...
  },
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Claude Desktop neu starten

### Cursor {#cursor}

Wenn Ihre Cursor-MCP-Konfiguration bereits andere Server enthält, fügen Sie den `marketo` Eintrag unter `mcpServers` hinzu.Das folgende Beispiel zeigt den vollständigen `mcpServers`-Block unter **[!UICONTROL Einstellungen]** > **[!UICONTROL MCP]** oder `.cursor/mcp.json` in Ihrem Projektverzeichnis:

>[!BEGINTABS]

>[!TAB IMS-Token]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Marketo-Client-Anmeldedaten]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
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

>[!ENDTABS]

Cursor neu starten.

### Claude-Code (CLI) {#claude-code}

Führen Sie den folgenden Befehl an Ihrem Terminal aus und ersetzen Sie dabei Ihre Anmeldeinformationen:

>[!BEGINTABS]

>[!TAB IMS-Token]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "Authorization: Bearer YOUR-IMS-TOKEN" \
  --header "x-gw-ims-org-id: YOUR-IMS-ORG-ID"
```

>[!TAB Marketo-Client-Anmeldedaten]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

>[!ENDTABS]

### OpenAI-Code {#codex}

1. Gehen Sie zu Einstellungen > MCP-Server > Server hinzufügen
1. Fügen Sie die Server-URL hinzu: `https://marketo-mcp.adobe.io/mcp`
1. Fügen Sie die Kopfzeilen für Ihre Authentifizierungsmethode hinzu:

>[!BEGINTABS]

>[!TAB IMS-Token]

* Autorisierung: „Bearer YOUR-IMS-TOKEN“
* x-gw-ims-org-id: „IHRE-IMS-ORG-ID“

>[!TAB Marketo-Client-Anmeldedaten]

* X-Marketo-Client-ID: „IHRE-CLIENT-ID“
* X-Marketo-Client-Secret: „IHR-CLIENT-GEHEIMNIS“
* X-Marketo-Munchkin-ID: „IHRE-MUNCHKIN-ID“

>[!ENDTABS]

1. Klicken Sie auf Speichern , um den Vorgang abzuschließen.


### VS-Code mit GitHub Copilot {#vscode}

Drücken Sie **[!UICONTROL Strg+Umschalt+P]** (oder **[!UICONTROL Befehlstaste+Umschalt+P]** auf macOS), geben Sie **[!UICONTROL MCP: Benutzerkonfiguration öffnen ein]** und drücken Sie die Eingabetaste. Dadurch wird `mcp.json` geöffnet. Fügen Sie den `marketo` Eintrag innerhalb des `servers` hinzu:

>[!BEGINTABS]

>[!TAB IMS-Token]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Marketo-Client-Anmeldedaten]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
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

>[!ENDTABS]

>[!NOTE]
>
>Verwenden Sie aus Sicherheitsgründen die Interpolation von Umgebungsvariablen in Konfigurationsdateien, anstatt Anmeldeinformationen direkt einzufügen. Sie können Variablen mithilfe von Syntax wie `${MARKETO_CLIENT_SECRET}` referenzieren und sie in Ihrer Umgebung festlegen. Dadurch wird verhindert, dass Anmeldeinformationen im Klartext in Dateien gespeichert werden, die der Versionskontrolle unterliegen.

### sammeln {#glean}

Um Glean mit dem Marketo Engage MCP Server zu verbinden, [ das Glean Support Team](https://docs.glean.com/release-notes/releases/2026-04-22-april-release#admin-features) die folgenden benutzerdefinierten Header konfigurieren.

| Header | Wert |
| ------ | ----- |
| `X-Marketo-Client-Id` | Ihre Client-ID |
| `X-Marketo-Client-Secret` | Ihr Client-Geheimnis |
| `X-Marketo-Munchkin-Id` | Ihre Munchkin-Konto-ID |

### Weitere Tools {#other-tools}

Adobe hostet den [!DNL Marketo] MCP-Server und stellt ihn unter einer öffentlichen URL bereit. Jeder MCP-Client, der Remote-Server über einen HTTP-Transport unterstützt, kann eine Verbindung mit ihm herstellen.Sie benötigen keine tool-spezifische Bridge oder lokal installierte Software. Wenn Ihr Tool oben nicht aufgeführt ist, verwenden Sie die Verbindungsdetails unten, um es manuell zu konfigurieren.

**Verbindungsdetails:**

| Einstellung | Wert |
| ------- | ----- |
| Transport | HTTP (Streamable HTTP) |
| Server-URL | `https://marketo-mcp.adobe.io/mcp` |

**Authentifizierungs-Header:**

Senden Sie bei jeder Anfrage die Header für eine der folgenden Authentifizierungsmethoden. Wo Sie die Server-URL und die Header eingeben, hängt von Ihrem Tool ab. Konsultieren Sie daher die MCP-Dokumentation.

>[!BEGINTABS]

>[!TAB IMS-Token]

| Header | Wert |
| ------ | ----- |
| `Authorization` | `Bearer YOUR-IMS-TOKEN` |
| `x-gw-ims-org-id` | Ihre IMS-Organisations-ID |

>[!TAB Marketo-Client-Anmeldedaten]

| Header | Wert |
| ------ | ----- |
| `X-Marketo-Client-Id` | Ihre Client-ID |
| `X-Marketo-Client-Secret` | Ihr Client-Geheimnis |
| `X-Marketo-Munchkin-Id` | Ihre Munchkin-Konto-ID |

>[!ENDTABS]

Wenn Ihr Tool eine JSON-Konfiguration akzeptiert, beginnen Sie mit den [Cursor](#cursor)- oder [VS-Code](#vscode)-Beispielen und passen Sie die Schlüssel (`mcpServers`, `servers`) an das Schema Ihres Tools an.

## Verfügbare Vorgänge

Sobald die Verbindung hergestellt ist, können Sie Ihren KI-Assistenten bitten, Vorgänge in den folgenden Kategorien durchzuführen. Eine vollständige Liste der unterstützten Vorgänge mit API-Referenzen finden Sie unter [Unterstützte MCP-Vorgänge](mcp-server-operations.md).

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

Suchen nach Leads nach E-Mail-Adresse, Erstellen oder Aktualisieren von Lead-Datensätzen und Verwalten der statischen Listenmitgliedschaft.

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
