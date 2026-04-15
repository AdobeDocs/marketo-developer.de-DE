---
title: MCP-Server
description: Erfahren Sie, wie Sie einen KI-Assistenten mithilfe des MCP-Servers mit Marketo verbinden. Konfigurieren Sie Claude Desktop, Cursor, Claude Code oder VS Code mit Ihren Marketo-Anmeldeinformationen.
hidefromtoc: true
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
source-git-commit: 3fe1c3e9fe572ef68d20ba10f93535aac9a98602
workflow-type: tm+mt
source-wordcount: '1324'
ht-degree: 1%

---

# [!DNL Marketo] MCP-Server

Das Model Context Protocol (MCP) ist ein offener Standard, der es KI-Tools ermöglicht, mit externen Services zu kommunizieren. Der [!DNL Marketo] MCP-Server fungiert als Brücke zwischen Ihrem KI-Assistenten und [!DNL Marketo]. Es stellt mehr als 100 Vorgänge in Formularen, Programmen, intelligenten Kampagnen, Leads, E-Mails, Snippets, Listen und Ordnern bereit.

Wenn Ihr KI-Tool den MCP-Server aufruft, führt der Server den entsprechenden REST-API-Aufruf in Ihrem Namen aus. Dabei werden die Anmeldeinformationen verwendet, die Sie in jeder Anfrage angeben. Sie müssen keine Server-seitige Software installieren, bereitstellen oder ausführen.

## Voraussetzungen

- Eine [!DNL Marketo] mit aktiviertem REST-API-Zugriff
- Administratorzugriff zum Erstellen von API-Anmeldeinformationen in [!DNL Marketo] LaunchPoint
- Eines der folgenden KI-Tools: Claude Desktop, Cursor, Claude Code (CLI) oder VS Code mit GitHub Copilot
- Netzwerkzugriff auf die MCP-Server-URL: `https://marketo-mcp.adobe.io/mcp`

## Marketo-Anmeldedaten abrufen

Sie benötigen die folgenden Werte aus Ihrer [!DNL Marketo]:

- **Client-ID**
- **Client Secret** (Client-Geheimnis)
- **Munchkin-Konto-ID**
- **REST-API-Endpunkt**

Wenn Sie bereits über diese verfügen, fahren Sie mit [KI-Tool konfigurieren](#configure-your-ai-tool) fort.

### Client-ID und Client-Geheimnis

1. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Klicken Sie auf Ihren API-Service. Wenn Sie noch keinen haben, wählen Sie **[!UICONTROL Neu]** > **[!UICONTROL Neuer Service]**, wählen Sie **[!UICONTROL Benutzerdefiniert]** als Service-Typ und weisen Sie einen dedizierten API-Benutzer zu.
1. Klicken Sie auf **[!UICONTROL Details anzeigen]** und kopieren Sie die Werte **[!UICONTROL Client-ID]** und **[!UICONTROL Client-]**).

### Munchkin-Konto-ID

1. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Kopieren Sie die **[!UICONTROL Munchkin-Konto-ID]**. Das Format ist `XXX-XXX-XXX` und entspricht dem Präfix Ihrer Instanz-URL.

### REST API-Endpunkt

1. Navigieren Sie **[!UICONTROL Admin]** > **[!UICONTROL Web-Services]**.
1. Kopieren **[!UICONTROL unter „REST]** API“ die URL **[!UICONTROL Endpunkt]**. Das Format ist `https://XXX-XXX-XXX.mktorest.com`.

## Konfigurieren Ihres KI-Tools

Jedes KI-Tool liest die MCP-Server-Konfiguration von einem anderen Speicherort aus. Suchen Sie unten Ihr Tool und führen Sie die Schritte zum Hinzufügen des [!DNL Marketo] MCP-Servers aus.

### Claude Desktop

Die Konfigurationsdatei wird `claude_desktop_config.json`. Öffnen Sie sie von einem der folgenden Standorte aus:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

Wenn die Datei bereits andere MCP-Server enthält, fügen Sie den `marketo` Eintrag unter `mcpServers` hinzu. Im folgenden Beispiel wird der vollständige `mcpServers` dargestellt:

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

Speichern Sie die Datei, beenden Sie Claude Desktop und öffnen Sie sie erneut.

### Cursor

Wenn Ihre Cursor-MCP-Konfiguration bereits andere Server enthält, fügen Sie den `marketo` Eintrag unter `mcpServers` hinzu. Das folgende Beispiel zeigt den vollständigen `mcpServers`-Block unter **[!UICONTROL Einstellungen]** > **[!UICONTROL MCP]** oder `.cursor/mcp.json` in Ihrem Projektverzeichnis:

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

Cursor neu starten.

### Claude-Code (CLI)

Führen Sie den folgenden Befehl an Ihrem Terminal aus und ersetzen Sie dabei Ihre Anmeldeinformationen:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

### VS-Code mit GitHub Copilot

Öffnen Sie Ihre VS-Code-`settings.json`, indem Sie **[!UICONTROL Strg+Umschalt+P]** oder **[!UICONTROL Befehl+Umschalt+P]** auf macOS drücken und dann **[!UICONTROL Voreinstellungen: Benutzereinstellungen öffnen (JSON) auswählen]**. Fügen Sie das folgende Beispiel hinzu:

