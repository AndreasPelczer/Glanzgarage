# Übergabe 2026-07-26 (2) — info-rentus.de LIVE über GitHub Pages

> **Kern:** Die RENT-US-Seite läuft jetzt unter der eigenen Domain **`info-rentus.de`** mit HTTPS 🔒.
> Fortsetzung der Session „Fake-Reviews-Blocker" — nach dem Fix wollte Andreas die Seite auf
> „Strato" hosten.

## Ergebnis
- **LIVE + HTTPS:** `https://info-rentus.de/` → 200, unsere Seite (v=34, keine Fake-Reviews).
  http → 301 auf https, `www` funktioniert, Zert = Let's Encrypt (bis 24.10.26, auto-renew).
- **3D-Buchung funktioniert** über `https://info-rentus.de/3d-check/`.

## Was wirklich passiert ist (wichtig fürs Verständnis)
- Andreas' Strato-Produkte: **SmartWebsite Pro** (Baukasten) + **HiDrive Free** (Cloudspeicher).
  **Keins kann eine fertig gebaute Seite hosten.** Domain `info-rentus.de` liegt aber bei Strato.
- Entscheidung „Weg B": Seite bleibt beim jetzigen Host (GitHub Pages), Domain zeigt drauf —
  Adresszeile bleibt sauber `info-rentus.de`.
- Auf `info-rentus.de` lag vorher eine **alte Baukasten-RENT-US-Seite** (Strato-IP 212.227.172.249).
  Die ist jetzt abgelöst.

## Umsetzung (Etappen)
1. **Neues Repo `info-rentus`** (`~/XcodeProjects/info-rentus`, public) = Deploy für die eigene
   Domain. Inhalt aus `deadrabbit-landing/rentus/` an die **Wurzel** gespiegelt + `CNAME`=info-rentus.de.
   GitHub Pages an (main/root).
2. **DNS bei Strato** (Andreas geklickt, Domainverwaltung → info-rentus.de → DNS):
   - **A-Record:** „Eigene IP" = **185.199.108.153** (Strato erlaubt nur EINE IP — reicht).
   - **CNAME:** `www` → `andreaspelczer.github.io` (Typ musste von TXT auf CNAME korrigiert werden!).
3. **HTTPS-Zert hing 90 Min** auf `null` (nicht gestartet). DNS war sauber (kein AAAA/CAA-Blocker).
   Fix: **Custom-Domain in GitHub einmal ab- und wieder angemeldet** (CNAME-Datei raus/rein) →
   Provisioning startete (`new`→`approved`). Dann `https_enforced=true` (per `-F`, nicht `-f`!).

## Learnings / Fallen
- **`3d-check/` MUSS ins `info-rentus`-Repo mit.** Der Wizard lädt absolut `/3d-check/` +
  `/3d-check/innen.html` als iframe → ohne den Ordner: **404 im Buchungs-Wizard** (genau passiert,
  gefixt: Live-3d-check aus deadrabbit gespiegelt, 17 MB). Keine weiteren absoluten Pfade offen.
- **Zwei Deploy-Ziele jetzt:** deadrabbit (pelczer.de/rentus) UND info-rentus (info-rentus.de).
  Bei Seitenänderungen beide spiegeln (siehe CLAUDE.md „Repos & Workflow").
- **gh api Boolean:** `https_enforced=true` mit `-F` (typisiert), nicht `-f` (String).
- **HTTPS-Edge-Lag:** nach cert=`approved` liefert `https://` noch ~1–3 Min lang `000`, bevor TLS steht.

## ⏸️ Offen
- **SmartWebsite Pro (Strato)** läuft ungenutzt weiter → **nicht kündigen**, bis geklärt ist, ob
  die Domain am Vertrag hängt. Mit Mike klären.
- Alte Baukasten-Seite: falls **Terminbuchungen** drin waren, prüfen.
- `pelczer.de/rentus` existiert weiter (Zweit-/Staging-Adresse).
