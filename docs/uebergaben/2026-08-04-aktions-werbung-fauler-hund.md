# Übergabe 2026-08-04 — Wechselnde Aktions-Werbung („Der Faule Hund")

> **Quelle:** Andreas' Auftrag nach dem Besuch bei Mike — Mike wünscht sich eine Werbung
> beim Seitenaufruf, **im monatlichen oder 3-monatlichen Wechsel**.
> **Kern:** Aktion erscheint 1,5 s nach dem Laden — am Rechner als Karte in der Mitte, am
> Handy als Streifen unten. Inhalte stehen in **einer** Datei mit Von/Bis-Datum; der Wechsel
> ist ab jetzt Datenpflege, kein Code. Live und per `curl` belegt.

## Ergebnis — LIVE

- **`info-rentus.de`** — `aktionen.js` liefert 200, `?v=38` im HTML angekommen, beide
  Ansichten (1280 / 500 px) auf der echten Domain per Screenshot geprüft.
- **`pelczer.de/rentus/`** — gespiegelt und gepusht.
- `3d-check/` nicht angefasst.

| Repo | Commit |
|---|---|
| Glanzgarage (Quelle) | `1da8487` |
| info-rentus (Live-Domain) | `6f51273` |
| deadrabbit-landing (pelczer.de) | `bd8c7ab` |

## Wo was liegt

| Datei | Rolle |
|---|---|
| `site/assets/js/aktionen.js` | **Die einzige Datei für den Aktionswechsel.** Nur Daten, keine Logik. Anleitung als Kommentar im Kopf. |
| `site/assets/js/main.js` (unten) | Wann und wie gezeigt wird — Datumsprüfung, Merker, Auf-/Zumachen. |
| `site/assets/css/style.css` (unten) | Aussehen. Block `AKTION` ganz am Ende. |
| `site/index.html` | Nur zwei Zeilen: `aktionen.js` wird **vor** `main.js` eingebunden (Reihenfolge ist Pflicht — main.js liest `window.RENTUS_AKTIONEN`). |
| `site/assets/img/dog.jpg` | Das Foto. Lag seit jeher ungenutzt im Repo: Hund im roten Oldtimer. |

## Wie die nächste Aktion eingehängt wird

1. `site/assets/js/aktionen.js` öffnen — unter der aktiven Aktion steht eine **auskommentierte
   Vorlage**. Kommentar-Sternchen drumherum weg, Werte ausfüllen.
2. Zeitraum über `von` / `bis` (`JJJJ-MM-TT`, beide Tage zählen mit). Die erste Aktion, deren
   Zeitraum aufs heutige Datum passt, gewinnt. **Passt keine, wird nichts angezeigt** — eine
   leere Liste ist erlaubt und lässt die Seite sauber.
3. `?v=` in `index.html` hochzählen (Hausregel), spiegeln, pushen.

**Feld `bildFokus`** steuert, welcher Punkt im Foto beim Beschneiden sichtbar bleibt
(`"62% 30%"` = etwas rechts, ziemlich weit oben). Am Handy wird das Bild auf 96 px Breite
beschnitten — ohne Fokus sieht man beim Hund nur rotes Blech. Weglassen = Mitte.

**Zum Ausprobieren:** `info-rentus.de/?aktion=test` zeigt die erste Aktion der Liste
**immer**, egal welches Datum und egal ob schon gesehen. Das ist auch der Link, den Mike
bekommt, wenn er sie nochmal sehen will.

## Entscheidungen (und warum)

- **Kein Vollbild-Intro.** Mikes Wunsch klang nach Ladebildschirm. Google straft
  Vollbild-Einblendungen auf dem Handy ab — das hätte die SEO-Arbeit vom 27./28.07.
  untergraben. Deshalb: Karte am Rechner (da ist ein Overlay unkritisch), Streifen unten am
  Handy. Der Hero bleibt dabei vollständig lesbar.
- **Einmal pro Aktion und Gerät** (`localStorage`, Schlüssel `rentus_aktion_gesehen`, Wert
  = `von|titel`). Wechselt die Aktion, ändert sich der Schlüsselwert und sie erscheint wieder.
  Wer dreimal am Tag auf die Seite geht, sieht sie einmal.
- **Daten getrennt von Logik.** Wenn für den Wechsel HTML angefasst werden muss, macht es
  nach zwei Monaten keiner mehr. Deshalb eine Datei mit Kommentar-Anleitung obendrauf.
- **Texte per `textContent`, nicht `innerHTML`** — was in `aktionen.js` steht, kann kein
  Markup in die Seite tragen.
- **Preis 99,90 € unverändert** aus der bestehenden Fauler-Hund-Sektion übernommen.
  Fauler-Hund-Konditionen sind Tabu ohne Andreas-Go (CLAUDE.md).

