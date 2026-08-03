---
description: Schließt die Session ab — erzeugt ein Diary (Ziel, Verlauf, nächste Schritte) in diaries/, damit die nächste Session nahtlos anknüpfen kann; unterstützt Forks ab älteren Einträgen
---

Schließe die aktuelle Session mit einem **Diary** ab. Zweck: Wenn der Nutzer das
nächste Mal Claude (oder einen anderen Agenten) in diesem Arbeitsverzeichnis öffnet,
soll die neue Session durch bloßes Lesen des letzten Diarys nahtlos weitermachen
können — als wäre die alte Session nie beendet worden. Kein `--resume` nötig.

Die Diaries bilden dabei einen **verzweigbaren Projektzeitverlauf**: Jeder Eintrag
verweist auf seinen Vorgänger, und eine Session darf auch an einem ÄLTEREN Eintrag
anknüpfen (rückwirkender Fork) — das wird beim nächsten `/wrap` im Diary vermerkt.

Gehe wie folgt vor:

## 1. Diary-Datei anlegen

- Ordner: `<projekt>/diaries/` (anlegen, falls nicht vorhanden).
- Dateiname: `YYYY-MM-DD-HH-MM-N.md` — Datum und Uhrzeit jetzt (via `date`-Kommando
  ermitteln, nicht raten), `N` ist eine fortlaufende Nummer PRO TAG: vorhandene
  Diaries des heutigen Tages zählen, N = Anzahl + 1 (erstes Diary des Tages: N = 1).
- Das neue Diary ist damit chronologisch immer der neueste Eintrag — auch wenn die
  Session inhaltlich an einem älteren Eintrag anknüpft (Fork, siehe Schritt 2).

## 2. Vorgänger und Fork-Status bestimmen

Jedes Diary verweist explizit auf seinen **Vorgänger** — den Eintrag, an dem diese
Session inhaltlich angeknüpft hat. So bleibt der Projektverlauf auch dann
nachvollziehbar, wenn er sich verzweigt.

- **Vorgänger ermitteln:** Das Diary, das zu Beginn dieser Session gelesen bzw. von
  dem aus explizit fortgesetzt wurde. Hat der Nutzer ausdrücklich einen bestimmten
  (älteren) Eintrag als Ausgangspunkt genannt, ist DIESER der Vorgänger. Wurde gar
  kein Diary konsultiert: das zum Sessionstart neueste Diary als Vorgänger annehmen;
  existiert noch keines, ist der Vorgänger „keiner (erstes Diary)".
- **Fork erkennen:** Liste `diaries/` auf. Ist der Vorgänger NICHT der neueste
  vorhandene Eintrag — liegen also zwischen Vorgänger und jetzt bereits andere
  Diaries —, dann ist diese Session ein **Fork**: Sie hat rückwirkend an einem
  früheren Projektstand angesetzt, und die dazwischenliegenden Einträge gehören zu
  einem anderen Strang.
- Bei einem Fork die übersprungenen Einträge (alle Diaries neuer als der Vorgänger)
  notieren — sie kommen in den Fork-Vermerk (Schritt 3). Falls aus der Session
  hervorgeht, WARUM geforkt wurde und was mit dem übersprungenen Strang passieren
  soll (verworfen? parallel weiterverfolgen? später zusammenführen?), das ebenfalls
  festhalten; wenn unklar, den Nutzer im Abschluss (Schritt 5) kurz darauf hinweisen.

## 3. Diary-Inhalt

Fasse die GESAMTE aktuelle Session zusammen — auch Teile, die vor einer etwaigen
Kontext-Komprimierung liegen, soweit rekonstruierbar. Struktur:

