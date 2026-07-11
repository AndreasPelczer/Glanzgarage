# Übergabe 2026-07-11 — 3D-AutoCheck → WhatsApp/Buchung

**Status:** lokal gebaut und geprüft, noch nicht gepusht/live.

## Kontext

Andreas fragte, ob der 3D-AutoCheck in WhatsApp übernommen werden kann, möglichst
mit Bild und Mängelliste, und ob die Buchung mit Auto-Icons sowie SUV +20 %
durchgängig funktioniert.

Technische Grenze: `wa.me` kann zuverlässig nur Text übergeben. Ein Bildanhang
geht im Browser nicht still/automatisch. Mobil ist der saubere Weg die native
Share-API (`navigator.share`) mit Text und PNG-Datei. Desktop bleibt beim
WhatsApp-Text bzw. Buchungs-Text.

## Geändert in `Glanzgarage`

- `tools/3d-check/index.html`
  - Button umbenannt zu `Liste per WhatsApp`
  - neuer Button `Bild + Liste teilen` für mobile/native Share-API
  - neuer Button `In Buchung übernehmen`
  - aktuelles 3D-Canvas wird als `rentus-3d-check.png` teilbar gemacht
  - 3D-Check speichert strukturiert in `localStorage.rentusAutoCheck`
  - gespeicherte Daten: Fahrzeugtyp, Modell, Name/Fahrzeug, Mängelzeilen, Text

- `site/assets/js/main.js`
  - Buchungs-Wizard liest `localStorage.rentusAutoCheck`
  - Fahrzeugtyp aus 3D-Check wird übernommen, z. B. `SUV`
  - 3D-Check-Text wird in `wMsg` eingetragen
  - WhatsApp-Link enthält dann die Mängelliste
  - SUV/Bus/Van +20 % wird als Preis-Hinweis in WhatsApp ergänzt
  - `Der Faule Hund` rechnet bei SUV/Bus/Van jetzt `72-96` statt `60-80`

- `site/index.html`
  - `Der Faule Hund` Preisbereich bekam Datenattribute
    `data-range-min="60"` und `data-range-max="80"`

- `site/assets/css/style.css`
  - Preisbereich bleibt optisch normal, obwohl technisch ein `<i>` verwendet wird

## Gespiegelt in `deadrabbit-landing`

Gleiche Änderungen vorbereitet in:

- `3d-check/index.html`
- `rentus/assets/js/main.js`
- `rentus/index.html`
- `rentus/assets/css/style.css`

Das ist das Repo, das aktuell `https://pelczer.de/rentus/` und
`https://pelczer.de/3d-check/` bedient.

## Lokale Prüfung

Lokal getestet über:

```bash
cd /private/tmp/deadrabbit-landing-codex
python3 -m http.server 8765 --bind 127.0.0.1
```

Prüfung im Browser:

- `/3d-check/` lädt das Modell
- neue Buttons sichtbar:
  - `Liste per WhatsApp`
  - `Bild + Liste teilen`
  - `In Buchung übernehmen`
- Klickweg 3D-Check → `/rentus/#buchung` funktioniert
- bei SUV wird im Wizard `SUV` automatisch selektiert
- `Der Faule Hund` zeigt bei SUV `72-96`
- nach Paketwahl `Pro All in One` steht in der Zusammenfassung `ab 431 €`
- WhatsApp-Link enthält:
  - Fahrzeug `SUV`
  - Preis-Hinweis `SUV/Bus/Van +20 % ist eingerechnet`
  - übernommene 3D-Check-Hinweise
- keine Browser-Console-Errors während der lokalen Prüfung

## Noch offen

1. Nicht gepusht/live. Andreas muss explizit `push live` freigeben.
2. Kontaktformular zeigt weiter auf `DEIN-FORMSPREE-CODE`.
3. Google-Bewertungs-Link zeigt weiter auf `#`.
4. Realer Bildanhang in WhatsApp muss auf einem Handy geprüft werden, weil
   Desktop-Browser je nach Umgebung nur Text-Share anbieten.
5. 3D-Modelle bleiben Welle 3: besser/leichter/markensicher.

## Nächster sinnvoller Schritt

Nach Freigabe:

1. Änderungen in `deadrabbit-landing` committen und pushen.
2. `https://pelczer.de/3d-check/` öffnen, SUV wählen, Namen eintragen.
3. `In Buchung übernehmen` klicken.
4. In `https://pelczer.de/rentus/#buchung` Paket wählen und WhatsApp-Link prüfen.
5. Auf iPhone/Android `Bild + Liste teilen` gegen WhatsApp testen.

## Nachtrag — Korrektur nach Nutzerabnahme

Andreas stellte klar: Der 3D-Check soll auf `pelczer.de/rentus` stattfinden,
nicht als gefühlter Absprung nach `/3d-check/`. Außerdem kam beim Test über
WhatsApp kein Bild mit.

Weiterführende Übergabe:

`docs/uebergaben/2026-07-11-rentus-embedded-autocheck.md`
