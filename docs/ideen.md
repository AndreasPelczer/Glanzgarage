# Ideen & Roadmap — RENT US Glanzgarage

Sammelstelle für geplante Umbauten und Feature-Ideen. GitHub = Wahrheit.

## ✅ LIVE (13.07.2026): Buchung + 3D-Check + Innenraum-Check verschmolzen
Am Handy verifiziert: Fahrzeug wählen → optional Außen (3D) + Innen (2D) einzeichnen →
EIN Sende-Knopf → WhatsApp bekommt Text (beide Listen) + EIN kombiniertes Report-Bild
(Außen+Innen gestapelt; iOS/WhatsApp nimmt nur 1 Datei). Desktop = WhatsApp Web (kein App-Zwang).
Aufgeräumt: Kontaktformular, Schwebe-Button, Panel-eigene WhatsApp-Knöpfe, obere 3D-Sektion — alles raus.
Cache-Stand v=16. Offen: 7 realistische 3D-Modelle (Andreas via Meshy → GLB → dann Loader auf GLTFLoader umstellen).

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
