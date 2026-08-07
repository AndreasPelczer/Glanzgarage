# CLAUDE.md — Glanzgarage / RENT US

Website & Buchung für **RENT US Glanzgarage**, Kunde **Mike Knörzer**
(Am Karussell 4, 97280 Remlingen). Auftragnehmer: Andreas Pelczer (B.I.N.D.A. Verlag).
**Diese Datei wird von jeder Terminal-Sitzung automatisch gelesen — erst hier lesen, dann bauen.**
Zuerst außerdem: `docs/uebergaben/` (neueste zuerst) + `docs/alte-website-inventur.md`.

## Rolle
- **Andreas entscheidet.** **Falbe prüft** (Statik/QA). **Codi baut** (Implementierer — das bist du).
- Nichts geht live ohne Andreas' ausdrückliches „push live" / Go.

## Repos & Workflow
- **Glanzgarage** (`~/XcodeProjects/Glanzgarage`) = **Wahrheit / Quelle. Hier bauen.**
  `site/` = Website · `tools/3d-check/` = 3D-Check-Werkzeug · `docs/` = Doku + Übergaben.
- **deadrabbit-landing** (`~/XcodeProjects/deadrabbit-landing`) = **Deploy (pelczer.de).**
  GitHub Pages → **live auf pelczer.de** (Push auf `main` = live). Live-Ordner: `rentus/`, `3d-check/`.
- **info-rentus** (`~/XcodeProjects/info-rentus`) = **Deploy (ECHTE Live-Domain seit 26.07.26).**
  GitHub Pages + Custom Domain **`info-rentus.de`** (HTTPS via Let's Encrypt, DNS liegt bei
  Strato: A→185.199.108.153, www CNAME→andreaspelczer.github.io). **Alles an der Wurzel** (nicht
  in `rentus/`). ⚠️ **`3d-check/` MUSS hier mit rein** — der Wizard lädt absolut `/3d-check/`;
  fehlt der Ordner → 404 im Buchungs-Wizard (Vorfall 26.07.).
- **Ablauf:** in Glanzgarage bauen → spiegeln nach **beiden** Deploys:
  deadrabbit (`site/`→`rentus/`, `tools/3d-check/`→`3d-check/`) UND info-rentus
  (`site/`→Wurzel, `3d-check/`→`3d-check/`) → committen → pushen →
  **live per `curl` verifizieren** (`https://info-rentus.de/`).
- **GitHub ist die einzige Wahrheit.** Sticks/externe Platten sind keine Repo-Heimat
  (Lehre 11.07.: leerer INTENSO-Mount). Kanonische Heimat: `~/XcodeProjects/`.
- ⚠️ `~/Documents` ist auf diesem Mac TCC-gesperrt (kein Terminal-Zugriff) — dort NICHT arbeiten.

## Hausregeln
- **Cache-Bump bei JEDER JS/CSS-Änderung:** `?v=` in allen Referenzen hochzählen (v=6, v=7 …).
- **Nach jedem HTML-Patch: Sektionszahl + ID-Liste gegen vorher diffen.**
  Prüfen, ob das **Alte noch da ist** — nicht nur, ob das Neue drin ist.
  (Falbe-Vorfall 11.07.: Patch-Regex fraß Galerie + Über-uns-Sektion.)
- **Echte Fotos, keine Stock-Behauptungen. Kennzeichen immer verpixeln. EXIF/GPS vor Upload strippen.**
- Muster vom Mitbewerber lernen ok — Grafiken/Code nie kopieren.
- **Jede Sitzung endet mit einer datierten Übergabe** in `docs/uebergaben/`.
- **Fertig = committet + gepusht + live per `curl` belegt.**
- **Aktions-Werbung wechseln = nur `site/assets/js/aktionen.js`.** Ein Block pro Aktion mit
  Von/Bis-Datum; Anleitung steht im Kopf der Datei. Kein HTML anfassen, keine Logik ändern.
  Ausprobieren mit `?aktion=test` an der URL. (Übergabe 04.08.)
- ⚠️ **Nie in einem Deploy-Spiegel bauen.** Die Suche nach „rentus" findet vier plausible
  Ordner — gebaut wird **immer** in `Glanzgarage/site/`. (Verfahrensfehler 04.08.)

## Tabus (ohne Andreas-Go NICHT anfassen)
- **Preise** — inkl. offenem **Versiegelungs-Preiskonflikt** (Wachs/Teflon/Nano/Keramik/Graphen
  widersprechen sich alt↔neu, siehe Inventur) und **„Fauler Hund"** Preis/Turnus.
  Nicht raten — ein falscher ab-Preis ist eine Preisdiskussion mit jedem Kunden.
- **Launch VOLLZOGEN 26.07.26:** live auf **`info-rentus.de`** (HTTPS 🔒), gehostet über
  GitHub Pages (Repo `info-rentus`). Strato ist nur **Domain-Registrar** — Seite liegt NICHT
  bei Strato (SmartWebsite-Baukasten kann keine eigene Seite hosten). DNS bei Strato zeigt
  auf GitHub. **SmartWebsite Pro läuft noch, wird nicht mehr gebraucht — NICHT kündigen,
  bis geklärt ist, ob die Domain am Vertrag hängt.** Falls in der alten Baukasten-Seite
  Terminbuchungen waren: prüfen.
- Strato/IONOS-Zugangsdaten fasst die KI nicht an; Strato-Kundenmenü/FTP macht Andreas selbst.

## Offene Klärungen (Mike / Andreas)
Formspree-Code (Platzhalter `DEIN-FORMSPREE-CODE`) ·
Versiegelungs-Preisliste · Fauler-Hund-Konditionen · Detailtext-Übernahme aus der alten Seite.
~~Google-Bewertungslink~~ — **erledigt 07.08.**: `https://g.page/r/CfEteytVLOowEAE/review`,
im Button und im Flyer-QR. Siehe `docs/uebergaben/2026-08-07-bewertungslink-und-flyer-qr.md`.
Details: `docs/alte-website-inventur.md`.
