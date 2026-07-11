# Übergabe 2026-07-11 (Abend) — Codi: v5, Bewertungen, WhatsApp-Bugs, Hero-Video

**Wer:** Codi (Implementierer) · **Status:** alles committet + gepusht + live per curl belegt.
Kundendaten/Externes bleiben draußen; GitHub = Wahrheit; Heimat der Repos ab heute `~/XcodeProjects/`.

## Repo-Aufräum-Aktion (wichtig für die nächste Instanz)
- Frische Sitzung fand lokal keine brauchbaren Repos. **Festlegung Andreas:** GitHub = einzige Wahrheit, kanonische Heimat **`~/XcodeProjects/`** (Sticks/INTENSO sind keine Heimat mehr).
- Beide Repos frisch geklont: `~/XcodeProjects/Glanzgarage` (Quelle) + `~/XcodeProjects/deadrabbit-landing` (Deploy). Git-lose Alt-Kopie → `~/Alt-Ablage/2026-07-12/`.
- **`CLAUDE.md` neu angelegt** (Rolle/Repos/Hausregeln/Tabus) — beide Repos lesen sie automatisch.
- ⚠️ **Plattenwarnung:** `~/Documents` UND (zeitweise) `~/Downloads` sind TCC-gesperrt für das Terminal. `~/Downloads` sprang mitten in der Sitzung vom INTENSO-Symlink auf den internen (gesperrten) Ordner. **Dateien, die Codi einbauen soll, direkt ins Repo legen** (per Finder), nicht über Downloads. Finder-**Kopie** verliert die Quarantäne (so kam das Hero-Video rein).

## Heute gebaut & LIVE (pelczer.de)
1. **v5 eingespielt** (Falbe-Paket) in beide Repos. Abnahme bestanden.
2. **Fake-Google-Bewertungen ausgeblendet** (`#bewertungen hidden`, Var. A). 3 erfundene Reviews (Sabine R./Thomas K./Melanie B.) — Verstoß gegen echt-statt-behauptet + Abmahnrisiko. ⚠️ `hidden` lässt die Fake-Texte noch im Quelltext → **vor Go-live Var. B** (echtes Feedback, z.B. Instagram `@info.rentus`).
3. **WhatsApp-Audit + Bugfix:** „Name: -" in der Buchungs-Anfrage gefixt (3D-Check-Feld „Fahrzeug & Name" → wandert jetzt ins Namensfeld). Fahrzeugtyp-Übernahme war KEIN Bug (das „Bus/Van" im Test war manuelle Wahl). Nur EIN Button liefert Bild+Liste (`navigator.share`, nur mobil) — technische wa.me-Grenze, kein Fehler.
4. **Hero-Video mobil** (pingpong-Loop) live: `hero__bg` → `<video class=hero__video>` + `<picture class=hero__pic>` Fallback, CSS zeigt Video nur `@max-width:700px`, Desktop behält Foto. Video-Asset `HTTP 200`.
   - **Nachgezogen a):** mobil aufgehellt — dunkler `hero__bg::after`-Overlay oben/mittig heller (.42/.30 statt .72/.55, unten dunkel für Textlesbarkeit) + Video `filter:brightness(1.3)`. Andreas: „passt".
   - **Nachgezogen b):** Seite startete unten/mittig statt oben (Safari-**Scroll-Restore**, Landepunkt wanderte). Fix: `history.scrollRestoration='manual'` + `scrollTo(0,0)` auf `load` UND `pageshow` (bfcache), wenn kein `#`-Anker; 3D-Check-iframe auf `loading="lazy"`. **Getestet: Chrome/Mac/iPhone-Safari starten alle oben.** ✅
- **Cache-Stand jetzt: v=10.** Commits u.a. Glanzgarage `f55672d`, deadrabbit `cb293c4`.

## Kollision (Lehre)
Andreas hat parallel per GitHub-Upload die Video-Rohdatei hochgeladen, während Codi lokal die Verkabelung baute → non-fast-forward. Sauber per Rebase gelöst (Upload = nur Datei, Verkabelung = Codi). **Nächstes Mal kurz abstimmen, wer welches Feature macht.**

## Offen
- **Formspree:** Kontaktformular-Action ist noch Platzhalter `DEIN-FORMSPREE-CODE` → Formular schickt ins Leere. Braucht Andreas' echte Form-ID ODER Formular für Demo ausblenden.
- **Bewertungen Var. B:** echtes Feedback rein. Mike hat KEINE Google-Bewertungen, nur Instagram `@info.rentus`. Für echte Google-Reviews bräuchte Mike ein **Google Unternehmensprofil** (kostenlos, macht ihn auch in Maps/Suche sichtbar).
- **3D-Modelle + Bus:** bessere Modelle kommen im richtigen Format (**OBJ+MTL**, `models/<slot>/car.obj`+`car.mtl`), Loader NICHT umstellen. Bus = evtl. 6. Slot. Slot-Zuordnung + `unbenannt.glb`-Klärung offen. Neue Modelle lösen evtl. die Orientierungs-Frage (globales `FRONT_POSITIV` reicht bei mehreren Modellen nicht).
- **Augen-Checks (Andreas, Handy):** Hero-Video mobil, WhatsApp Bild+Liste.

## Tabus (unverändert)
Preise · „Fauler Hund"-Konditionen · Launch auf `info-rentus.de` (IONOS, BookingPress-Altbuchungen vorher klären). Ohne Andreas-Go nicht anfassen.
