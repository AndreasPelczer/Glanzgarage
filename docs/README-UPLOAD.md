# Upload-Anleitung — rentus Site-Update (Welle 1)

## Was ist neu (11.07.2026)
1. **Hero:** echtes Foto (Heckflosse mit gespiegeltem RENT-US-Schild),
   auf Mobil der blaue Klassiker. og:image (WhatsApp/Facebook-Vorschau) ebenfalls neu.
2. **Galerie:** alle 8 Stock-Fotos ersetzt durch Mikes echte Arbeit (Kennzeichen verpixelt).
3. **Vorher/Nachher:** jetzt ZWEI echte Regler — Sitzreinigung (gleicher Tag,
   morgens/abends) und Kofferraum.
4. **Intro-Bild:** Stock-Porsche ersetzt durch das Spiegel-Markenfoto.
5. **Farbwelt:** Akzentgrün auf das echte Garagen-Grün #9fb247 umgestellt
   (gemessen aus den Fotos — Website spricht jetzt dieselbe Farbsprache wie
   Werkstatt, Flyer und Kinowerbung).

## Hochladen (Strato)
Diesen Ordnerinhalt 1:1 in das bestehende Site-Verzeichnis laden —
es werden nur Dateien ÜBERSCHRIEBEN oder ERGÄNZT, nichts muss vorher gelöscht werden:
- `index.html`                → ersetzt die alte
- `assets/css/style.css`      → ersetzt
- `assets/js/main.js`         → ersetzt
- `assets/img/real/` (NEU)    → kompletter Ordner, 24 Dateien, 4,8 MB

## Danach prüfen (Handy + Desktop)
- [ ] Hero lädt, Mobil zeigt den blauen Wagen
- [ ] Beide Vorher/Nachher-Regler lassen sich ziehen
- [ ] Galerie: 8 echte Fotos, Klick öffnet Großansicht
- [ ] Kennzeichen nirgends lesbar (KONTROLLE-kennzeichen im Asset-ZIP)
- [ ] Grünton überall einheitlich

## Aufräumen (optional, später)
Die alten Stock-Dateien werden nicht mehr referenziert und können vom Server
gelöscht werden: hero-audi.jpg, lambo-orange.jpg, wheel-foam.jpg, beads-red.jpg,
leather-seats.jpg, ferrari-wheel.jpg, polish-taillight.jpg, engine.jpg,
porsche-matte.jpg, polish-porsche.jpg. (mike.jpg und logo.png bleiben!)

## Noch offen (Welle 2+)
- Buchungs-Wizard (Fahrzeugwahl → Paket → Halbtags-Slot)
- 3D-Check-Integration + bessere Modelle
- Kontaktformular-Backend (wohin geht "Nachricht senden"?)
- Google-Bewertungs-Link (aktuell #) — Mikes Google-Profil-Link eintragen
- Hero-Video, falls Mike liefert
- noindex entfernen + Domain-Entscheidung beim Launch