## Der Fehler, der beim Testen auffiel

Die Einblendung hing zuerst an `requestAnimationFrame`. **Das ruht in Hintergrund-Tabs.**
Der Zeitgeber, der die Aktion einblendet, läuft dort aber weiter und setzte den
Gesehen-Merker — die Aktion wäre also als „gesehen" abgehakt worden, ohne je sichtbar
gewesen zu sein. Wer die Seite per „In neuem Tab öffnen" im Hintergrund lädt und den Tab
nie ansieht, hätte die Aktion nie zu Gesicht bekommen.

Jetzt: Einblenden über `setTimeout`, und `zeigen()` wartet aktiv auf `visibilitychange`,
solange `document.visibilityState === 'hidden'`. Gemerkt wird erst, wenn wirklich gezeigt wird.

## Prüfungen

- **Verhaltenstest** über eine Wegwerf-Seite auf demselben Origin (iframe + geteilter
  `localStorage`), drei Fälle einzeln — alle bestanden:
  frischer Besucher → *erscheint und merkt sich* · Wiederkehrer → *bleibt still* ·
  `?aktion=test` → *erscheint trotzdem*. Dazu ESC-Schließen und das Abräumen der
  `body`-Klasse. Testdatei danach gelöscht.
- **Sektionszahl 14 und ID-Liste 32 unverändert** gegen den Ausgangsstand (Hausregel).
  Der komplette `index.html`-Diff sind 2 Zeilen Cache-Bump + 3 Zeilen Skript-Einbindung.
- Ansichten geprüft bei 1280×820, 500×860 und 1280×600 (flaches Laptop-Fenster).
- Vor dem Spiegeln per `md5` geprüft: Glanzgarage und deadrabbit waren **byte-identisch**
  mit dem alten Stand → nichts überschrieben.
- Live per `curl`: `aktionen.js` = 200, `aktionen.js?v=38` im ausgelieferten HTML.
  GitHub Pages brauchte gut eine Minute.

### ⚠️ Verfahrensfehler dieser Sitzung — festgehalten, damit er sich nicht wiederholt

**Gebaut wurde zuerst direkt im Deploy-Spiegel `info-rentus`, nicht in `Glanzgarage/site/`.**
Aufgefallen ist es erst beim Schreiben dieser Übergabe, durch das README im Spiegel-Repo.
Nachgezogen: Quelle und zweiter Spiegel sind jetzt wieder byte-identisch mit dem Live-Stand.

**Ursache:** Die Suche nach „rentus" führt zu vier plausiblen Ordnern
(`info-rentus`, `deadrabbit-landing/rentus`, `Documents/retus-werkstadt/rentus-landing`,
`Downloads/rentus…`). Der Live-Spiegel sieht dabei am meisten nach „der Homepage" aus.
**Konsequenz für die nächste Sitzung:** bei allem, was RENT US / Glanzgarage betrifft, zuerst
`~/XcodeProjects/Glanzgarage/CLAUDE.md` lesen — dort steht die Repo-Aufteilung. Nie im
Spiegel bauen.

## Offen

- 🟡 **Foto.** Läuft mit `dog.jpg` (Hund im roten Oldtimer) aus dem Repo-Bestand. Hat Mike ein
  **echtes** Foto vom Faulen Hund, gehört das hier hin — Hausregel „echte Fotos". Tausch ist
  ein Pfad in `aktionen.js` plus passender `bildFokus`.
- 🟡 **Nächste Aktion ab 01.11.2026.** Der Faule Hund läuft bis 31.10. und hört dann von selbst
  auf. Was danach kommt, ist mit Mike zu klären — sonst zeigt die Seite ab November nichts
  (was kein Fehler ist, aber die Bühne verschenkt).
- 🟢 **Karteileiche `.wa-float`.** Das CSS für den schwebenden WhatsApp-Knopf steht noch in
  `style.css`, im HTML gibt es **kein** Element dazu. Die Bauchbinde würde ihn am Handy
  zudecken, deshalb liegt eine Schutzregel bereit (`body.aktion-offen .wa-float`), die ihn
  hochschiebt, falls er zurückkommt. Beschriftet, damit sie nicht rätselhaft wirkt.
- 🟢 **Bekannte Grenze der Testmethode:** headless Chrome lädt unter
  `--virtual-time-budget` **kein zweites iframe** nach (`onload` feuert nicht). Mehrstufige
  Abläufe deshalb als getrennte Läufe testen, Zustand vorher über `localStorage` setzen —
  nicht versuchen, alles in einen Lauf zu packen. (Ergänzung zur 500px-Falle vom 31.07.)
