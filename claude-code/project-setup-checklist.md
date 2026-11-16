# Checkliste: Claude-Code-Umgebung für ein neues Projekt

### 1. Grundlagen
- [ ] **Projektordner anlegen und in VS Code öffnen.** Claude Code startet immer im geöffneten Ordner.
- [ ] **`git init` ausführen** und eine `.gitignore` anlegen. Viele Funktionen brauchen Git, etwa `/code-review`, Worktrees und Commits. Git ist außerdem dein Sicherheitsnetz, falls Claude etwas falsch ändert.
- [ ] **Geheimnisse ausschließen:** `.env` und andere Dateien mit Schlüsseln gehören in die `.gitignore`.
- [ ] **Build-, Test- und Lint-Befehle einrichten.** Claude kann seine Änderungen nur prüfen, wenn es Tests ausführen kann.

### 2. Projektwissen für Claude
- [ ] **`/init` ausführen**, sobald erster Code existiert. Daraus entsteht eine `CLAUDE.md`.
- [ ] **`CLAUDE.md` von Hand ergänzen:** Befehle (Build, Tests, einzelner Test), Architektur, Konventionen und Stolperfallen. Halte sie kurz und konkret.
- [ ] **Globale Vorlieben in `~/.claude/CLAUDE.md` festhalten**, etwa „antworte auf Deutsch“. Diese Datei gilt für alle Projekte.

### 3. Berechtigungen und Sicherheit
- [ ] **`.claude/settings.json`** enthält Regeln für das ganze Team und wird eingecheckt. Dort erlaubst du z. B. `npm test` oder `git status`.
- [ ] **`.claude/settings.local.json`** enthält deine persönlichen Einstellungen. Trag sie in die `.gitignore` ein.
- [ ] **Sensible Dateien sperren**, z. B. mit `"deny": ["Read(./.env)"]`.
- [ ] Nach den ersten Sitzungen **`/fewer-permission-prompts` ausführen**. Das Kommando legt häufige, harmlose Befehle automatisch auf die Erlaubnisliste.

### 4. Erweiterungen (je nach Bedarf)
- [ ] **MCP-Server** (Datenbank, GitHub, Browser usw.) mit `claude mcp add --scope project …` hinzufügen. Sie landen in der `.mcp.json`.
- [ ] **Hooks** für automatische Abläufe einrichten, z. B. den Formatter nach jeder Dateiänderung laufen lassen. Bitte Claude einfach darum, es nutzt dafür den Skill `update-config`.
- [ ] **Eigene Skills/Slash-Commands** für wiederkehrende Abläufe unter `.claude/skills/<name>/SKILL.md` ablegen.
- [ ] **Subagents** für spezielle Rollen, z. B. einen Reviewer, unter `.claude/agents/` ablegen.

### 5. Feinschliff
- [ ] **Modell und Einstellungen** mit `/model` und `/config` prüfen.
- [ ] **Statuszeile** optional mit `/statusline` einrichten.
- [ ] **Ersten Commit machen**, und zwar mit `CLAUDE.md`, `.claude/settings.json` und `.mcp.json`. Dann hat jede neue Sitzung (und jedes Teammitglied) dieselbe Umgebung.

**Tipp:** Pflege die `CLAUDE.md` laufend. Wenn Claude etwas wiederholt falsch macht, schreib die Korrektur dort hinein.

Soll ich die Checkliste als Datei speichern, etwa als Vorlage, die du in jedes neue Projekt kopierst? Oder soll ich gleich ein Grundgerüst anlegen, mit `.gitignore`, `.claude/settings.json` und einer leeren `CLAUDE.md`?