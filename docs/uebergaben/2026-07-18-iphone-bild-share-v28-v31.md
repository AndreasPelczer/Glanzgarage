# Übergabe 2026-07-18 — iPhone-Bildversand im Buchungs-Check (v=28 → v=31)

**Status: LIVE, aber noch nicht final abgenommen.** Mike/Andreas-Rückmeldung am Handy:
„klappt und klappt nicht" — **was genau noch fehlt, ist offen** (morgen klären, siehe unten).

## Worum es geht
Im Buchungs-Wizard gibt es den Zustand-Check (Außen + Innen). Beim „Senden" soll die
Anfrage per WhatsApp zu Mike — **inklusive Bild(er)** vom Check. Auf dem **iPhone** kam das Bild
nicht / nur teilweise an. Diese Sitzung hat das in mehreren Schritten umgebaut.

## Was heute passiert ist (chronologisch, alles gepusht & live)
1. **v=29** — 3D-Auto-Check als Außen-Ansicht **wiederhergestellt** (war auf 2D `aussen.html`
   umgestellt; Mike/Kundenwunsch = 3D). Report-Bild im Embed reaktiviert (Regression aus
   Commit `e593d8b` behoben). *Ergebnis am iPhone: nur Text, kein Bild.*
2. **v=30** — **iOS-Text+Bild-Problem** gelöst: iPhone-WhatsApp verwirft beim Teilen von
   Text **und** Bild zusammen das Bild. Fix: Buchungsdaten als **Deckblatt-Bild** rendern
   (`buildCoverImage`), Deckblatt + Außen + Innen zu **EINEM** Bild stapeln (`stackBlobs`),
   und per `navigator.share({ files })` **ohne text-Feld** teilen. *(Andreas-Entscheidung:
   „Bild-Weg" — Bild kommt sicher, dafür wählt der Kunde Mike selbst im Teilen-Menü, Nummer
   nicht mehr vorausgefüllt.)*
3. **v=31** — **3D-WebGL-Screenshot geht auf iPhone nicht** (genau der alte `e593d8b`-Grund).
   Fix: Der Außen-Report wird jetzt als **2D-Draufsicht** gezeichnet — die 3D-Marker-Positionen
   (`m.p`, Welt-Koordinaten) werden über die Fahrzeug-Bounding-Box (`carBox`) in eine
   schematische Auto-Draufsicht projiziert (nummerierte Punkte + Schadensliste). **Reines 2D →
   iOS-sicher.** Der **3D-Check bleibt die Bildschirm-Ansicht** (Mikes Wunsch).

## Architektur jetzt (Stand v=31)
- **Bildschirm/Ansicht:** Außen = 3D-Check (`/3d-check/?embed=1&cb=31&typ=…`), Innen = 2D
  (`/3d-check/innen.html?embed=1&cb=31`).
- **Bild für WhatsApp:** beide Checks liefern ein **2D-Canvas-Bild** (Außen = Draufsicht,
  Innen = 2D) → in `main.js` per postMessage aufgenommen (`checkImgAussen`/`checkImgInnen`).
- **Senden (Handy, wenn Check-Bild da):** `buildCoverImage(baueText())` → `stackBlobs([cover,
  außen, innen])` → `navigator.share({ files })` (ohne text). Sonst/Desktop: wa.me-Textlink
  (Mikes Nummer vorausgefüllt; Text enthält auch die Schadensliste).

## Geänderte Dateien (Quelle Glanzgarage → Deploy deadrabbit gespiegelt)
- `site/assets/js/main.js` → `rentus/assets/js/main.js`
  (`buildCoverImage`, `stackBlobs`, Senden-Handler files-only, `cb=31`)
- `tools/3d-check/index.html` → `3d-check/index.html`
  (`buildReportImage` = 2D-Draufsicht statt WebGL-Screenshot)
- `site/index.html` → `rentus/index.html` (`?v=31`)

Commits: Glanzgarage `2b21e76` / deadrabbit `a6cc953` (jeweils Spitze). Beide gepusht, live
per curl belegt (v=31, Deckblatt, files-only, cb=31, Draufsicht, kein WebGL-toDataURL mehr).

## OFFEN — morgen zuerst klären
- **Was genau „klappt nicht"?** Nicht spezifiziert. Mögliche Kandidaten am iPhone:
  - Kommen jetzt **beide** Bild-Teile (Deckblatt + Außen-Draufsicht + Innen) in EINEM Bild an,
    oder fehlt weiter etwas?
  - **Marker-Positionen** in der Draufsicht plausibel? (Projektion nutzt die längere
    xz-Achse als Fahrzeuglänge; Front/Heck ist schematisch, nicht orientiert.)
  - **Empfänger-Reibung:** Kunde muss Mike im Teilen-Menü selbst wählen (Nummer nicht
    vorausgefüllt) — von Andreas so entschieden, aber im Alltag beobachten.
- Sinnvoller nächster Schritt: **konkret reproduzieren** (welcher Schritt, welches Bild fehlt/
  ist falsch), erst dann fixen.

## Nicht vergessen (aus früheren Übergaben, weiter offen)
- Preise/Versiegelung/Fauler-Hund-Konditionen = Tabu ohne Mike-Go (Fauler Hund läuft aktuell
  als Coming-Soon-Teaser, ohne Preis).
- Mittwochs-Assets (Fotos, Logo, Kinowerbung-PDF) einbauen.
- Detailtexte aus alter Seite (`/upgrade-lackversieglung/`, `/sonderleistungen/`) übernehmen.
- Echte Google-Bewertungen + Link. Hoster IONOS/Strato final klären.
