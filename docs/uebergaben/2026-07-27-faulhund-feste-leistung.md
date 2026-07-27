# Übergabe 2026-07-27 — „Der Faule Hund": von „Abo/Demnächst" zu fester Leistung

> **Quelle:** Codi-Auftrag 26.07. (Voicemail Mike). ⚠️ Das im Auftrag zitierte
> `docs/uebergaben/2026-07-26-mike-voicemail-memo.md` existiert NICHT im Repo — Details kamen
> direkt aus dem Auftragstext.
> **Kern:** 🟠 Vor Mikes Instagram-Launch (Montag) — Faule-Hund-Karte ist jetzt buchbar, kein Abo.

## Ergebnis
- **LIVE + verifiziert** (curl + Chrome, visuell & CTA-Klick): `https://info-rentus.de/`, **v=35**.
- **Alle 3 Repos gepusht:** Glanzgarage `f443b10` · deadrabbit `521c477` · info-rentus `6f0b137`.

## Was geändert wurde (`.faulhund`-Karte in `site/index.html`)
- **Raus:** Badge „Bald neu 🐶", Eyebrow-„Demnächst", Aside „Bald verfügbar / Neues Abo in
  Vorbereitung", Button „Vormerken lassen", der alte abo-artige Pitch (Hol-/Bringservice, Rhythmus).
- **Rein:**
  - Badge **„🐶 Jetzt buchbar"**, Eyebrow „Mikes Renner" (bleibt als Sympathie-Label).
  - Claim (Mikes Wortlaut) **„Der Faule Hund — für kleines Geld."** (neue Klasse `.faulhund__claim`).
  - **Festpreis 99,90 €** prominent (`.faulhund__price`, war im CSS schon vorbereitet).
  - Leistungsliste: Handwäsche außen · Innenraum saugen · Fußmatten reinigen · Einstiege reinigen.
  - Abgrenzung: „Kleine Aufbereitung – keine Vollaufbereitung." (`.faulhund__note`).
  - CTA **„Jetzt anfragen"** (`btn--primary`) → `wa.me/4915901606913` mit vorbefülltem
    „Hallo Mike, ich möchte den ‚Faulen Hund' (99,90 €) buchen." (getestet, öffnet WhatsApp korrekt).
- CSS: `.faulhund__claim` neu (grün, kursiv). Cache v34→v35. Sicherheits-Diff: Sektionen 14/14,
  IDs identisch. Backups `*.backup_20260727_114719_faulhund`.

## 3 Rückfragen — mit Andreas geklärt (27.07., risikoarme Launch-Variante)
1. **SUV/Bus/Van-Aufschlag:** NEIN — echter **Einheits-Festpreis 99,90 €**, kein Aufschlagshinweis.
2. **Wizard-Integration:** NEIN vorerst — **nur Karte + WhatsApp**. Wizard kann Welle N+1.
3. **Turnus (Mike sagt „Samstag"):** **keine Turnus-/Waschtag-Angabe** — soll nicht nach Abo klingen.

## Deploy-Hinweis (wichtig seit 26.07.)
Es gibt jetzt **3 Deploy-Ziele** — bei Seitenänderungen alle spiegeln:
`Glanzgarage/site` (Wahrheit) → `deadrabbit/rentus` (pelczer.de) → `info-rentus` (Wurzel, **echte
Live-Domain info-rentus.de**). Siehe CLAUDE.md „Repos & Workflow".

## ⏸️ Offen / später
- **Wizard-Integration** des Faulen Hund (exklusiv zur Paketgruppe) als eigene Welle.
- **Turnus/Aufschlag** ggf. nachschärfen, falls Mike sich dazu noch äußert.
- Fehlendes Voicemail-Memo im Repo (falls Andreas es noch ablegen will).
- Leasing-Abschnitt (separater Mike-Wunsch, kein Blocker).

## Buch-Bezug
„Zustände statt Bewertungen" — ein klarer Festpreis + benannter Leistungsumfang statt vagem
„demnächst/Abo in Vorbereitung". Der Kunde sieht, was er für 99,90 € bekommt.
