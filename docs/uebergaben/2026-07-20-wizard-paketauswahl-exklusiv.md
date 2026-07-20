# Übergabe 2026-07-20 — Wizard: Paketauswahl exklusiv (Bug-Report Mike 19.07.)

> **Auftrag:** `docs/`-Memo „Codi-Auftrag: Wizard — Paketauswahl exklusiv machen" (20.07.).
> **Quelle:** Voicemail Mike Knörzer 19.07. 18:46 — im Wizard-Schritt „Leistung" ließen sich
> scheinbar mehrere Aufbereitungspakete gleichzeitig markieren.
> **Priorität:** vor Endabnahme / Go-live Ende der Woche.

## Endstand
- **LIVE** (curl-belegt, ~15 s nach Push): `pelczer.de/rentus/` = **`?v=32`**,
  `main.js` enthält Toggle-Code, `style.css` enthält `@media (hover: hover)`.
- **Beide Repos gepusht & synchron:**
  - Glanzgarage (Quelle) `9641431` == origin
  - deadrabbit-landing (Deploy) `98c7b94` == origin
- **v=31 → v=32.**

## Diagnose — der Report war halb richtig
Die **JS-Radio-Logik war seit dem Ur-Wizard korrekt** (`beaf369`): `state.paket` hielt
immer nur *ein* Paket, `remove('sel')` von allen vor `add('sel')`. Mehrfachauswahl war im
State **nie** möglich.

**Der echte Grund für Mikes „drei Pakete markiert" war CSS:** `.wcard:hover` (grüner Rahmen,
fast identisch zu `.wcard.sel`) **klebt auf Touch-Geräten** nach dem Antippen — der vorher
getippte Karton behält den Hover-Rahmen und *sieht* ausgewählt aus.
→ **Nur-JS (wie im Memo gedacht) hätte Testfall 7 (echtes Handy) nicht behoben.** Deshalb CSS **und** JS.

## Geändert (3 Dateien)
**`assets/css/style.css`**
- `.wcard:hover` (+ `:hover svg`) jetzt in `@media (hover: hover) and (pointer: fine)` gekapselt
  → auf Touch kein klebender Rahmen mehr.
- `.wcard.sel` deutlicher: getönter Hintergrund `rgba(159,178,71,.14)` + kräftigerer Ring
  (`.4` statt `.25`) → unverwechselbar vs. Hover.
- Neu: `.wbtn-off { opacity:.45; pointer-events:none; }` (gesperrter Weiter-Button).

**`assets/js/main.js`**
- Paket-Handler: **Toggle** — Re-Klick aufs aktive Paket wählt ab (`state.paket=null`, `preis=0`).
  Radio-Verhalten bleibt.
- Neu `updateNext()`: **Weiter-Button gesperrt in Schritt 2, solange kein Paket gewählt** ist
  (gesetzt in `show()` + bei jedem Paket-Klick). `next`-Click zusätzlich hart geguarded.
- Sonderleistungen **unangetastet** (Mehrfachauswahl bleibt).

**`index.html`** — nur `?v=31`→`?v=32` an main.js/style.css.

## Testfälle
| # | Test | Status |
|---|---|---|
| 1 | Paket A→B | ✅ nur B aktiv (Logik-Sim) |
| 2 | aktives Paket erneut | ✅ nichts aktiv, Weiter gesperrt |
| 4 | Paket + 3 Sonder | ✅ Sonder unverändert |
| 5 | SUV-Wechsel ×1,2 (359→431) | ✅ |
| 6 | Durchlauf bis WhatsApp | ✅ Text-Logik unverändert korrekt |
| 7 | **echtes Handy** | 🟡 **offen — Andreas testet** (Browser-Extension der KI war aus) |

## Offen / bewusst nicht gemacht
- **Fauler Hund (Memo-Punkt 3):** ist **kein buchbares Wizard-Paket**, nur eine
  „Vormerken lassen"-Card (index.html ~Z.292). Es gibt nichts in die Exklusiv-Gruppe zu stecken.
  **Andreas-Entscheid 20.07.: „lass den Hund wie er ist (für den Moment)".** Nichts erfunden.
- **Testfall 7** vom echten Handy noch zu bestätigen → dann Endabnahme durch.

## Backups
`site/assets/js/main.js.backup_20260720_140030` · `…/css/style.css.backup_20260720_140030`

## Unverändert offen (aus Übergabe 18.07.)
Hoster IONOS vs. Strato · Google-Bewertungslink (`#`) · Detailtext-Übernahme alte Seite.
Tabus unverändert (Preise / Fauler Hund / Versiegelungsliste / Launch info-rentus.de).
