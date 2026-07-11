# Übergabe 2026-07-11 — Rentus v5 / Welle 3 (REKONSTRUKTION)

**Status:** Die ursprüngliche Übergabe dieses Namens ging **uncommittet verloren**
(vorige Sitzung am Kontext-Limit abgebrochen, Datei nie committet). Neu angelegt
im Zuge der Repo-Aufräum-Aktion — daher knapp, ohne die Detailhistorie der
verlorenen Fassung.

## Aufräum-Aktion (Kontext, 11.07.)
Frische Sitzung nach Mac-Neustart fand lokal **keine brauchbaren Repos**:
- `~/Documents/*` ist **TCC-gesperrt** (kein Terminal-Zugriff).
- INTENSO-`deadrabbit-landing` war ein **leerer Mount-Geist**.
- `~/XcodeProjects/deadrabbit-landing` war eine **git-lose Kopie**.

**Festlegung Andreas:** GitHub = einzige Wahrheit; kanonische Heimat **`~/XcodeProjects/`**.
Umgesetzt: beide Repos frisch von GitHub geklont; git-lose Kopie nach
`~/Alt-Ablage/2026-07-12/` verschoben (nicht gelöscht); `~/Documents/Glanzgarage`-Geist
unangetastet (für mich unlesbar → Andreas räumt im Finder); `CLAUDE.md` neu angelegt.

## Stand v5 (✅ ERLEDIGT — LIVE)
- `rentus-final-v5.zip` (Falbe-Paket, gegen aktuellen Stand verifiziert) in **beide**
  Repos eingespielt: `rentus/*`+`3d-check/` (deadrabbit) und `site/*`+`tools/3d-check/`
  (Glanzgarage). Betroffen: index.html, style.css, main.js, 3d-check/index.html.
- **Cache-Bump v=4 → v=5.** Hausregel-Diff vor Einbau: Sektionen 15→15, IDs 33→33,
  **keine verloren.**
- Commits: Glanzgarage `76bf46e`, deadrabbit-landing `eae2840` (beide `main`, gepusht).
- **Live-Abnahme bestanden** (`curl https://pelczer.de/rentus/`, ~10 s nach Push):
  `v=5` = **2**, `details class="mehr"` = **19**. ✓

## Vorhandener Stand (aus den anderen Übergaben)
Welle 1 (echte Fotos), Welle 2 (Buchungs-Wizard + WhatsApp, SUV/Bus/Van +20 %),
Welle 2.1 (3D-Check eingebettet in `/rentus/`, Report-PNG mit Mängelliste, Share-API).
Welle 3 geplant: besseres/leichteres/markensicheres 3D-Modell + Kamerafahrt.

## Offene Aufgaben 2–5 (aus CLAUDE.md/Einweisung)
Formspree-Platzhalter (mit Andreas klären) · Google-Bewertungslink (Mikes Profil-URL
bei Andreas erfragen) · 3D-Kalibrierung (`FRONT_POSITIV`/`LINKS_POSITIV`, Andreas testet
Motorhaube→„vorne") · Handy-Test „Bild + Liste teilen" dokumentieren.
**Tabu ohne Go:** Preise, Fauler-Hund-Konditionen, Launch auf info-rentus.de.