```markdown
# Session-Diary — <Datum, Uhrzeit>

**Vorgänger:** `<dateiname.md>` <oder: keiner (erstes Diary)>

<!-- NUR bei einem Fork zusätzlich: -->
**Fork von:** `<vorgänger.md>` — diese Session hat rückwirkend an diesem Stand
angesetzt; die Einträge `<liste der übersprungenen diaries>` gehören zu einem
anderen Strang und sind NICHT Grundlage dieses Eintrags.

## Fork-Vermerk
<!-- NUR bei einem Fork. --> Warum wurde an einem früheren Stand angesetzt, und
was gilt für den übersprungenen Strang: verworfen (dieser Eintrag ersetzt ihn),
parallel (beide Stränge leben weiter) oder später zusammenzuführen? Mit diesem
Eintrag ist der Fork Teil des einheitlichen Projektzeitverlaufs — die nächste
Session knüpft normal am neuesten Diary an.

## Ziel der Session
Was wollten wir in dieser Session erreichen? (Auch, wenn sich das Ziel unterwegs
geändert hat — dann beides nennen.)

## Was geschehen ist
Chronologische, aber verdichtete Zusammenfassung: was wurde gebaut/geändert/
entschieden, mit konkreten Datei-Pfaden und ggf. Commit-Hashes.

## Entscheidungen und Erkenntnisse
Getroffene Entscheidungen MIT Begründung; Verworfenes mit dem Warum (damit es die
nächste Session nicht erneut vorschlägt); offene Fragen.

## Stand bei Session-Ende
Zustand des Arbeitsverzeichnisses: Was ist committet, was liegt uncommittet vor,
was ist halbfertig?

## Nächste Schritte
Konkret und priorisiert: Womit machen wir weiter? Was war als Nächstes vereinbart
oder in Aussicht gestellt? Was sollte die nächste Session als Erstes tun?
```

Schreibe fürs Wiederanknüpfen, nicht fürs Protokoll: Die nächste Session kennt NUR
das Repo und dieses Diary — keine Codenamen oder Abkürzungen aus dem
Sessionverlauf verwenden, ohne sie zu erklären.

## 4. In CLAUDE.md und anderen relevanten Dateien verankern

- Prüfe, ob `CLAUDE.md` bereits einen Abschnitt über die Diaries enthält (inkl.
  Fork-Absatz — ältere Fassungen ohne ihn entsprechend ergänzen).
  Falls nein, ergänze (sinngemäß):

  ```markdown
  ## Session-Diaries

  In `diaries/` liegt pro Arbeitssession ein Diary (`YYYY-MM-DD-HH-MM-N.md`) mit
  Ziel, Verlauf, Entscheidungen, Endstand und nächsten Schritten der Session.
  Jedes Diary nennt im Kopf seinen **Vorgänger** — den Eintrag, an dem es
  anknüpft; darüber ist der Verlauf auch bei Verzweigungen nachvollziehbar.

  **Zu Sitzungsbeginn IMMER zuerst das neueste Diary lesen** und die Arbeit von
  dessen „Nächste Schritte" aus fortsetzen, sofern der Nutzer nichts anderes
  vorgibt. Ältere Diaries nur bei Bedarf nachschlagen.

  **Fork / Resume ab einem älteren Eintrag:** Sagt der Nutzer, dass ab einem
  bestimmten (älteren) Diary fortgesetzt werden soll, dann NUR dieses Diary und
  seine Vorgängerkette als Arbeitsstand nehmen — neuere Einträge bewusst
  ignorieren. Das nächste `/wrap` vermerkt den Fork dann im neuen Diary.
  Sessions werden mit `/wrap` abgeschlossen, das das nächste Diary erzeugt.
  ```

- Falls eine Pflegeliste für `/commit` in `CLAUDE.md` existiert: `diaries/` dort
  ergänzen, falls noch nicht enthalten.
- Falls `README.md` oder eine `AGENTS.md` existiert und dort Projektstruktur oder
  Arbeitsweise beschrieben ist: die Diaries dort in einem Satz erwähnen.
- Prüfe `.gitignore`: `diaries/` darf nicht ignoriert werden.

## 5. Abschluss

- Kurz berichten: Pfad des erzeugten Diarys und die „Nächste Schritte" daraus
  (damit der Nutzer sie noch korrigieren kann, solange die Session offen ist).
- Bei einem Fork zusätzlich nennen: Vorgänger, übersprungene Einträge und wie der
  übersprungene Strang eingestuft wurde (verworfen/parallel/zusammenzuführen) —
  und nachfragen, falls diese Einstufung aus der Session nicht klar hervorging.
- Nicht automatisch committen; falls das Projekt einen `/commit`-Befehl kennt,
  darauf hinweisen, dass ein `/commit` jetzt sinnvoll wäre.

$ARGUMENTS
