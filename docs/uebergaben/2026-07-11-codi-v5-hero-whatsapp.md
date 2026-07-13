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
- **Cache-Stand jetzt: v=14.** (v=11 Scroll-Fix · v=12 Kontaktformular→WhatsApp · v=13 Desktop-Hero-Video (`hero-d.mp4`, quadratisch 640×640, H.264, 1,8 MB, nur `@min-width:701px`) · v=14 Desktop-Video etwas heller.) **Hinweis:** `hero-d.mp4` ist QUADRAT, kein echtes Querformat — auf breitem Hero waagerecht cover-geschnitten, sieht ok aus; echtes Querformat-Video wäre besser (morgen). Foto bleibt Poster/Fallback.

**GEPLANT (morgen früh, siehe `docs/ideen.md`):** WhatsApp-Sachen + **Buchung↔3D-Check verschmelzen** („beides anbieten", optional) auf Branch `feature/buchung-3d-fusion`. Dazu **Innenraum-Check-Idee** (Flecken markieren). **Modelle noch nicht einsatzbereit** (in `tools/3d-check/models-neu/`: OBJs ohne vollständige .mtl/Texturen, GLBs zu groß — via Meshy zu leichten GLBs aufbereiten; NICHT ins Repo committen, ~40 MB). Demo läuft mit den alten, funktionierenden Modellen.

## Kollision (Lehre)
Andreas hat parallel per GitHub-Upload die Video-Rohdatei hochgeladen, während Codi lokal die Verkabelung baute → non-fast-forward. Sauber per Rebase gelöst (Upload = nur Datei, Verkabelung = Codi). **Nächstes Mal kurz abstimmen, wer welches Feature macht.**

## Offen
- **Formspree:** ✅ ERLEDIGT (Demo-Flick, v=12) — Kontaktformular-Action war Platzhalter `DEIN-FORMSPREE-CODE` (schickte ins Leere). Jetzt: **Formular → WhatsApp** (submit öffnet wa.me mit Name/E-Mail/Telefon/Nachricht, Pflichtfelder greifen). Offen für morgen: entscheiden, ob echtes Formspree/E-Mail-Backend gewünscht, oder WhatsApp-Routing bleibt (passt zum WhatsApp-Thema).
- **Bewertungen Var. B:** echtes Feedback rein. Mike hat KEINE Google-Bewertungen, nur Instagram `@info.rentus`. Für echte Google-Reviews bräuchte Mike ein **Google Unternehmensprofil** (kostenlos, macht ihn auch in Maps/Suche sichtbar).
- **3D-Modelle + Bus:** bessere Modelle kommen im richtigen Format (**OBJ+MTL**, `models/<slot>/car.obj`+`car.mtl`), Loader NICHT umstellen. Bus = evtl. 6. Slot. Slot-Zuordnung + `unbenannt.glb`-Klärung offen. Neue Modelle lösen evtl. die Orientierungs-Frage (globales `FRONT_POSITIV` reicht bei mehreren Modellen nicht).
- **Augen-Checks (Andreas, Handy):** Hero-Video mobil, WhatsApp Bild+Liste.

## Tabus (unverändert)
Preise · „Fauler Hund"-Konditionen · Launch auf `info-rentus.de` (IONOS, BookingPress-Altbuchungen vorher klären). Ohne Andreas-Go nicht anfassen.
