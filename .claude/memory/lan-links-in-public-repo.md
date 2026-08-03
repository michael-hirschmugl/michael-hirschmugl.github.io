---
name: lan-links-in-public-repo
description: links.html ist ein privates LAN-Dashboard, wird aber öffentlich über GitHub Pages ausgeliefert
metadata:
  type: project
---

`links.html` ist ein persönliches Dashboard und enthält Links in das private
Heimnetz des Nutzers (u. a. `192.168.0.146:8123` Home Assistant, `192.168.0.193:8006`
Proxmox, `192.168.0.87:8096` Jellyfin) sowie seit 2026-08-03 eine Kurzübersicht der
Proxmox-Backups (Storage-Namen, Datum, Größe). Dieses Repo ist
das öffentliche GitHub-Pages-Repo `michael-hirschmugl/michael-hirschmugl.github.io`
und wird unter `www.michael-hirschmugl.com` (siehe `CNAME`) ausgeliefert — die
Datei ist also für jeden im Internet lesbar.

**Why:** Die Seiten sind nur aus dem LAN erreichbar, aber die Existenz und Struktur
der internen Dienste ist damit öffentlich einsehbar. Das ist eine bewusste
Alltags-Bequemlichkeit, kein Versehen — aber jede Änderung an `links.html` sollte
im Bewusstsein dieser Öffentlichkeit erfolgen.

**How to apply:** Beim Ergänzen von Links keine Zugangsdaten, Tokens oder
Query-String-Secrets in `links.html` aufnehmen. Wenn der Nutzer künftig private
Einträge hinzufügen will, aktiv auf die Öffentlichkeit des Repos hinweisen und ggf.
eine nicht deploybare Alternative vorschlagen. Gilt sinngemäß auch für
[[repo-ist-oeffentliche-github-pages-seite]] und für alles unter `.claude/`.
