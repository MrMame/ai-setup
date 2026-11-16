# Leitfaden: Von der Projektidee zur Spezifikation und zum Techstack

Dieser Leitfaden hilft dir, aus einer groben Idee eine klare Spezifikation zu machen und daraus einen passenden Techstack abzuleiten. Er funktioniert für dich allein und auch als Grundlage für eine Beratung mit Claude (siehe [Abschnitt 9](#9-claude-als-berater-nutzen)).

**Grundregel:** Den Techstack wählst du nicht zuerst. Er ergibt sich aus den Anforderungen und deinen Rahmenbedingungen.

---

## Überblick: Phasen und Fachbegriffe

| Phase | Fachbegriff | Ergebnis |
|---|---|---|
| 1. Idee schärfen | Produktvision / Discovery | Vision, Problem Statement |
| 2. Anforderungen sammeln | Requirements Engineering (Anforderungsanalyse) | funktionale und nicht-funktionale Anforderungen |
| 3. Kern festlegen | MVP-Scoping | priorisierte Funktionsliste |
| 4. Rahmenbedingungen klären | Constraints | Liste von Vorgaben und Grenzen |
| 5. Stack wählen | Software-Architektur / technisches Design | Stack-Entscheidung |
| 6. Ausprobieren | Spike / Proof of Concept (PoC) | Erkenntnisse aus einem Wegwerf-Prototyp |
| 7. Festhalten | Spezifikation + ADR (Architecture Decision Record) | `SPEC.md`, `CLAUDE.md`, ADRs |

Verwandte Begriffe: **Lastenheft** (was der Auftraggeber will), **Pflichtenheft** (wie es umgesetzt wird), **PRD** (Product Requirements Document), **SRS** (Software Requirements Specification), **User Stories** (agile Form von Anforderungen). Arbeitest du mit KI-Agenten, heißt dieses Vorgehen oft **Spec-Driven Development**: Zuerst entsteht die Spezifikation, danach baut der Agent nach ihr.

---

## 1. Die Idee schärfen (noch ohne Technik)

Beantworte in ein paar Sätzen:

- [ ] **Was** soll das Ding tun? Welches Problem löst es?
- [ ] **Für wen** ist es: nur für dich, für ein paar Leute oder für viele Nutzer?
- [ ] **Warum** gibt es das noch nicht, oder was ist an bestehenden Lösungen schlecht?
- [ ] **Woran** erkennst du, dass das Projekt erfolgreich ist?

## 2. Anforderungen ableiten

### Funktionale Anforderungen (was das System tut)
Formuliere sie am besten als User Stories:
> *Als **[Rolle]** möchte ich **[Funktion]**, damit **[Nutzen]**.*

### Nicht-funktionale Anforderungen (wie gut es das tut)
Diese Fragen bestimmen den Stack am stärksten:

- [ ] **Plattform:** Browser, Smartphone (iOS/Android), Desktop, CLI/Skript, Server oder Gerät (z. B. Raspberry Pi)?
- [ ] **Benutzerkonten/Login** nötig? Gibt es Rollen und Rechte?
- [ ] **Daten:** Was wird gespeichert, wie viel, wie strukturiert? Sind es sensible Daten (Datenschutz/DSGVO)?
- [ ] **Echtzeit:** Chat, Live-Updates, Benachrichtigungen?
- [ ] **KI/LLM**, Datenverarbeitung oder externe APIs?
- [ ] **Offline-Fähigkeit**, Hardwarezugriff (Kamera, GPS, Dateien, Sensoren)?
- [ ] **Last:** Wie viele gleichzeitige Nutzer sind realistisch?
- [ ] **Kosten/Hosting:** kostenlos, selbst gehostet oder Cloud? Wie hoch ist das Budget pro Monat?

## 3. Den kleinsten sinnvollen Kern (MVP) festlegen

- [ ] Wähle **3–5 Funktionen**, ohne die das Produkt keinen Sinn ergibt.
- [ ] Ordne alles andere einer Prioritätsstufe zu, etwa mit **MoSCoW**:
  - **Must:** gehört ins MVP
  - **Should:** wichtig, kommt direkt danach
  - **Could:** schön zu haben
  - **Won't (for now):** bewusst ausgeschlossen
- [ ] Schreib auf, was **nicht** gebaut wird (Non-Goals). Das schützt vor Scope Creep, also dem schleichenden Anwachsen des Projekts.

## 4. Rahmenbedingungen klären

Sie sind mindestens genauso wichtig wie die Technik:

- [ ] Welche **Programmiersprachen und Tools** kennst du schon, auch nur ein bisschen?
- [ ] Ist das Ziel, etwas zu **lernen** oder **schnell fertig** zu werden? Daraus folgen oft unterschiedliche Entscheidungen.
- [ ] Wie viel **Zeit** hast du, gemessen in Stunden pro Woche und bis zu einem Zieltermin?
- [ ] Arbeitest du **allein oder im Team**?
- [ ] Gibt es **Vorgaben**, z. B. ein bestimmtes Betriebssystem, vorhandene Server oder Firmenrichtlinien?

## 5. Techstack wählen: lieber langweilig als schick

### Faustregeln
- **Bekanntes schlägt Neues.** Mit einer Sprache, die du kennst, kommst du schneller voran.
- **Große Community** bringt mehr Doku und Beispiele. Auch KI-Assistenten helfen bei verbreiteten Technologien besser.
- **So wenige Bausteine wie möglich:** eine Sprache, ein Framework, eine Datenbank, ein Hosting.
- **Reife statt Hype.** Ein Werkzeug sollte schon ein paar Jahre stabil gepflegt werden.
- **Spätere Wechsel bedenken.** Wie schwer ist es, eine Komponente später auszutauschen?

### Bausteine, die du festlegen musst
| Baustein | Beispiele |
|---|---|
| Frontend | HTML/JS, React, Vue, Svelte; mobil: Flutter, React Native |
| Backend | Node.js, Python (FastAPI/Django), C#/.NET, Go, oder gar keins (Backend-as-a-Service) |
| Datenbank | SQLite, PostgreSQL, MongoDB, Supabase/Firebase |
| Authentifizierung | selbst gebaut, Auth-Dienst (z. B. Supabase Auth, Auth0) |
| Hosting | lokal, VPS, Vercel/Netlify, Docker auf eigenem Server, Cloud |
| Tests & Qualität | Test-Framework, Linter, Formatter |

### Vergleich mehrerer Optionen
Vergleiche 2–3 Kandidaten in einer Tabelle und bewerte sie z. B. von 1–5:

| Kriterium | Option A | Option B | Option C |
|---|---|---|---|
| Passt zu meinen Kenntnissen | | | |
| Erfüllt die Anforderungen | | | |
| Community & Doku | | | |
| Kosten | | | |
| Aufwand bis zum MVP | | | |

## 6. Prototyp bauen (Spike)

- [ ] Wähle die **riskanteste oder wichtigste Funktion**.
- [ ] Baue sie in **1–2 Tagen** als Wegwerf-Prototyp.
- [ ] Prüfe danach: Passt der Stack? Wo hakt es? Sind Annahmen falsch gewesen?
- [ ] Passe bei Bedarf Spezifikation oder Stack an, **bevor** du richtig loslegst.

## 7. Entscheidungen festhalten

- [ ] **`SPEC.md`** im Projekt anlegen (Vorlage siehe Abschnitt 8).
- [ ] **ADR** für wichtige Entscheidungen schreiben, z. B. unter `docs/adr/0001-techstack.md`:
  > **Kontext:** Welches Problem? · **Entscheidung:** Was wurde gewählt? · **Alternativen:** Was wurde verworfen? · **Begründung:** Warum? · **Folgen:** Was bedeutet das?
- [ ] **`CLAUDE.md`** mit Stack, Befehlen und Konventionen anlegen, damit Claude in jeder Sitzung den Kontext hat (siehe [project-setup-checklist.md](project-setup-checklist.md)).

---

## 8. Vorlage: SPEC.md

Kopiere diese Vorlage in ein neues Projekt und fülle sie aus:

```markdown
# Spezifikation: <Projektname>

## Vision
<1–3 Sätze: Was ist es, für wen, welches Problem löst es?>

## Zielgruppe
<Wer nutzt es? Wie viele?>

## Erfolgskriterien
- <Woran erkenne ich, dass es funktioniert?>

## Funktionale Anforderungen (User Stories)
### Must (MVP)
- Als <Rolle> möchte ich <Funktion>, damit <Nutzen>.
### Should
- …
### Could
- …

## Non-Goals (bewusst nicht enthalten)
- …

## Nicht-funktionale Anforderungen
- Plattform:
- Login/Rollen:
- Daten & Datenschutz:
- Echtzeit/Offline:
- Last:
- Budget/Hosting:

## Rahmenbedingungen
- Vorkenntnisse:
- Ziel (Lernen / schnell fertig):
- Zeit:
- Team:

## Techstack
| Baustein | Wahl | Begründung |
|---|---|---|
| Frontend | | |
| Backend | | |
| Datenbank | | |
| Auth | | |
| Hosting | | |

## Offene Fragen & Risiken
- …

## Meilensteine
1. Spike / Prototyp
2. MVP
3. …
```

---

## 9. Claude als Berater nutzen

Starte in einem neuen Projektordner eine Claude-Code-Sitzung und kopiere diesen Prompt hinein:

```text
Ich habe eine Projektidee und möchte, dass du mich als erfahrener Software-Architekt
berätst. Halte dich an den Leitfaden in app-spec-dev.md.

Gehe Phase für Phase vor und stelle mir pro Phase nur wenige, gezielte Fragen.
Warte jeweils meine Antworten ab. Triff keine Stack-Entscheidung, bevor
Anforderungen, MVP und Rahmenbedingungen geklärt sind.

Schlage mir danach 2–3 Stack-Optionen mit Vor- und Nachteilen vor und gib eine
klare Empfehlung. Erstelle am Ende eine SPEC.md nach der Vorlage im Leitfaden
und ein ADR für die Stack-Entscheidung.

Meine Idee: <hier in ein paar Sätzen beschreiben>
```

**Tipps für die Beratung:**
- Antworte ehrlich mit „weiß ich nicht“, wenn du unsicher bist. Dann kann Claude dir Optionen erklären.
- Frag nach, wenn du einen Begriff nicht kennst.
- Lass Claude nach dem Spike die Spezifikation mit den neuen Erkenntnissen aktualisieren.
- Liegt der Leitfaden nicht im Projektordner, gib Claude den vollständigen Pfad zur Datei an oder kopiere den Inhalt hinein.
