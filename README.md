# Glanzgarage — Website & Buchung für RENT US Glanzgarage

Kunde: **Mike Knörzer**, RENT US Glanzgarage, Am Karussell 4, 97280 Remlingen
Auftragnehmer: Andreas Pelczer (B.I.N.D.A. Verlag) · Crew: Andreas, Falbe, Codi

## Struktur

- `site/` — die Website (live unter https://pelczer.de/rentus/, Launch-Domain offen)
- `docs/` — Bildplan, Entscheidungen, offene Punkte
- `docs/uebergaben/` — datierte Übergaben (Falbe-Statik-Berichte)

## Stand & Wellen

- **Welle 1 (11.07.2026):** Stock-Fotos komplett ersetzt durch echte Arbeit
  (Kennzeichen verpixelt), 2 echte Vorher/Nachher-Regler, Garagen-Grün #9fb247.
- **Welle 2 v1 (11.07.2026, vorbereitet):** Buchungs-Wizard ist mit
  Fahrzeugtyp → Paket → Abholung → Wunschtermin → WhatsApp-Anfrage gebaut.
  SUV/Bus/Van +20 % wird im Wizard und WhatsApp-Text gerechnet.
- **Welle 2.1 (11.07.2026, vorbereitet):** 3D-Check schreibt eine strukturierte
  Mängelliste in die Buchung und bietet mobil "Bild + Liste teilen" über die
  native Share-API an. Siehe `docs/uebergaben/2026-07-11-autocheck-whatsapp.md`.
- **Welle 3 (geplant):** besseres/komprimiertes 3D-Modell + Kamerafahrt.
- **Geparkt:** "Fauler Hund" Preis/Turnus bei Mike final klären; technisch wird
  aktuell 60–80 € bzw. 72–96 € bei SUV/Bus/Van gerechnet.

## Regeln

- Echte Fotos, keine Stock-Behauptungen. Kennzeichen immer verpixeln.
- EXIF/GPS vor Upload strippen.
- Muster vom Mitbewerber lernen ist ok — Grafiken/Code nie kopieren.
- Deployment: Strato, macht Andreas (Zugangsdaten fasst die KI nicht an).
