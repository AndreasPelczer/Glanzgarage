# Übergabe 2026-07-13 — Codi — Check-Fusion stabil (v=19)

## Was jetzt LIVE ist (pelczer.de/rentus, Cache v=19 / iframe cb=19)
Ein Fluss in der Buchung, **am Handy bestätigt** (beide Listen + Innenraum-Bild kommen an):
1. Fahrzeug wählen (6 Typen).
2. Zustand-Panel (optional, überspringbar), zwei Tabs:
   - **Außen = 3D-Check** (`tools/3d-check/index.html`): drehen, Kratzer setzen → übernimmt **NUR die Liste**
     (`localStorage rentusAutoCheck`). Baut **kein Bild** mehr → sendet nur `postMessage({rentusCheck:'done'})`.
   - **Innen = 2D-Check** (`tools/3d-check/innen.html`): Flecken tippen → übernimmt **Liste + Report-Bild**
     (`rentusInnenCheck`, `postMessage({rentusCheck:'innen', img})`).
3. Ein Sende-Knopf → WhatsApp: **Text mit beiden Listen** + **ein** Report-Bild (= Innenraum).
   Desktop = WhatsApp Web, Mobil = `navigator.share` / wa.me.

## Warum so (wichtig fürs nächste Mal)
Das 3D-**Bild** (WebGL→Canvas, auch mit `toDataURL`-Fix) kam auf iOS **nicht zuverlässig** durch.
Zwischenschritt „Außen komplett 2D" (`aussen.html`) lief, aber Andreas wollte die 3D-Auswahl behalten.
**Endentscheidung Andreas:** stabile Sache — 3D behalten, aber nur Liste; ein Bild reicht (Innen).

## Dateien
- `tools/3d-check/index.html` — 3D, Embed-Booking-Handler sendet nur noch die Liste (Zeile ~442).
- `tools/3d-check/innen.html` — 2D Innenraum, Liste + Bild.
- `tools/3d-check/aussen.html` — 2D Außen (Auto von oben, Canvas-gezeichnet). **Liegt bereit, nicht eingebunden.**
- `site/assets/js/main.js` `showZustand()` — Außen-iframe → `/3d-check/?embed=1&cb=19&typ=…`, Innen → `innen.html`.
  Message-Listener (Zeile ~368): `done`→checkImgAussen (bleibt jetzt null), `innen`→checkImgInnen.
  `combineReports()` stapelt vorhandene Bilder (aktuell nur Innen) zu einem PNG.

## Offen / als Nächstes
- **7 Meshy-Modelle** (Andreas holt sie, GLB): Loader OBJ→GLTFLoader umstellen, Typ→Modell mappen.
  Da 3D nur noch die Liste liefert, sind Modelle reine Optik/Auswahl — kein Funktionszwang.
- Optionaler Feinschliff: Außen-2D um Seitenansichten ergänzen (Türkratzer-Höhe) — nur falls Mike will.

## Tabus unverändert
Preise / Fauler-Hund / Versiegelungsliste / Launch auf info-rentus.de (BookingPress-Altbuchungen) — ohne Andreas-Go nicht anfassen.
