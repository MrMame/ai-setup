# Einrichtung des Codex-Harness & Systemvergleich (Claude Code vs. OpenAI Codex vs. GitHub Copilot)

Der **Codex-Harness** wird über eine Kombination aus **`AGENTS.md`** (Projekt- und Nutzeranweisungen), **`config.toml`** (Laufzeit- und Sandbox-Regeln), **Skills** (`SKILL.md`) sowie **Hooks** eingerichtet. Da du bereits Erfahrung mit **Claude Code** hast, lässt sich dein bestehendes Setup fast eins zu eins übertragen. Alle drei Plattformen (Claude Code, OpenAI Codex und GitHub Copilot) haben sich inzwischen auf gemeinsame offene Standards wie `AGENTS.md`, `SKILL.md` und MCP geeinigt, unterscheiden sich jedoch deutlich in Konfigurationsformat, Sicherheitsarchitektur und Ausführungsmodell.

---

## 1. Drei-Wege-Vergleich: Claude Code vs. OpenAI Codex vs. GitHub Copilot

Die folgende Tabelle stellt die Harness-Architekturen von **Claude Code**, **OpenAI Codex (CLI/App)** und **GitHub Copilot (CLI/IDE)** direkt gegenüber.

| Harness-Komponente | Claude Code (Anthropic) | OpenAI Codex (CLI & App) | GitHub Copilot (CLI & IDE) | Wichtiger Unterschied |
| :--- | :--- | :--- | :--- | :--- |
| **Projekt-Anweisungen** | `CLAUDE.md` (liest auch `AGENTS.md`) | `AGENTS.md` | `.github/copilot-instructions.md` & `AGENTS.md` | **`AGENTS.md`** dient als gemeinsamer Nenner, den alle drei Tools nativ einlesen. |
| **Globale Defaults** | `~/.claude/CLAUDE.md` | `~/.codex/AGENTS.md` | `~/.copilot/copilot-instructions.md` | Identische Logik für persönliche Standards über alle Repositories hinweg. |
| **Konfiguration & MCP** | `~/.claude/settings.json` und `.mcp.json` | `~/.codex/config.toml` und `.codex/config.toml` | `~/.copilot/config.json` und `.mcp.json` | Codex nutzt **TOML** und bündelt alles zentral; Claude und Copilot setzen auf JSON und `.mcp.json`. |
| **Sandbox & Rechte** | Berechtigungs-Prompts / Auto-Modus; OS-Sandbox opt-in (`/sandbox`) | OS-Level-Sandbox (Seatbelt/Landlock) standardmäßig aktiv | Local Sandbox opt-in (`/sandbox enable`) & GitHub-Scopes | Codex setzt auf harte OS-Grenzen ab Werk; Copilot integriert sich tief in GitHub-Permissions. |
| **Skills & Workflows** | `.claude/skills/<name>/SKILL.md` | `.agents/skills/<name>/SKILL.md` | `.github/skills/`, `.claude/skills/` oder `.agents/skills/` | Copilot und Codex lesen `.agents/skills/` nativ; ein gemeinsamer Skill-Ordner funktioniert überall. |
| **Custom Subagenten** | `.claude/agents/*.md` | Rollen/Subagenten in `config.toml` | `.github/agents/*.md` | Claude und Copilot definieren spezialisierte Subagenten als eigene Markdown-Dateien. |
| **Lifecycle-Hooks** | `settings.json` (`PreToolUse`, `PostToolUse`, `Stop`, ...) | `config.toml` oder `.codex/hooks.json` | `.github/hooks/*.json` (`preToolUse`, `postToolUse`, `agentStop`) | Event-Logik und Exit-Code-Steuerung sind nahezu identisch, nur die Dateipfade variieren. |
| **Headless / CI-Modus** | `claude -p "Prompt"` | `codex exec "Prompt"` | `copilot -p "Prompt"` | Alle drei bieten einen nicht-interaktiven Modus für Skripte und CI/CD-Pipelines. |
| **Cloud & Async-Delegation** | Claude Code on the Web | `codex cloud` & Codex App | `copilot --cloud` & GitHub Agent HQ | Copilot bindet Issues/PRs direkt auf GitHub ein; Codex bietet isolierte Cloud-Container. |
| **Kern-Philosophie** | **Agent-first:** Autonome Multi-File-Schleifen & tiefes Reasoning | **Sandbox- & Cloud-first:** Strikte OS-Isolation & Hintergrund-Tasks | **Platform- & IDE-first:** Inline-Autocomplete, Multi-Modell & GitHub-Hub | Claude glänzt bei großen Refactorings, Codex bei isolierter Ausführung, Copilot im Editor- & GitHub-Alltag. |

### Einordnung der drei Systeme für deinen Workflow

