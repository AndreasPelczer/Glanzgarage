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

## NACHTRAG 13.07. nachmittags — weiter gebaut (Stand jetzt **v=24 / cb=21**)
1. **Echte Meshy-Modelle** im 3D-Außen-Check (ersetzen OBJ-Platzhalter):
   - Loader OBJ/MTL → **GLTFLoader + DRACOLoader** (Modelle sind Draco-komprimiert).
   - 8 GLB von Meshy (20–65 MB) mit `gltf-transform optimize --compress draco --texture-compress webp
     --texture-size 1024` auf **1–4 MB** geschrumpft (265 → 16 MB total). Liegen in `tools/3d-check/models-web/`.
   - Roh-GLB (`models-glb/`, 265 MB) bleiben **gitignored**; `models-web/` per `.gitignore`-Ausnahme getrackt.
   - Typ→Modell: Kleinwagen/Limousine/Kombi/SUV/Bus/Coupé (+ Pickup/Oldtimer als Extra-Chips im Standalone).
   - Skalierung `scale=4.0` (25% größer), `.stage`-Hintergrund heller (`#3d4650…`).
   - **Kompressions-Rezept** für weitere Modelle: siehe oben. Node/npx vorhanden.
2. **Sonderleistungen in der Buchung** (Schritt 2, unter den Paketen):
   - 10 Stück als **Mehrfachauswahl** (`[data-sonder]`, toggle `.sel`), optional, `state.sonder[]`.
   - Fließen in `summary()` + `baueText()` (WhatsApp). **Paket-Auto-Sprung entfernt**, damit Extras wählbar bleiben.
   - CSS: `.wcard--s` / `.wgrid--sonder` in style.css.
3. **Schritt-Sprung gefixt:** Weiter/Zurück → `scrollIntoView` auf aktiven `.wstep` + CSS
   `scroll-margin-top:96px` (Header 76px fix), in `requestAnimationFrame`. Sitzt (Andreas bestätigt).

## Offen / als Nächstes
- 3D-Modelle bei Bedarf pro Stück kalibrieren (Ausrichtung/Größe) — Andreas fand Stand ok.
- Optionaler Feinschliff: Außen-2D (`aussen.html`, liegt bereit) um Seitenansichten ergänzen — nur falls Mike will.
- Sonderleistungs-**Preise** stehen als ab-Werte im Text; falls Mike SUV-Zuschlag auch auf Extras will → klären (aktuell KEIN Faktor auf Sonderleistungen).

## Tabus unverändert
Preise / Fauler-Hund / Versiegelungsliste / Launch auf info-rentus.de (BookingPress-Altbuchungen) — ohne Andreas-Go nicht anfassen.
