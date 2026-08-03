---
description: Aktualisiert Doku und Memories (laut Pflegeliste in CLAUDE.md) und erstellt dann einen Commit — ohne Push
---

Bringe die Projekt-Dokumentation auf den Stand der aktuellen Änderungen und erstelle
dann einen Commit. Es wird NIE gepusht.

Gehe wie folgt vor:

## 1. Pflegeliste ermitteln

- Lies `CLAUDE.md` im Projektwurzelverzeichnis. Maßgeblich ist der Abschnitt
  `## Pflegeliste (/commit)`: Er listet die Dateien, die bei jedem Commit auf
  Aktualität geprüft werden müssen, je Eintrag mit einem Satz, WANN eine
  Aktualisierung nötig ist.
- Existiert der Abschnitt (oder die ganze `CLAUDE.md`) noch nicht: Lege ihn jetzt an.
  Erstelle die initiale Liste aus dem, was im Repo tatsächlich existiert — typische
  Kandidaten: `CLAUDE.md` selbst, `README.md` und weitere README-Dateien,
  Projekt-Memories (bei self-contained Projekten `.claude/memory/` samt Index
  `MEMORY.md`, sonst das externe Memory-Verzeichnis unter
  `~/.claude/projects/<kodierter-pfad>/memory/`), generierte Übersichten
  (z. B. ein `STATUS.md`), zentrale Doku unter `docs/`. Beispielformat:

  ```markdown
  ## Pflegeliste (/commit)

  Diese Dateien bei jedem Commit prüfen und bei Bedarf aktualisieren:

  - `README.md` — wenn sich Zweck, Struktur oder Benutzung des Projekts geändert hat
  - `CLAUDE.md` — wenn sich Konventionen oder Arbeitsweise geändert haben
  - `.claude/memory/` — wenn die Änderungen dauerhaftes Projektwissen betreffen
  - `docs/…` — wenn das jeweilige Konzept von den Änderungen berührt ist
  ```

## 2. Änderungen sichten

- `git status` und `git diff` (inkl. bereits gestagter Änderungen und neuer Dateien):
  Was hat sich seit dem letzten Commit inhaltlich getan?

## 3. Pflegeliste abarbeiten

- Prüfe für JEDEN Eintrag der Pflegeliste, ob die aktuellen Änderungen ihn berühren.
- Aktualisiere die betroffenen Dateien; lege in der Pflegeliste genannte, aber noch
  fehlende Dateien an (z. B. eine erste `README.md`).
- Memories: Nur dauerhaftes, nicht aus dem Code ableitbares Projektwissen ergänzen
  oder korrigieren — keine Zusammenfassung des Commits. Bei self-contained Projekten
  ausschließlich im Repo (`.claude/memory/`), niemals extern.
- Nichts zu tun ist ein gültiges Ergebnis — keine kosmetischen Pflicht-Edits, nur
  echte Aktualisierungen.
- Fällt dabei auf, dass die Pflegeliste selbst veraltet ist (Datei gelöscht/
  umbenannt, neue pflegewürdige Datei entstanden), aktualisiere die Liste mit.

## 4. Committen (kein Push!)

- Stage die inhaltlichen Änderungen UND die soeben aktualisierte Doku zusammen.
  Stage gezielt die relevanten Dateien, kein pauschales `git add -A`; offensichtlich
  nicht Zugehöriges (Build-Artefakte, Temporäres) bleibt draußen und wird im Bericht
  erwähnt.
- Eine aussagekräftige Commit-Message zum inhaltlichen Kern der Änderung (die
  Doku-Aktualisierungen sind Beiwerk und gehören nicht in die Überschrift).
- KEIN `git push` — auch dann nicht, wenn ein Remote konfiguriert ist.

## 5. Kurzbericht

- Welche Pflegelisten-Einträge wurden aktualisiert, welche waren aktuell?
- Commit-Hash und -Message.
- Was absichtlich nicht gestagt wurde.

$ARGUMENTS
