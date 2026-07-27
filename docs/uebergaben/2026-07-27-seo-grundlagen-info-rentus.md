# Übergabe 2026-07-27 — SEO-Grundlagen für info-rentus.de (LIVE)

> **Quelle:** Codex-Auftrag „SEO-Grundlagen für info-rentus.de".
> **Kern:** robots.txt, sitemap.xml, canonical + LocalBusiness-JSON-LD — via PR gemergt, live & getestet.

## Ergebnis
- **LIVE auf `info-rentus.de`** (Google Rich-Results-Test auf der **Live-URL**: „2 gültige Elemente —
  Lokale Unternehmen + Unternehmen, 0 Fehler", erfolgreich gecrawlt 27.07. 14:04).
- **PR #1** (github.com/AndreasPelczer/info-rentus/pull/1) gemergt → `c3a24f7`, Branch gelöscht.
- Live geprüft: `robots.txt` 200, `sitemap.xml` 200 (3 URLs), canonical + JSON-LD auf index,
  canonical auf impressum + datenschutz.

## Was drin ist
- **`robots.txt`** (Wurzel): `Allow: /` + `Sitemap: https://info-rentus.de/sitemap.xml`.
- **`sitemap.xml`** (Wurzel): Start (prio 1.0), Impressum, Datenschutz (prio 0.3), lastmod 2026-07-26.
- **`index.html`** vor `</head>`: `<link rel="canonical" href="https://info-rentus.de/">` +
  JSON-LD `["LocalBusiness","AutoWash"]` — Name, url, image (og-hero.jpg), description, telephone
  (+4915901606913), email (info.rentus@web.de), priceRange €€, address (Am Karussell 4, 97280
  Remlingen, Bayern, DE), areaServed (Würzburg/Wertheim/Marktheidenfeld/Remlingen),
  openingHoursSpecification **Mo–Fr 08:00–18:00**, sameAs [] (leer, für Instagram später).
- **`impressum.html` / `datenschutz.html`**: je ein canonical.

## Entscheidungen / Kontext
- **Öffnungszeiten Mo–Fr 08–18** im Schema = spiegelt den bereits sichtbaren Kontakt-Text
  (index Zeile 751 „Mo–Fr 8–18 Uhr · Sa nach Vereinbarung"). **Samstag bewusst NICHT** als feste
  Zeit ins Schema (nur „nach Vereinbarung" sichtbar). Falls echte Zeiten abweichen: sichtbaren
  Text UND Schema gemeinsam ändern.
- **3-Repo-Verteilung:** robots.txt + sitemap.xml nur ins info-rentus-Repo (Domain-Wurzel) +
  Glanzgarage/site (Wahrheit/Record). canonical + JSON-LD in alle drei — auf `pelczer.de/rentus`
  ist canonical→info-rentus.de ein **Dedup-Signal** (sagt Google: info-rentus.de = Original).
- **Workflow:** info-rentus als Branch+PR (Review vor Live, wie im Auftrag gefordert); Glanzgarage
  + deadrabbit direkt zu main. Commits: Glanzgarage `d52617b`, deadrabbit `c73a09a`, info-rentus (PR) `2f99eec`.
- **Fehlend:** das im Auftrag zitierte Voicemail-Memo `2026-07-26-mike-voicemail-memo.md` existiert
  nicht (galt schon beim Faulhund-Auftrag).

## ⏸️ Offen — Andreas separat (NICHT Teil des Auftrags)
- **Search Console** einrichten + Sitemap einreichen: `https://info-rentus.de/sitemap.xml`.
- **Öffnungszeiten final bestätigen** (Mo–Fr 08–18).
- **`sameAs`** später mit Instagram-Profil füllen (Mike launcht auf Instagram) → dann JSON-LD ergänzen.

## Buch-Bezug
„Nachweis statt Kontrolle" — strukturierte Daten machen die (belegbaren) Fakten der Werkstatt für
Google maschinenlesbar; keine Behauptung, die nicht auch sichtbar auf der Seite steht.
