# Übergabe 2026-07-11 — Rentus eingebetteter 3D-AutoCheck

**Wer:** Codex im Auftrag von Andreas  
**Wann:** 11.07.2026, nach Live-Test der ersten AutoCheck/WhatsApp-Version  
**Warum:** Andreas hat bei der Abnahme klargestellt, dass der 3D-Check auf
`pelczer.de/rentus` selbst stattfinden soll. Außerdem kam beim Test über
WhatsApp kein Bild mit.

## Ausgangslage

Es gab bereits mehrere 3D-/Schaden-Werkzeuge:

- `/rentus/` hatte eine 3D-Sektion mit reinem `model-viewer` und einem Button
  zu `/3d-check/`.
- `/3d-check/` hatte den markierbaren 3D-Check mit Mängelliste.
- `/schaden/` hatte eine 2D-Skizze.
- `/3d-test/` war eine ältere/andere 3D-Testvariante.

Die Lücke war nicht "3D-Check fehlt", sondern: Der markierbare Check war nicht
direkt im Rentus-Ablauf erlebbar.

## Änderungen

### In `deadrabbit-landing`

- `rentus/index.html`
  - Die vorhandene 3D-Check-Sektion `#dreid-check` bleibt erhalten.
  - Der alte `model-viewer` wurde durch ein Iframe ersetzt:
    `/3d-check/?embed=1`
  - Der Hauptbutton heißt jetzt `Zur Buchung`.
  - Zusätzlich bleibt ein Sekundärlink `3D-Check einzeln öffnen`.
  - Der ungenutzte `model-viewer`-Script-Import wurde entfernt.

- `rentus/assets/css/style.css`
  - Neue Klasse `.dreid__frame` für das eingebettete Werkzeug.
  - Desktop-Höhe: `760px`, mobil: `1120px`.

- `3d-check/index.html`
  - Unterstützt `?embed=1`.
  - Im Embed-Modus werden eigene große Überschrift, Subline und Footer
    ausgeblendet, damit es als Rentus-Werkzeug wirkt.
  - Button `Liste per WhatsApp` wurde zu `WhatsApp-Text öffnen`, damit klar ist:
    dieser Weg ist text-only.
  - Button `Report-Bild speichern` ergänzt.
  - `In Buchung übernehmen` hat `target="_top"`, damit aus dem Iframe die
    Hauptseite nach `/rentus/#buchung` navigiert.
  - `Bild + Liste teilen` erzeugt jetzt ein Report-PNG, in dem die Mängelliste
    direkt im Bild steht. Grund: WhatsApp übernimmt Text und Datei je nach
    Gerät/App-Version nicht zuverlässig gemeinsam.

### In `Glanzgarage`

Gleiche Quelländerungen gespiegelt in:

- `site/index.html`
- `site/assets/css/style.css`
- `tools/3d-check/index.html`

Dokumentation aktualisiert:

- `README.md`
- `docs/README-UPLOAD.md`
- `site/README-UEBERGABE.md`
- Nachtrag in `docs/uebergaben/2026-07-11-autocheck-whatsapp.md`

## Technische Entscheidung

Kein neuer 3D-Check. Der bestehende `/3d-check/` bleibt die Quelle des
markierbaren Werkzeugs und wird in `/rentus/` eingebettet. So kann die Funktion
weiter einzeln getestet werden, fühlt sich für Kunden aber als Teil der
Rentus-Seite an.

## WhatsApp-Grenze

`wa.me` kann nur Text zuverlässig öffnen. Bildanhang per Browser geht nur über
`navigator.share()` und nur mit Nutzeraktion. Selbst dann entscheidet das Gerät
und WhatsApp, ob Text + Bild gemeinsam übernommen werden. Deshalb enthält das
Report-Bild die Mängelliste selbst.

## Lokale Prüfung

Server:

```bash
cd /private/tmp/deadrabbit-landing-codex
python3 -m http.server 8765 --bind 127.0.0.1
```

Geprüft:

- `/3d-check/?embed=1` direkt:
  - Embed-Klasse aktiv
  - H1 ausgeblendet
  - Canvas vorhanden
  - Buttons `WhatsApp-Text öffnen`, `Bild + Liste teilen`,
    `Report-Bild speichern`, `In Buchung übernehmen`
  - `In Buchung übernehmen` hat `target="_top"`
- `/rentus/#dreid-check`:
  - Rentus-Sektion zeigt den 3D-Check eingebettet
  - Iframe-Höhe und Werkzeug sichtbar
  - 3D-Check wirkt als Teil der Rentus-Seite

## Noch zu prüfen nach Push

1. Live auf `https://pelczer.de/rentus/#dreid-check` öffnen.
2. Im eingebetteten Check Fahrzeug markieren.
3. `In Buchung übernehmen` klicken und prüfen, dass die Hauptseite zu
   `/rentus/#buchung` springt.
4. Auf einem echten Handy `Bild + Liste teilen` gegen WhatsApp testen.
5. Wenn WhatsApp weiterhin nur das Bild oder nur Text übernimmt: Report-Bild
   speichern und manuell anhängen; für automatisiertes Medien-Senden wäre die
   WhatsApp Business API nötig.
