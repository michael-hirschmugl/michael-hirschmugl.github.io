---
name: links-html-quelle-proxmox-notiz
description: Die Wahrheit über Homelab-Container steht in der Obsidian-Notiz "Proxmox Server"; links.html ist deren öffentliche Kurzfassung
metadata:
  type: reference
---

Die maßgebliche Übersicht über den Homelab-Server (LXC-Container, VMs, IPs,
Cloudflare-Tunnels, Scripts, Backups) ist die Obsidian-Notiz
`Projects/Homelab & Infrastruktur/Proxmox Server.md` im Vault
`/home/michael/Obsidian/obsidian-notes-2025` (Vault-Regeln dort in `CLAUDE.md`
und `AI-README.md` — Deutsch, Wikilinks, keine Massen-Reformatierung).

`links.html` in diesem Repo ist nur die öffentliche Kurzfassung davon.

**Why:** Wer nur `links.html` ändert, hinterlässt die eigentliche Doku veraltet —
und umgekehrt. Der Vault steht unter keiner Versionskontrolle (Sync über CouchDB
LiveSync, CT 103), Löschungen dort sind also nicht per git wiederherstellbar.

**How to apply:** Bei Änderungen an der Homelab-Übersicht immer beide Seiten
nachziehen. Im Vault stillgelegte Container ins Archiv-Kapitel verschieben statt
zu löschen. Zugangsdaten aus der Notiz (sie enthält Klartext-Passwörter) niemals
nach `links.html` übernehmen — siehe [[lan-links-in-public-repo]].
