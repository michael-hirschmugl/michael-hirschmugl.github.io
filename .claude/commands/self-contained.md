---
description: Wandelt das aktuelle Arbeitsverzeichnis in ein self-contained Projekt um — alles Projektwissen wird ins Verzeichnis migriert und dort gepflegt
---

Wandle das aktuelle Arbeitsverzeichnis in ein **self-contained Projekt** um. Ziel: Das
Verzeichnis (bzw. das Git-Repo) enthält ALLES, was es direkt und indirekt über dieses
Projekt zu wissen gibt. Es muss verlustfrei auf einen anderen Rechner oder zu einem
anderen Agenten umziehen können — auch wenn dort keine der benutzereigenen
`~/.claude`-Daten existieren. Nach der Umwandlung ist das Repo der EINZIGE Speicherort
für Projektwissen: Es dürfen keine externen Memories zu diesem Projekt mehr erzeugt
werden. Der Befehl ist idempotent: Ein erneuter Lauf ist ein Reparatur-/Prüflauf, der
regelwidrig doch extern entstandene Reste nachmigriert und die Stilllegung erneuert.

Gehe wie folgt vor:

## 1. Externes Projektwissen finden

- Ermittle das Memory-Verzeichnis dieses Projekts:
  `~/.claude/projects/<kodierter-pfad>/memory/`, wobei der kodierte Pfad der absolute
  Projektpfad mit `/` und `.` jeweils ersetzt durch `-` ist
  (Beispiel: `/home/michael/Documents/git/foo` → `-home-michael-Documents-git-foo`).
- Prüfe außerdem `~/.claude/CLAUDE.md` (globale Nutzer-Anweisungen) auf Regeln, die
  sich spezifisch auf DIESES Projekt beziehen. Allgemeine persönliche Präferenzen
  gehören NICHT ins Repo.
- Prüfe `~/.claude.json` bzw. `~/.claude/settings.json` auf projektspezifische
  Einträge (z. B. MCP-Server, die nur dieses Projekt braucht).

## 2. Ins Projekt migrieren

- Kopiere die Projekt-Memories nach `<projekt>/.claude/memory/` (inkl. eines
  Index `MEMORY.md`). Bei erneutem Lauf: abgleichen und nur Neues/Geändertes
  übernehmen; Konflikte zugunsten der aktuelleren Fassung auflösen.
- **Personenbezogene Inhalte:** Memories vom Typ `user` (Infos über den Nutzer selbst)
  nur übernehmen, soweit sie fürs Projekt nötig sind, und den Nutzer im
  Abschlussbericht ausdrücklich darauf hinweisen, dass diese Daten jetzt im Repo
  liegen (relevant, falls das Repo jemals öffentlich gepusht wird).
- Kopiere diese Command-Datei selbst nach `<projekt>/.claude/commands/self-contained.md`,
  falls dort noch nicht vorhanden — damit der Befehl auch auf dem Zielrechner existiert.
- Projektrelevante Slash-Commands/Skills aus `~/.claude/commands/` bzw.
  `~/.claude/skills/`, die dieses Projekt braucht, ebenfalls nach
  `<projekt>/.claude/commands/` bzw. `.claude/skills/` kopieren.
- Teile aus `.claude/settings.local.json`, die das Projekt auf jedem Rechner braucht
  (z. B. Permission-Allowlist), nach `.claude/settings.json` verschieben.
  Maschinenspezifisches (absolute Pfade, Credentials) bleibt in `settings.local.json`.
  **Niemals Secrets/Tokens ins Repo übernehmen.**

## 2b. Externes Memory-Verzeichnis stilllegen

Nach erfolgreicher Migration wird das externe Memory-Verzeichnis stillgelegt, damit
dort nichts mehr entsteht oder veraltet weiterexistiert:

- Lösche die migrierten Memory-Dateien in `~/.claude/projects/<kodierter-pfad>/memory/`.
- Ersetze dessen `MEMORY.md` durch einen einzigen Hinweis:
  „Dieses Projekt ist self-contained. Alle Memories liegen im Repo unter
  `.claude/memory/` — NICHTS hier speichern, Änderungen nur im Repo."
  So wird jede Session, die nur den externen Index lädt, sofort umgeleitet.

## 3. Versionierung sicherstellen

- Prüfe `.gitignore`: `CLAUDE.md`, `.claude/memory/`, `.claude/commands/`,
  `.claude/skills/`, `.claude/settings.json` dürfen NICHT ignoriert werden.
  `.claude/settings.local.json` soll ignoriert bleiben. Passe `.gitignore` bei
  Bedarf an.

## 4. CLAUDE.md verankern

Erzeuge oder ergänze `<projekt>/CLAUDE.md` um einen Abschnitt wie diesen (sinngemäß,
an das Projekt angepasst; bei erneutem Lauf nicht duplizieren):

```markdown
## Self-contained-Projekt

Dieses Projekt ist self-contained: Sämtliches Projektwissen liegt im Repo selbst.

- Projekt-Memories liegen in `.claude/memory/` (Index: `.claude/memory/MEMORY.md`).
  Lies den Index zu Sitzungsbeginn.
- Neues dauerhaftes Projektwissen wird AUSSCHLIESSLICH hier im Repo gespeichert —
  in `.claude/memory/` bzw. in `docs/`. Es dürfen KEINE Memories zu diesem Projekt
  in benutzer- oder maschinenspezifischen Verzeichnissen (z. B. `~/.claude/`)
  angelegt werden; das dortige Memory-Verzeichnis ist stillgelegt.
- Gilt für jeden Agenten, der an diesem Repo arbeitet, nicht nur für Claude Code.
- Externe Voraussetzungen (nicht migrierbar): <hier auflisten, z. B. benötigte
  CLI-Tools, Zugangsdaten, MCP-Server — oder "keine">
```

- Falls keine `AGENTS.md` existiert, lege eine an, die auf `CLAUDE.md` verweist
  (eine Zeile genügt), damit auch andere Agenten die Regeln finden.

## 5. Abschlussbericht

Berichte am Ende:
- was migriert wurde (Dateien, Anzahl Memories),
- was NICHT migrierbar war und auf dem Zielrechner manuell eingerichtet werden muss
  (Credentials, installierte Tools, MCP-Server, Betriebssystem-Abhängigkeiten),
- ob personenbezogene Daten ins Repo gelangt sind,
- Hinweis, dass die Änderungen noch zu committen sind (nicht automatisch committen,
  außer der Nutzer verlangt es).

$ARGUMENTS
