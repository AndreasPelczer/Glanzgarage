# Übergabe 2026-07-31 — Echte Google-Bewertungen LIVE + Kopfzeile nachgezogen

> **Quelle:** Codi-Auftrag „Echte Google-Bewertungen einbauen & Sektion sichtbar machen" (30.07.).
> **Kern:** 5 echte Rezensionen aus Mikes Google-Profil wörtlich übernommen, Sektion entriegelt,
> Kopfzeile für den zusätzlichen Menüpunkt umgebaut. Live und per `curl` belegt.
> **Vorgeschichte:** löst den Blocker aus `2026-07-26-fake-reviews-blocker-strato-launch.md`.

## Ergebnis — LIVE
- **`info-rentus.de`** — Bewertungssektion sichtbar, alle 5 Rezensenten im Live-HTML nachgewiesen.
- **`pelczer.de/rentus/`** — ebenfalls live.
- `3d-check/` weiterhin 200 (nicht angefasst).

| Repo | Commit |
|---|---|
| Glanzgarage (Quelle) | `56fbde1` |
| info-rentus (Live-Domain) | `1d5b9e2` |
| deadrabbit-landing (pelczer.de) | `be124e1` |

## Was drin ist
- **5 echte Google-Rezensionen**, wörtlich aus Mikes Unternehmensprofil: Fabio, Chris S.,
  Ashley knoerzer, Mattis Hiske, Gina. **Nichts umformuliert, nichts gekürzt, Tippfehler stehen**
  (§5 UWG). Warnkommentar über der Sektion hält die Regel fest.
- **`hidden`-Attribut entfernt** — die CSS-Notbremse `#bewertungen[hidden]{display:none!important}`
  **bleibt bewusst stehen** als Sicherheitsnetz, falls jemand `hidden` wieder setzt.
- **Schnitt über den Karten:** „5,0 · 11 Bewertungen auf Google" (Profilstand 31.07.2026).
- **Karten von Grid auf `columns` umgestellt.** Grund: echte Rezensionen sind unterschiedlich lang —
  im Grid wurden kurze Karten auf die Höhe der längsten gezogen. `columns` lässt jede Karte in
  ihrer natürlichen Höhe. `max-width: 1120px` hält sie auch bei 1–2 Karten mittig zusammen.
- **Navi:** „Bewertungen" in Desktop- und Mobil-Menü.

## Kopfzeile — der unerwartete Teil
Der zehnte Menüpunkt sprengte die Kopfzeile: Flexbox stauchte „Über uns" auf zwei Zeilen,
Logo und Button überlappten die Nav.

**Gemessen (nicht geschätzt):** Kopfzeile braucht 1062px (Logo 159 + Nav 725 + Button 178).
Der Container ist auf 1180px gedeckelt und hat 90px Rand je Seite — liefert also erst ab
~1242px Fensterbreite genug Platz.

Gelöst mit drei Stellschrauben:
- **„Kontakt" aus der Desktop-Navi** (Andreas' Entscheidung). Bleibt im Burger-Menü, im
  Fußbereich und über den Button „Termin anfragen". Kommentar im Code warnt vor Wiedereinfügen.
- **Nav-Abstand 30 → 20px** + `white-space: nowrap` gegen den Zeilenumbruch.
- **Burger-Umschaltpunkt 1120 → 1240px.**

⚠️ **Nebenbefund:** Die alten **1120px waren schon vorher zu niedrig**. Zwischen 1120 und ~1280px
war die Kopfzeile bereits mit den neun alten Links auf Kante genäht — das ist beim Umbau
aufgefallen, nicht dadurch entstanden.

## Prüfungen
- 14 Sektionen und ID-Liste **unverändert** gegen den Ausgangsstand (Hausregel nach HTML-Patch).
- Kein horizontaler Überlauf mobil — gemessen, `OVERFLOW_COUNT=0` bei 390px.
- Kopfzeile sauber bei 1250 / 1440 / 1920px, Burger greift darunter.
- Vor dem Spiegeln geprüft: alle vier Zieldateien waren exakt der alte Quellstand → nichts überschrieben.
- Live per `curl`: 5 Rezensenten, kein `hidden`, `style.css?v=37` mit `columns`-Regel und
  Breakpoint 1240 auf dem Server angekommen.

**Screenshot-Weg ohne Bildschirmaufnahme-Rechte:** Das Terminal hat keine Bildschirmaufnahme-
Berechtigung. Funktioniert hat headless Chrome:
`/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --headless --disable-gpu
--window-size=B,H --virtual-time-budget=5000 --screenshot=ziel.png URL` gegen einen lokalen
`python3 -m http.server`. **Falle:** Headless erzwingt **mindestens 500px Fensterbreite** — ein
390er-Shot ist nur beschnitten, nicht überlaufend. Echte Mobilbreite geht über eine
Wrapper-Seite mit `<iframe width="390">`.

## Offen
- 🟡 **Mikes Bewertungslink (`g.page/r/…`) fehlt weiter.** Der Button „Jetzt bei Google bewerten"
  zeigt als **Übergangslösung** auf die Google-Maps-Suche nach dem Betrieb — von dort ist
  „Rezension schreiben" einen Klick entfernt. Sobald der echte Link da ist: nur den `href`
  ersetzen, sonst nichts anfassen.
- 🟡 **6 weitere Rezensionen** sind auf Google vorhanden (11 gesamt, 5 eingebaut). Mark Schaebler
  u.a. lagen nur als Anriss im Screenshot vor. Nachtragen **nur per Copy-Paste aus dem echten
  Profil** — nichts rekonstruieren.
- 🟢 **Ginas Text enthält eigene Sterne-Emojis** (⭐️⭐️⭐️⭐️⭐️ im Fließtext), zusätzlich zur
  Sternzeile der Karte. Wirkt doppelt — steht aber wörtlich so im Original, bleibt deshalb stehen.
- 🟢 `deadrabbit-landing/.gitignore` um `*.backup_*` ergänzt (Regel wie in Glanzgarage);
  7 Backup-Dateien lagen dort als untracked herum. **Gelöscht wurde nichts.**