```json
{
  "mcp": {
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
}
```

Drücken Sie **[!UICONTROL Strg+Umschalt+P]** (oder **[!UICONTROL Befehl+Umschalt+P]** auf macOS), geben Sie **[!UICONTROL Fenster neu laden]** ein und drücken Sie die Eingabetaste.

>[!NOTE]
>
>Verwenden Sie aus Sicherheitsgründen die Interpolation von Umgebungsvariablen in Konfigurationsdateien, anstatt Anmeldeinformationen direkt einzufügen. Sie können Variablen mithilfe von Syntax wie `${MARKETO_CLIENT_SECRET}` referenzieren und sie in Ihrer Umgebung festlegen. Dadurch wird verhindert, dass Anmeldeinformationen im Klartext in Dateien gespeichert werden, die in die Versionskontrolle übertragen werden können.

## Verfügbare Vorgänge

Sobald die Verbindung hergestellt ist, können Sie Ihren KI-Assistenten bitten, Vorgänge in den folgenden Kategorien durchzuführen.

### Formulare

Formulare durchsuchen, erstellen, klonen und genehmigen. Hinzufügen oder Entfernen von Feldern, Konfigurieren von Regeln für die Feldsichtbarkeit und Ermitteln, wo Formulare eingebettet sind.

Beispiel-Eingabeaufforderungen:

- „Alle genehmigten Formulare anzeigen“
- „Klonen Sie das Formular „Kontakt“ in den Ordner „Q2 Campaign“.
- „Hinzufügen eines Firmenfelds zum Demo-Anfrageformular“

### Intelligente Kampagnen

Erstellen Sie intelligente Kampagnen, konfigurieren Sie Filter für intelligente Listen, fügen Sie Flussschritte hinzu und aktivieren oder deaktivieren Sie Kampagnen.

Beispiel-Eingabeaufforderungen:

- „Welche intelligenten Kampagnen sind derzeit aktiv?“
- „Erstellen Sie im Ordner „Vorgänge“ eine neue intelligente Kampagne mit dem Namen Aktualisierung der Lead-Bewertung.“
- „Flussschritte in der Willkommens-E-Mail-Kampagne anzeigen“

### Leads und Listen

Suchen nach Leads nach E-Mail-Adresse, Erstellen oder Aktualisieren von Lead-Datensätzen und Verwalten der statischen Listenmitgliedschaft.

Beispiel-Eingabeaufforderungen:

- „Lead mit E-Mail jane@example.com suchen“
- „Lead-ID 12345 zur MQL-Liste im 2. Quartal hinzufügen“
- „Erstellen Sie eine neue statische Liste mit dem Namen Summer Event Attendees“

### Programme

Erstellen, Klonen und Taggen von Programmen. Programme nach Typ, Kanal oder Datumsbereich durchsuchen.

Beispiel-Eingabeaufforderungen:

- „Klonen Sie das Webinar-Programm für das 4. Quartal in den Ereignisordner für 2026“
- „Erstellen Sie im Ordner „Kampagnen“ ein neues E-Mail-Programm mit dem Namen „Sommerverkauf“.“
- „Alle Programme anzeigen, die als Webinar getaggt sind“

### E-Mails und Snippets

E-Mails durchsuchen, E-Mails aus Vorlagen erstellen, Inhaltsabschnitte aktualisieren und wiederverwendbare Snippets verwalten.

Beispiel-Eingabeaufforderungen:

- „Alle E-Mail-Entwürfe anzeigen“
- „Aktualisieren des Kopfzeilenabschnitts der Begrüßungs-E-Mail“
- „Welche Assets verwenden das Snippet „Holiday Promo“?“

### Instanzstruktur

Durchsuchen Sie Ordner, Kanäle, Tag-Typen und Aktivitätstypen, um Ihre [!DNL Marketo] zu verstehen.

Beispiel-Eingabeaufforderungen:

- „Alle Ordner in Marketo auflisten“
- „Alle verfügbaren Kanäle anzeigen“
- „Welche Tag-Typen sind konfiguriert?“

### Massenvorgänge

Exportieren Sie Lead-Daten in Batches und überprüfen Sie den Import- oder Exportvorgangsstatus.

Beispiel-Eingabeaufforderungen:

- „Erstellen eines Massenexports von Leads, die in den letzten 30 Tagen erstellt wurden“
- „Status des Exportvorgangs xx überprüfen“

## Fehlerbehebung

