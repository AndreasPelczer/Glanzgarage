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
   - **Nachgezogen b) „Seite startet nicht oben":** mehrere Ursachen abgearbeitet — (1) 3D-Check-iframe `loading="lazy"`; (2) Safari-Scroll-Restore via `scrollTo(0,0)` auf `load`+`pageshow`; (3) **eigentliche Ursache:** Nav-Klicks hinterließen einen `#anker` in der URL, der beim **Teilen** mitwanderte → geteilte Seite startete mitten drin. Fix: Nav-Klicks halten die URL sauber (`replaceState`), beim Laden wird ein Sektions-Anker entfernt + nach oben (nur `#buchung` bleibt).
   - ⚠️ **LEHRE (wichtig, nicht nochmal jagen):** Am Ende lag's an **iPhone-Safari-Cache** — normaler Tab zeigte die alte Version, **privater Tab startete oben**. `?v=`-Cache-Bump bustet nur Assets, NICHT die HTML selbst; Safari kann die alte HTML servieren. Bei „geht auf Chrome, nicht auf Safari": **erst privaten Tab testen**, bevor man Code jagt.
- **Cache-Stand jetzt: v=11.** Commits u.a. Glanzgarage `4a6ad40`, deadrabbit `fb793fe`.

## Kollision (Lehre)
Andreas hat parallel per GitHub-Upload die Video-Rohdatei hochgeladen, während Codi lokal die Verkabelung baute → non-fast-forward. Sauber per Rebase gelöst (Upload = nur Datei, Verkabelung = Codi). **Nächstes Mal kurz abstimmen, wer welches Feature macht.**

## Offen
- **Formspree:** Kontaktformular-Action ist noch Platzhalter `DEIN-FORMSPREE-CODE` → Formular schickt ins Leere. Braucht Andreas' echte Form-ID ODER Formular für Demo ausblenden.
- **Bewertungen Var. B:** echtes Feedback rein. Mike hat KEINE Google-Bewertungen, nur Instagram `@info.rentus`. Für echte Google-Reviews bräuchte Mike ein **Google Unternehmensprofil** (kostenlos, macht ihn auch in Maps/Suche sichtbar).
- **3D-Modelle + Bus:** bessere Modelle kommen im richtigen Format (**OBJ+MTL**, `models/<slot>/car.obj`+`car.mtl`), Loader NICHT umstellen. Bus = evtl. 6. Slot. Slot-Zuordnung + `unbenannt.glb`-Klärung offen. Neue Modelle lösen evtl. die Orientierungs-Frage (globales `FRONT_POSITIV` reicht bei mehreren Modellen nicht).
- **Augen-Checks (Andreas, Handy):** Hero-Video mobil, WhatsApp Bild+Liste.

## Tabus (unverändert)
Preise · „Fauler Hund"-Konditionen · Launch auf `info-rentus.de` (IONOS, BookingPress-Altbuchungen vorher klären). Ohne Andreas-Go nicht anfassen.
