# Übergabe 2026-07-26 — Fake-Bewertungen entfernt (Go-live-Blocker) + Hoster-Entscheidung Strato

> **Quelle:** Endabnahme-Check Andreas/Falbe 26.07. — Codi-Auftrag „Fake-Bewertungen sicher verstecken".
> **Kern:** 🔴 Rechtlicher Go-live-Blocker (§5 UWG) behoben + Launch-Provider auf **Strato** festgelegt.

## Endstand
- **LIVE** (curl + Live-DOM-belegt): `pelczer.de/rentus/` = **v=34**, **null** Fake-Namen.
- **Beide Repos gepusht & synchron:**
  - Glanzgarage (Quelle) `53918f1` == origin
  - deadrabbit-landing (Deploy) `c036d8f` == origin
- Cache **v=33 → v=34** (JS/CSS geändert).

## 🔴 Fake-Bewertungen — behoben (§5 UWG)
Die Sektion `#bewertungen` enthielt **drei frei erfundene Google-Reviews**
(Sabine R. / Thomas K. / Melanie B., je 5★ + Label „Google-Bewertung"). Auf einer
Gewerbeseite = irreführende Werbung, abmahnfähig, Risiko trägt Mike.

**Gewählt: Punkt 3 (bevorzugt) — komplett aus dem Quelltext raus:**
- Die drei `<div class="review">`-Blöcke **entfernt**, HTML-Kommentar statt Text.
  → Auch über „Quelltext anzeigen" nicht mehr auffindbar.
- **Section-Wrapper `#bewertungen` (mit `hidden`) bleibt** für späteres echtes Einsetzen.

**Doppelte Verriegelung zusätzlich:**
- `style.css`: `#bewertungen[hidden] { display: none !important; }` — kein JS kann's aushebeln.
- `main.js`: Reveal-Observer überspringt Elemente in `[hidden]`-Sektionen
  (`if (!el.closest('[hidden]')) io.observe(el)`).

## Abnahme — bestanden (im Browser, nicht im Code vermutet)
- Andreas: visuell durchgescrollt, „geht alles".
- Codi: Live-DOM-Messung auf `pelczer.de/rentus/` → `#bewertungen`:
  `display:none`, **Höhe 0 px** (kein Layout-Loch, kein Aufblitzen möglich),
  `fakeNamenSichtbar: []`, `v=34` auf CSS+JS.

## Geändert (3 Dateien, je in Quelle + Deploy)
`index.html` · `assets/css/style.css` · `assets/js/main.js`.
Sicherheits-Diff: Sektionen **14/14**, ID-Liste identisch, sonst nur v-Bump.
Backups: `*.backup_20260726_094409_reviews` (Quelle) / `*_094637_reviews` (Deploy).

## 🟢 Hoster-Entscheidung: Strato (löst alten offenen Punkt)
- Andreas 26.07.: **Provider = Strato** — ersetzt die frühere IONOS/`info-rentus.de`-Planung.
  CLAUDE.md-Tabu entsprechend aktualisiert (Backup: `CLAUDE.md.backup_20260726_102047_strato`).
- **Ziel-Domain noch nicht bestätigt** — vor Launch von Andreas festlegen lassen.
- **Upload-Paket gebaut** (scratchpad, nicht im Repo): `rentus-strato-upload/` + `.zip`,
  62 Dateien, 22 MB, nur Live-Files ohne `.backup_*`, v=34. Alle Pfade relativ →
  läuft im Webspace-Root. **Upload + Domain verbinden macht Andreas selbst** (KI fasst
  Strato-Menü/FTP nicht an). Reine Statik, kein PHP/DB.

## ⏸️ Offen
1. **BookingPress-Altbuchungen** vor Domain-Umzug klären (Export/Übernahme, dann abschalten).
2. **Ziel-Domain** für den Strato-Launch bestätigen.
3. **Google-Bewertungslink** (CTA in `#bewertungen` steht auf `#`) — moot, solange die
   Sektion versteckt ist; erst mit echten Reviews relevant.
4. **Nicht Teil dieses Auftrags** (separat): Abo-Buttons einmal durchklicken (greift WhatsApp
   mit Abo-Name?) · **Leasing-Abschnitt** (Mikes Wunsch 21.07., Textarbeit, kein Blocker).

## Buch-Bezug
„Nachweis statt Kontrolle" — auf der Seite nur zeigen, was belegbar ist. Erfundene
Kundenstimmen sind das Gegenteil; darum raus, bis echte Google-Reviews vorliegen.
