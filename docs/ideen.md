# Ideen & Roadmap — RENT US Glanzgarage

Sammelstelle für geplante Umbauten und Feature-Ideen. GitHub = Wahrheit.

## ✅ LIVE (13.07.2026): Buchung + Außen-Check + Innenraum-Check verschmolzen
Fahrzeug wählen → optional Außen + Innen einzeichnen → EIN Sende-Knopf → WhatsApp bekommt
Text (beide Listen) + EIN kombiniertes Report-Bild (Außen+Innen gestapelt; iOS/WhatsApp nimmt
nur 1 Datei). Desktop = WhatsApp Web (kein App-Zwang).
Aufgeräumt: Kontaktformular, Schwebe-Button, Panel-eigene WhatsApp-Knöpfe, obere 3D-Sektion — alles raus.

### ⚠️ Wende 13.07. nachmittags: Außen von 3D auf 2D umgestellt
**Grund:** Das 3D-Report-Bild (WebGL→Canvas) kam auf dem iPhone **nicht durch** — nur der Text
und das Innen-Bild landeten in WhatsApp. Auch der `toDataURL`-Fix half nicht zuverlässig.
**Lösung (Andreas' Idee):** Außen-Check jetzt als **2D-Canvas-Zeichnung** `tools/3d-check/aussen.html`
(Auto von oben, direkt auf Canvas gemalt — kein Bild-Asset, kein WebGL, kein SVG). Gleiche
bombensichere Technik wie der Innenraum-Check. Buchung hört unverändert auf `rentusCheck:'done'`
+ liest `rentusAutoCheck`; in `main.js`/`showZustand` zeigt der Außen-iframe jetzt auf `aussen.html`.
`tools/3d-check/index.html` (3D) bleibt als Datei liegen, wird aber nicht mehr eingebunden.
Cache-Stand **v=18 / cb=18**.

**Folge für die Meshy-Modelle:** Für den *Check* braucht es jetzt **keine 3D-Modelle** mehr.
Die 7 realistischen Modelle wären nur noch Deko — ODER man rendert sie einmal zu Draufsicht-Bildern
und legt sie hinter `drawCar()` als optischen Ersatz für die schematische Zeichnung. Offen mit Andreas.
Nächster möglicher Feinschliff: Außen-Skizze evtl. um Seitenansichten ergänzen (Türkratzer-Höhe).

## 🔨 (erledigt) Buchung + 3D-Check verschmelzen ("Ein Fluss")
Entscheidung Andreas (11.07.): **beides anbieten** (Einstieg oben UND als Schritt in der Buchung, gleiche Daten dahinter) + Zustand-Schritt **optional/überspringbar**.

**Problem heute:** Auto wird 2× gewählt (3D-Check-Modell ≠ Buchungs-Typ), 3D-Check ist separater Block, 4 verschiedene WhatsApp-Buttons.

**Zielfluss:**
```
1. Fahrzeug wählen (die 6 Buchungs-Typen, EINE Liste)
2. Zustand einzeichnen (optional) — gewähltes Auto in 3D, Kratzer/Dellen markieren [Überspringen]
3. Leistung / Paket
4. Abholung & Termin
5. Anfrage → EIN WhatsApp-Button (alles) + "Bild + Liste teilen" (mobil)
```
**Stufen:** (1) eine Auto-Liste + Typ→Modell-Mapping, (2) 3D als optionaler Wizard-Schritt (iframe bekommt `?typ=`, gibt Schadensliste zurück via localStorage `rentusAutoCheck`), (3) Oben-Teaser → Buchung mit vorgewähltem Auto, (4) WhatsApp-Buttons auf einen reduzieren.
**Bauen auf Branch `feature/buchung-3d-fusion`, kein Deploy bis Abnahme** (Demo bleibt sicher).

**Offen (Q2, später):** Typen ohne 3D-Modell (Kleinwagen/Kombi/Bus bis Modell da) → Schritt überspringen ODER generisches Auto zeigen?

## 💡 Idee: INNENRAUM-Check (neu, 11.07.)
Gleiche Mechanik wie der Außen-Check, aber **Innenraum von oben** (Vorlage: `Innenraumansicht 2.jpg`): Kunde **markiert Flecken/Verschmutzungen** auf Sitzen/Teppich → Mike weiß genau, wo er aufpassen muss → **Report-Bild an die WhatsApp-Nachricht**. „BAM."
- Umsetzung analog: markierbares Bild (2D reicht, wie die Skizze) + Notiz je Fleck + Report-PNG mit Liste.
- Passt als **zweiter Tab/Schritt** neben „Außen": „Außen einzeichnen" | „Innen einzeichnen".

## 📦 3D-Modelle (Status offen)
Andreas hat neue Modelle, **Format/Größe noch unklar**. Aktueller Loader will **OBJ+MTL** (`models/<slot>/car.obj`+`car.mtl`). Falls GLB → konvertieren oder Loader auf GLTFLoader umstellen. Slots: `auto/auto2/suv/sport/taxi` (+ geplant `bus`). Zuordnung Buchungs-Typ → Modell noch festzulegen.
