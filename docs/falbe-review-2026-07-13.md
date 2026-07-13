# Falbe-Review (Fable 5.0 / Statik-QA) — 13.07.2026

Statik-Blick über die Live-Seite (v=24). Urteil: **steht** — wirkt professionell, Texte mit
Handschrift, Vorher/Nachher + Galerie verkaufen. Aber: **Abo-Buchung ist ein tragendes Teil, das
noch fehlt.** Punkte 1–4 vor dem Mike-Test/Launch, Rest kann in eine spätere Welle.

> ⚠️ **FREEZE aktiv** (Mike-Review ~2 Tage). Nichts davon ohne Andreas-Go deployen. Spalte „Entscheider"
> zeigt, was reiner Fix ist vs. was Mike-Input braucht.

## MUSS (vor Mike-Test / Launch)

| # | Punkt | Fixart | Entscheider |
|---|---|---|---|
| 1 | **Abos nicht buchbar.** Abo-Bereich hat 4× „Abo buchen" → springt zu #buchung, aber Wizard Schritt 2 kennt nur die 4 Pro-Pakete + Faulen Hund. Wer Abo Premium will, findet es nicht → **größter Funnel-Bruch.** Optionen: (a) Abos als wählbare Leistung in den Wizard, oder (b) Abo-CTAs auf WhatsApp/Anruf mit vorbefülltem Text umleiten (leichterer, risikoärmerer Fix). | Bug/Feature | Andreas (Weg a/b) |
| 2 | **Abo-Preislogik prüfen.** Abo Plus 99,90 €/**mtl.** vs. Abo Premium 149,90 €/**¼-jährlich** (~50 €/Monat) → Premium mit Handwäsche+Versiegelung wäre *halb so teuer* wie Plus. Übernahmefehler alte Seite (Intervall↔Preis)? Oder Premium/Deluxe = quartalsweise *Leistungen* statt monatliches Abo? → klarer formulieren. | Inhalt/Preis | **Mike** (TABU-Preis) |
| 3 | **FAQ nennt „Kontaktformular", das es nicht gibt.** Seite hat nur Wizard→WhatsApp, Tel, E-Mail. Wort → „über die Online-Anfrage" o.ä. | reiner Textfix | Andreas |
| 4 | **Sie/Du-Mischmasch.** Hero/Über-uns/Kontakt = Sie; Ablauf/FAQ/Buchung = Du. Fällt unbewusst auf. Eins konsequent. Detailing duzt meist. | Inhalt/Stimme | **Mike** |

## SOLLTE

| # | Punkt | Entscheider |
|---|---|---|
| 5 | **Öffnungszeiten fehlen** (Kontakt + Footer). Kunden & Google erwarten das. Z.B. „Termine nach Vereinbarung, Mo–Sa 8–18 Uhr erreichbar". | Mike (Zeiten) |
| 6 | **„Der Faule Hund" nur im Wizard.** Mikes charmantestes Produkt + bester Einstiegs-Hook hat keine eigene Karte. Gehört als Karte zu den Abos — der Name allein ist Marketing. | Andreas/Mike |
| 7 | **Kundenstimmen fehlen.** 2–3 Google-Rezensionen / O-Töne als Vertrauensanker. Falls Google-Bewertungen da: verlinken (Link steht aktuell auf `#`). | Mike (Rezensionen) |
| 8 | **Zwei juristisch heikle Formulierungen:** „Scheinwerferaufbereitung **nach StVZO**" (Bauartzulassung = Graubereich, absichern oder weicher). Absolutaussagen „neutralisieren Schimmelsporen **vollständig**" + „Zufriedenheit **garantiert**" → wettbewerbsrechtlich angreifbar. Ein „nahezu" kostet nichts. | Andreas (Textfix) + Mike (StVZO) |
| 9 | **E-Mail `info.rentus@web.de` neben Domain `info-rentus.de`** wirkt halbfertig. IONOS-Postfach `info@info-rentus.de` in 5 Min einrichtbar. | Mike (IONOS) |

## KLEINKRAM (später / Welle 4)

| # | Punkt |
|---|---|
| 10 | **Schema.org LocalBusiness-Markup** (Adresse, Tel, Öffnungszeiten) für Google — lohnt für „Fahrzeugaufbereitung Würzburg". |
| — | **WhatsApp-Button auf Gerät OHNE WhatsApp testen** (Desktop-Weg greift?). |
| — | **Hero-Video-Load im Mobilfunknetz** (nicht WLAN) testen — trifft Mike-Kunden real. |

## Was Fable Mike für den 2-Tage-Test konkret mitgeben würde
1. **Kompletten Wizard auf eigenem Handy** durchspielen, bis die WhatsApp-Nachricht wirklich bei ihm
   ankommt — inkl. Sonderleistungen **und SUV-Aufschlag: stimmt der Preis in der Nachricht?**
2. **Versuchen, ein Abo zu buchen** — dann merkt er selbst, was Punkt 1 & 2 meinen.

## Codi-Einordnung (Umsetzbarkeit, wenn grün)
- **Sofort & risikoarm, reine Textfixes:** #3 (FAQ-Wort), #8-Teil (Absolutaussagen entschärfen). Kein Preis/Struktur.
- **Kleiner, sicherer Funktions-Fix:** #1 Weg (b) — Abo-CTAs auf WhatsApp mit vorbefülltem Text (kein Wizard-Umbau).
- **Braucht Mike zuerst:** #2 (Preis-TABU!), #4 (Stimme), #5/#7/#9 (Zeiten/Rezensionen/IONOS), #8-StVZO.
- Passt inhaltlich zum geplanten **Wartbarkeits-Refactor**: #1/#2/#6 landen ohnehin in `config.js` (Pakete+Abos als eine Datenquelle) → Refactor + diese Fixes am besten in einem Rutsch nach Mikes Feedback.
