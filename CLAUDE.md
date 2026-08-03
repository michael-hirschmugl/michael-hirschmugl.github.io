# CLAUDE.md

Persönliche Website von Michael Hirschmugl, statisch über **GitHub Pages** unter
`www.michael-hirschmugl.com` (siehe `CNAME`) ausgeliefert. Kein Build-Schritt, kein
Framework, keine Abhängigkeiten: Jede Seite ist eine einzelne HTML-Datei mit
inline-CSS und -JS.

- `index.html` — Portfolio-Startseite (Embedded & FPGA Engineering), EN
- `links.html` — persönliches Link-Dashboard inkl. Homelab-Diensten und
  Backup-Übersicht, DE (siehe `.claude/memory/`)
- `snake.html` — Snake-Spiel, DE
- `links.bak` — ältere Fassung der Linksammlung
- Bilddateien im Wurzelverzeichnis werden direkt von den HTML-Seiten referenziert

Änderungen wirken sich unmittelbar auf die öffentliche Seite aus, sobald auf
`main` gepusht wird.

## Self-contained-Projekt

Dieses Projekt ist self-contained: Sämtliches Projektwissen liegt im Repo selbst.

- Projekt-Memories liegen in `.claude/memory/` (Index: `.claude/memory/MEMORY.md`).
  Lies den Index zu Sitzungsbeginn.
- Neues dauerhaftes Projektwissen wird AUSSCHLIESSLICH hier im Repo gespeichert —
  in `.claude/memory/` bzw. in `docs/`. Es dürfen KEINE Memories zu diesem Projekt
  in benutzer- oder maschinenspezifischen Verzeichnissen (z. B. `~/.claude/`)
  angelegt werden; das dortige Memory-Verzeichnis ist stillgelegt.
- Die projektrelevanten Slash-Commands liegen in `.claude/commands/`
  (`/commit`, `/wrap`, `/self-contained`) und ziehen mit dem Repo um.
- Gilt für jeden Agenten, der an diesem Repo arbeitet, nicht nur für Claude Code.
- Externe Voraussetzungen (nicht migrierbar):
  - Git mit SSH-Zugang zu `git@github.com:michael-hirschmugl/michael-hirschmugl.github.io.git`
    (Deploy = Push auf `main`).
  - Ein Browser zum Sichten der Seiten — sonst nichts. Keine Toolchain, keine
    Credentials, keine MCP-Server.
  - Die in `links.html` verlinkten Dienste laufen im privaten Heimnetz des Nutzers
    (`192.168.0.x`) und sind von außerhalb dieses Netzes nicht erreichbar.
  - Die Homelab-Daten in `links.html` (Container, IPs, Backups) stammen aus der
    Obsidian-Notiz `Projects/Homelab & Infrastruktur/Proxmox Server.md` im Vault
    `~/Obsidian/obsidian-notes-2025` — dort steht die maßgebliche Fassung, hier
    nur die öffentliche Kurzfassung. Siehe `.claude/memory/`.

## Pflegeliste (/commit)

Diese Dateien bei jedem Commit prüfen und bei Bedarf aktualisieren:

- `CLAUDE.md` — wenn sich Struktur, Konventionen oder Arbeitsweise geändert haben
- `.claude/memory/` samt Index `MEMORY.md` — wenn die Änderungen dauerhaftes,
  nicht aus dem Code ableitbares Projektwissen betreffen
- `.claude/commands/` — wenn ein Workflow-Befehl angepasst wurde
- `diaries/` — sofern vorhanden: das aktuelle Session-Diary (`/wrap`)
