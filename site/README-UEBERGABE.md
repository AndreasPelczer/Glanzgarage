# RENT US Glanzgarage — neue Website (Übergabe)

Statische Website (HTML/CSS/JS), läuft auf **jedem** Webspace ohne Datenbank/WordPress.

## Was hochgeladen wird
Einfach den **kompletten Inhalt des Ordners `site/`** in das Web-Root des Hosters
(meist `httpdocs/`, `public_html/` oder `www/`) hochladen. Fertig.

```
site/
├── index.html          ← Startseite (Onepage)
├── impressum.html
├── datenschutz.html
└── assets/
    ├── css/style.css
    ├── js/main.js
    └── img/            ← alle Bilder
```

## Noch zu erledigen (2 Kleinigkeiten)

1. **Kontaktformular scharf schalten**
   Aktuell zeigt das Formular auf einen Platzhalter. Zwei Wege:
   - **Formspree** (kostenlos, kein Server nötig): auf formspree.io anmelden,
     Formular-Code holen und in `index.html` bei `action="https://formspree.io/f/DEIN-FORMSPREE-CODE"`
     einsetzen.
   - **oder** falls der Hoster PHP kann: kleines `send.php` bauen (sag Bescheid).
   Bis dahin funktionieren **WhatsApp, Telefon und E-Mail** als Kontaktweg voll.

2. **Online-Terminbuchung**
   Die alte Seite hatte einen WordPress-Buchungskalender (BookingPress). Auf einer
   statischen Seite gibt es den nicht 1:1. Der aktuelle Weg ist ein
   Buchungs-Wizard mit WhatsApp-/Mail-Ausgabe. Fahrzeugtyp, Paket, Abholung und
   Wunschtermin werden in eine fertige Anfrage übertragen; SUV/Bus/Van +20 %
   wird eingerechnet. Für echte Kalender-Slots später z. B. Calendly/eTermin
   oder Kalender-Backend anbinden.

3. **3D-Check / WhatsApp**
   Der 3D-Check läuft direkt im Rentus-Abschnitt als eingebettetes Werkzeug.
   Er kann eine Mängelliste an den Buchungs-Wizard übergeben. Auf Mobilgeräten
   gibt es zusätzlich "Bild + Liste teilen" über die native Share-API. Das
   geteilte Bild enthält die Mängelliste im Bild selbst, weil WhatsApp Text und
   Datei je nach Gerät/App-Version getrennt behandeln kann. Automatisches
   WhatsApp-Senden mit Bild ohne Nutzerauswahl ist im Browser nicht zuverlässig
   möglich; dafür bräuchte man WhatsApp Business API.

## Gut zu wissen
- **Schriften** kommen von Google Fonts (extra Zeile im Datenschutz). Wenn keine
  externen Google-Verbindungen gewünscht sind, lassen sich die Fonts lokal einbinden.
- **Karte**: OpenStreetMap-Einbettung (kein Google-Maps-Key nötig).
- **Rechtstexte** (Impressum/Datenschutz) wurden aus der alten Seite übernommen und
  aktualisiert. Bitte vom Betreiber gegenchecken lassen — ich bin kein Anwalt.
- **Bilder**: stammen laut altem Impressum von Pexels, Pixabay und Mike Knörzer.

## Lokal ansehen
```
cd site
python3 -m http.server 4599
# dann http://localhost:4599 im Browser
```

## Inhalte / Preise (Stand: von info-rentus.de übernommen)
- Pakete: Pro Inside ab 199 €, Pro Outside ab 199 €, Pro All in One ab 359 €, Pro Deluxe ab 649 €
- Abos: Smart 49,90 €/mtl., Plus 99,90 €/mtl., Premium 149,90 €/¼-jährl., Deluxe 199,90 €/¼-jährl.
- Versiegelung: Wachs ab 199 €, Teflon ab 249 €, Nano ab 599 €, Keramik ab 899 €, Graphen ab 1049 €
- 10 Sonderleistungen mit Einzelpreisen