| Fehler | Ursache | Korrigieren |
| ------- | ------- | ----- |
| &quot;Marketo-Endpunkt nicht bereitgestellt“ | Die Kopfzeile &quot;`X-Marketo-Endpoint`&quot; fehlt in der Konfiguration. | Überprüfen Sie Ihre MCP-Konfiguration erneut und bestätigen Sie, dass alle vier Header vorhanden sind. |
| &quot;Marketo-Anmeldeinformationen nicht bereitgestellt“ | Mindestens ein `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` oder `X-Marketo-Munchkin-Id` fehlt. | Überprüfen Sie, ob alle vier Kopfzeilen in Ihrer Konfiguration vorhanden sind. |
| „Authentifizierungsfehler“ | Ihre Anmeldedaten sind ungültig oder abgelaufen. | Überprüfen Sie Ihre Client-ID und Ihren geheimen Client-Schlüssel unter **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| „403 Verboten“ | Ihre Munchkin ID befindet sich nicht auf der Server-Zulassungsliste. | Wenden Sie sich an Ihren [!DNL Marketo] MCP-Administrator, um Ihre Munchkin ID hinzuzufügen. |
| Verbindungs-Timeout oder Zurückgewiesen | Der MCP-Server ist nicht über das Netzwerk erreichbar. | Bestätigen Sie, dass Sie die Server-URL aus Ihrer Umgebung erreichen können. Überprüfen Sie ggf. die VPN-Anforderungen. |
| Tool-Aufrufe geben leere Ergebnisse zurück | Der API-Benutzer verfügt nicht über die erforderlichen Berechtigungen für den angeforderten Asset-Typ. | Bitten Sie Ihren [!DNL Marketo], die Rolle und die Berechtigungen des API-Benutzers zu überprüfen. |

## Häufig gestellte Fragen

### Sind meine Daten sicher?

Anmeldeinformationen werden bei jeder einzelnen Anfrage in HTTP-Headern übertragen. Der Server speichert oder speichert Anmeldeinformationen nicht zwischen Sitzungen und jede Anfrage ist vollständig isoliert.

### Können mehrere Personen das gleichzeitig benutzen?

Ja. Der Server ist mehrmandantenfähig. Jeder Benutzer stellt eine Verbindung mit seinen eigenen Anmeldeinformationen her, und Anfragen werden voneinander isoliert.

### Was passiert, wenn mein Zugriffs-Token abläuft?

Bei der Authentifizierung mit Client-ID und Client-Geheimnis verarbeitet der Server die Token-Aktualisierung automatisch. Sie müssen keinerlei Maßnahmen ergreifen.

### Muss ich etwas installieren oder ausführen?

Nein. Der MCP-Server wird von Adobe gehostet. Sie müssen lediglich Ihr KI-Tool konfigurieren, um eine Verbindung herzustellen.

### Welche [!DNL Marketo] Berechtigungen benötigt mein API-Benutzer?

Der API-Benutzer benötigt Zugriff auf die Asset-Typen, die Sie verwalten möchten. Weisen Sie mindestens eine schreibgeschützte Rolle für Browservorgänge und eine Lese-/Schreibrolle für das Erstellen oder Ändern von Assets zu. Wenden Sie sich an Ihren [!DNL Marketo], um die entsprechenden Berechtigungen zuzuweisen.

### Wie hoch sind die Limits?

Der MCP-Server übernimmt die API-Ratenbeschränkungen der Marketo-Instanz. Verwenden Sie einen dedizierten API-Benutzer, um die Kontingentnutzung zu verfolgen und zu verwalten.

### Welche KI-Tools werden unterstützt?

Claude Desktop, Cursor, Claude Code (CLI) und VS-Code mit GitHub Copilot. Jedes KI-Tool, das das Model Context Protocol über HTTP unterstützt, sollte funktionieren.

### Kann ich eine Verbindung zu mehreren [!DNL Marketo]-Instanzen herstellen?

Ja. Fügen Sie der MCP-Konfiguration Ihres KI-Tools mehrere Einträge mit jeweils einem eindeutigen Namen und den Anmeldeinformationen für die entsprechende Instanz hinzu. Sie können beispielsweise `marketo-prod` und `marketo-staging` als separate Server konfigurieren.

## Sicherheitsüberlegungen

>[!IMPORTANT]
>
>Verwenden Sie einen dedizierten API-Benutzer in [!DNL Marketo], der nur über die für Ihre Arbeit erforderlichen Berechtigungen verfügt. Verwenden Sie keine Administratorberechtigungen für den API-Zugriff erneut.

- **Anmeldeinformationen pro Anfrage.** Bei jeder Anfrage werden die Client-ID, das Client-Geheimnis, die Munchkin-ID und der REST-API-Endpunkt in HTTP-Headern übertragen. Der Server speichert oder speichert sie nicht im Cache.
- **Isolierung mehrerer Mandanten.** Jede Anfrage verwendet einen eigenen Satz von Anmeldeinformationen. Ihre Daten überschneiden sich nicht mit der Sitzung eines anderen Benutzers.
- **Munchkin ID-Zulassungsliste.** Der Server akzeptiert nur Anfragen für genehmigte [!DNL Marketo]. Anfragen, die eine nicht autorisierte Munchkin-ID verwenden, werden mit einem 403-Fehler zurückgewiesen.
- **Halten Sie Anmeldeinformationen außerhalb der Versionskontrolle.** Verwenden Sie die Umgebungsvariableninterpolation (`${MARKETO_CLIENT_SECRET}`), wenn Ihr KI-Tool sie unterstützt, sodass Anmeldeinformationen nicht im Klartext in Dateien gespeichert werden, die in ein Repository übertragen werden.
