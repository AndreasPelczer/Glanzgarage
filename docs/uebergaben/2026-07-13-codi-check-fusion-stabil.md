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

## Session-Abschluss 13.07. — von Andreas abgenommen
- **Alle 3D-Modelle final ok** (Andreas: „Größe und Stand und Position auch ok"). Einzel-Kalibrierung
  NICHT nötig → nicht gebaut. Falls doch mal eins schief wirkt: in `loadCar` (`tools/3d-check/index.html`)
  einen `MODEL_FIX`-Table (rotY/rotX/scaleMul/yOffset je Modell-Key) einziehen und dort nachziehen.
- **Schritt-Sprung** in der Buchung von Andreas bestätigt („sitzt").
- **Sonderleistungen** von Andreas bestätigt („kommen mit").

## Offen / als Nächstes (nichts akut)
- Optionaler Feinschliff: Außen-2D (`aussen.html`, liegt bereit) um Seitenansichten ergänzen — nur falls Mike will.
- Sonderleistungs-**Preise** stehen als ab-Werte im Text; falls Mike SUV-Zuschlag auch auf Extras will → klären
  (aktuell KEIN Faktor auf Sonderleistungen).
- **Tabu bleibt:** Preise/Fauler-Hund/Versiegelungsliste/Launch auf info-rentus.de nur mit Andreas-Go.

**Endstand Session: v=24 / cb=21 live auf pelczer.de/rentus. Beide Repos gepusht (Glanzgarage + deadrabbit).**

## ⚠️ BETRIEBS-LEHRE 13.07. spät — Glanzgarage IMMER mitpushen
**Vorfall:** Über den Tag wurde nur **deadrabbit** (Deploy) gepusht, **Glanzgarage** (die „Wahrheit")
nur lokal committet → das Glanzgarage-**Remote auf GitHub hing 20 Commits zurück (~v16-Ära)**, obwohl
lokal alles auf v=24 war. Eine QA-Warnung deutete das (falsch) als „8 Versionen nur im Deploy gebaut".
**Real:** lokal war korrekt & voraus, nur nie gepusht.
- **Regel (Hausregel schärfen):** Nach JEDEM Commit **beide** Repos pushen — Glanzgarage UND deadrabbit.
  Check: `git -C ~/XcodeProjects/Glanzgarage status -sb` darf nie „ahead" stehen bleiben.
- **Falle bei der Reparatur:** NIE „deadrabbit → Glanzgarage zurücksyncen"! Die Wahrheit ist lokal Glanzgarage;
  sie ist absichtlich **voraus** (z.B. noch nicht deployte Mike-Änderungen). Fix ist immer **`git push` der
  Quelle**, nie ein Rücksync vom Deploy — sonst löscht man die neueste Arbeit.
- Am 13.07. gelöst: Glanzgarage lokal (e52dbd8) → origin gepusht, lokal==Remote.

## 🧊 FREEZE ab 13.07. abends — Kundenreview läuft
**Mike prüft die Seite die nächsten ~2 Tage (bis ~15.07.), Frau + weitere haben Mitspracherecht.
Ihm gefällt es schon sehr gut.** → **KEINE Live-Deploys während des Reviews**, außer echte Bugfixes,
die Andreas freigibt. Nichts an Inhalt/Struktur ohne sein Go.

### ✅ Ausnahme: 2 sichere Textfixes von Andreas freigegeben & LIVE (13.07.)
Aus dem Falbe-Review (`docs/falbe-review-2026-07-13.md`), reine Texte, kein Preis/Struktur:
- **#3** FAQ: „über das Kontaktformular" → **„über die Online-Anfrage"** (Formular gibt's nicht mehr).
- **#8** Absolutaussagen entschärft: „Schimmelsporen vollständig" → **„nahezu vollständig"**;
  „keine Gerüche zurückbleiben" → **„In aller Regel bleiben keine…"**; „Zufriedenheit garantiert"
  → **„Deine Zufriedenheit ist unser Anspruch"**. **StVZO bewusst UNANGETASTET (braucht Mike).**
- Deploy: Glanzgarage-Commit + deadrabbit `1794282`. **Achtung Betrieb:** GitHub-Pages-Build hing ~23 Min
  auf „building"; leerer Commit `6c925d9` als Nudge hat's gelöst → live um 15:45. (Merke: Pages kann
  hängen; `git commit --allow-empty` + Push stößt neu an.) HTML-only, KEIN `?v=`-Bump nötig. Stand bleibt v=24.

### Wartbarkeits-Refactor: GEPLANT, bewusst PAUSIERT (nicht anfangen!)
Andreas will die Seite wartbarer (MVVM-Geist, kein Framework). Gewählter Weg: **Vanilla-Refactor**.
Bewusst **verschoben bis nach Mikes Feedback** — weil sein Feedback Inhalte/Preise/Struktur umwerfen
kann und ein Refactor jetzt (a) reines Bruch-Risiko während des Reviews wäre, (b) doppelte Arbeit.
Erst Feedback einarbeiten, DANN gegen die finalen Inhalte refactoren.

**Refactor-Plan (Stufen, wenn grün):**
- **A** `config.js` als eine Datenquelle: WA-Nummer (steht aktuell **29×** hartcodiert!), Mail, Tel.
  Shared unter `/rentus/assets/js/config.js`, von index.html UND den 3 Check-Tools per absolutem Pfad geladen.
- **B** Die 3 Check-Tools (index/innen/aussen) zu **einem** `check-core.js` entdoppeln
  (`render`/`updateSend`/`damageLines` in allen 3, `makeReport`/`buildText` in 2). Größter Wartungs-Gewinn.
- **C** Pakete/Sonderleistungen/Preise aus HTML-`data-*` → in `config.js`, Wizard-Grids daraus rendern.
- **D** `?v=`-Cache-Bump automatisieren (aktuell Handarbeit, Stand v=24).

## Tabus unverändert
Preise / Fauler-Hund / Versiegelungsliste / Launch auf info-rentus.de (BookingPress-Altbuchungen) — ohne Andreas-Go nicht anfassen.
