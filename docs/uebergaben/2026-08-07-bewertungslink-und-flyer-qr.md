# Übergabe 2026-08-07 — Echter Google-Bewertungslink + QR-Codes für Mikes Flyer

> **Auslöser:** Andreas brauchte zwei QR-Codes für einen Flyer. Dabei kam der echte
> Bewertungslink von Mike — und hat den offenen Punkt vom 31.07. geschlossen.

## Der Link
```
https://g.page/r/CfEteytVLOowEAE/review
```
Aufgelöst per `curl` (Weiterleitung mitverfolgt): landet auf
`search.google.com/local/writereview?placeid=ChIJx0ITece9okcR8S17K1Us6jA`.
Ohne Login schiebt Google eine Anmeldeseite dazwischen — auf dem Handy ist der Kunde
in der Regel eingeloggt und steht direkt im Bewertungsformular.

## Website — Button richtiggestellt
`site/index.html`, Sektion `#bewertungen`, Button „Jetzt bei Google bewerten":

| | vorher | nachher |
|---|---|---|
| Ziel | `google.com/maps/search/?api=1&query=Rent Us Glanzgarage…` | `g.page/r/CfEteytVLOowEAE/review` |
| Kundenweg | Profil öffnen → „Rezension schreiben" suchen | direkt ins Formular |

Damit ist die **Übergangslösung vom 31.07. abgelöst** (siehe
`2026-07-31-echte-google-bewertungen-live.md`, Abschnitt „Offen").

⚠️ **Nicht verwechseln:** Die Seite hat einen *zweiten* Maps-Link — den Routen-Button
zur Werkstatt (`maps/dir/?api=1&destination=Am Karussell 4…`). Der ist richtig und
wurde **nicht** angefasst.

## Prüfungen (Hausregel nach HTML-Patch)
- Sektionen **14 → 14**, IDs **33 → 33**, `diff` der ID-Listen **leer**.
- Alle 5 echten Rezensenten (Fabio, Chris S., Ashley knoerzer, Mattis Hiske, Gina) noch da.
- `git diff`: 5 Zeilen rein, 4 raus — nur der eine `href` plus Kommentar.
- Vor dem Spiegeln geprüft: beide Zieldateien waren **exakt** der alte Quellstand → nichts überschrieben.
- Kein Cache-Bump nötig — reine HTML-Änderung, kein JS/CSS angefasst.

## Spiegel
| Repo | Datei |
|---|---|
| Glanzgarage (Quelle) | `site/index.html` |
| info-rentus (info-rentus.de) | `index.html` (Wurzel) |
| deadrabbit-landing (pelczer.de) | `rentus/index.html` |

Alle drei tragen denselben `href` — verifiziert.

## QR-Codes für den Flyer
Liegen auf dem Schreibtisch: **`~/Desktop/QR-Mike/`** (nicht im Repo — Flyer-Material,
keine Website-Quelle).

| Datei | Ziel |
|---|---|
| `rentus-website.png` / `.svg` | `https://info-rentus.de` |
| `rentus-google-bewertung.png` / `.svg` | `https://g.page/r/CfEteytVLOowEAE/review` |

- Erzeugt mit **segno** (Python, in einem Wegwerf-venv im Scratchpad — nichts systemweit installiert),
  Fehlerkorrektur `M`, 4 Module Rand, ~1220 px Kantenlänge.
- **Beide zurückgescannt** statt nur behauptet: kleines Swift-Skript mit `CIDetector`
  (CoreImage, macOS-Bordmittel) — decodieren exakt die richtigen Adressen.
- Fürs Layout die **SVG** nehmen; das PNG trägt im Druck nur bis ca. 10 cm Kante.
- Der kurze `g.page`-Link ergibt einen **grobkörnigeren** Code als die alte lange Maps-URL —
  scannt aus mehr Abstand und verträgt kleineren Druck.

## Live — vollzogen 07.08.
Andreas' „push live" um 16:2x. Alle drei Repos gepusht:
`Glanzgarage 402ce78` · `info-rentus f2b1235` · `deadrabbit-landing d179d40`.

Per `curl` belegt (GitHub Pages brauchte ~30 s):
- `info-rentus.de` → `g.page/r/CfEteytVLOowEAE/review` ✅
- `pelczer.de/rentus/` → derselbe Link ✅
- Routen-Button (`maps/dir`) auf beiden unverändert ✅
- alle 5 Rezensenten live noch da ✅
- `info-rentus.de/3d-check/` weiterhin **200** (Buchungs-Wizard unbeschädigt) ✅

## Offen
- Unverändert offen: Formspree-Code, Versiegelungs-Preisliste, Fauler-Hund-Konditionen,
  Detailtext-Übernahme aus der alten Seite.

## Merksatz
**Der Bewertungslink steht jetzt an zwei Stellen** — im Website-Button und im Flyer-QR.
Ändert Mike sein Google-Profil, müssen **beide** nach. Der Kommentar im HTML sagt das auch.