* **Claude Code (Dein Referenzpunkt):** Arbeitet stark agentisch („Plan – Ausführen – Prüfen“) mit großem Kontextfenster und zieht komplexe Multi-File-Aufgaben meist autonom durch, ohne bei jedem Zwischenschritt nachzufragen.
* **OpenAI Codex (CLI & App):** Setzt auf strikte OS-Isolation (Seatbelt unter macOS, Landlock unter Linux) und trennt interaktive Sessions sauber von automatisierten Läufen (`codex exec`) sowie Cloud-Delegation (`codex cloud`).
* **GitHub Copilot (CLI & IDE):** Verbindet Echtzeit-Inline-Vervollständigung im Editor mit einem terminalbasierten Agenten (`copilot`), der mehrere Modellanbieter (OpenAI, Anthropic, Google) unterstützt und native Brücken zu GitHub Issues, PRs und `.github/`-Strukturen schlägt.

---

## 2. Schritt-für-Schritt: Den Codex-Harness einrichten

### Schritt 1: Installation und Authentifizierung
* **Installation durchführen.** Installiere die OpenAI Codex CLI global über npm mit `npm install -g @openai/codex` oder über Homebrew (`brew install --cask codex`).
* **Login einrichten.** Starte `codex` im Terminal, um dich per ChatGPT-Account (Plus/Pro/Team/Enterprise) anzumelden, oder hinterlege einen `OPENAI_API_KEY` über `codex auth`.

### Schritt 2: Instruktions-Layer anlegen (`AGENTS.md`)
* **Globale Regeln definieren.** Erstelle die Datei `~/.codex/AGENTS.md` für deinen persönlichen Coding-Stil, bevorzugte Paketmanager oder Git-Konventionen.
* **Projektregeln anlegen.** Lege im Wurzelverzeichnis deines Repos eine `AGENTS.md` an. Da sowohl **Claude Code** als auch **GitHub Copilot** mittlerweile `AGENTS.md` nativ auswerten, kannst du eine einzige zentrale Anweisungsdatei für alle drei Tools pflegen.
* **Regeln schlank halten.** Schreibe nur dauerhaft gültige Architektur-, Test- und Build-Befehle in die `AGENTS.md` und lagere komplexe Abläufe in Skills aus.

### Schritt 3: Laufzeit und Sandbox konfigurieren (`config.toml`)
* **Konfigurationsdatei erstellen.** Öffne `~/.codex/config.toml` (für globale Defaults) und optional `.codex/config.toml` im Projektverzeichnis, um Standardmodell, Approval-Policy und Sandbox-Stufe festzulegen.
* **Sandbox-Modus wählen.** Nutze `workspace-write` für normales Arbeiten im Projektordner (Dateien im Repo sind beschreibbar, Netzwerkzugriff ist blockiert) oder `read-only` für reine Code-Analysen und Reviews.
* **Kommando-Policies schärfen.** Erzwinge harte Verbote oder Freigaben über Regeln in der `config.toml` statt über reine Text-Hinweise in Markdown, damit destruktive Befehle auf Systemebene blockiert werden.

### Schritt 4: MCP-Server und Skills verbinden
* **MCP-Server registrieren.** Füge externe Tools direkt per CLI hinzu, zum Beispiel mit `codex mcp add <name> -- <befehl>` für lokale Server oder `codex mcp add <name> --url <url>` für Remote-Server. Die Einträge werden automatisch in deiner `config.toml` gespeichert.
* **Skills plattformübergreifend ablegen.** Lege wiederverwendbare Workflows unter `.agents/skills/<skill-name>/SKILL.md` (im Projekt) oder `~/.agents/skills/` (persönlich) ab. Sowohl **Codex** als auch **GitHub Copilot** lesen diesen Pfad nativ; für **Claude Code** kannst du bei Bedarf einen Symlink von `.claude/skills` auf `.agents/skills` setzen.

### Schritt 5: Deterministische Hooks einbauen
* **Feedback-Schleifen erzwingen.** Konfiguriere Hooks in der `config.toml` oder `.codex/hooks.json` (Events wie `PostToolUse` oder `Stop`), um nach Dateiänderungen automatisch Linter, Type-Checks oder Test-Suites auszuführen.
* **Exit-Codes nutzen.** Ein Skript mit Exit-Code `0` lässt Codex fortfahren, während Exit-Code `2` den Abschluss blockiert und die Fehlermeldung an den Agenten zurückspielt, damit dieser das Problem selbstständig behebt.

---

## 3. Praxis-Tipp: Ein gemeinsames Repo-Setup für alle drei Tools

Wenn du Claude Code, Codex und Copilot parallel im selben Repository nutzen möchtest, vermeidest du doppelte Pflege mit dieser Verzeichnisstruktur:

```text
mein-projekt/
├── AGENTS.md                     # Wird von Codex, Claude Code und Copilot gelesen
├── .mcp.json                     # Gemeinsame MCP-Server für Claude Code und Copilot
├── .agents/
│   └── skills/
│       └── code-review/
│           └── SKILL.md          # Nativ von Codex & Copilot gelesen (für Claude symlinken)
├── .codex/
│   └── config.toml               # Codex-spezifische Sandbox-, MCP- und Hook-Regeln
├── .claude/
│   ├── settings.json             # Claude-spezifische Berechtigungen & Hooks
│   └── skills -> ../.agents/skills
└── .github/
    ├── agents/                   # Copilot Custom Agents
    └── hooks/                    # Copilot Lifecycle-Hooks
```
