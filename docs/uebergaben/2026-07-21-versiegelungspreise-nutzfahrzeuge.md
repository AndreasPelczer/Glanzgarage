# Übergabe 2026-07-21 — Versiegelungspreise korrigiert + Nutzfahrzeuge

> **Quelle:** WhatsApp-Voicemail Mike Knörzer 21.07. 08:41 (1:48 min), Memo mit
> Live-Gegenprüfung der Preise.
> **Kern:** 🔴 Preisversatz auf der Versiegelungs-Sektion (Launch-Blocker) behoben.

## Endstand
- **LIVE** (curl-belegt, ~36 s nach Push): `pelczer.de/rentus/`
  Versiegelungspreise = **ab 199 € | ab 249 € | ab 599 € | ab 899 € | auf Anfrage**
  (Wachs/Teflon/Nano/Keramik/Graphen) · **Nutzfahrzeuge-Pill** drin.
- **Beide Repos gepusht & synchron:**
  - Glanzgarage (Quelle) `ea3cd17` == origin
  - deadrabbit-landing (Deploy) `d5dc953` == origin
- Reine HTML-Änderung → **kein v-Bump** (bleibt v=32, betrifft nur JS/CSS).

## 🔴 Preisversatz Versiegelung — behoben
Jeder Preis stand **eine Karte zu tief**. Korrigiert (Mikes Ansage, gegen Live geprüft):

| Karte | War live ❌ | Jetzt ✅ |
|---|---|---|
| Wachs | ab 249 € | **ab 199 €** |
| Teflon | ab 599 € | **ab 249 €** |
| Nano | ab 899 € | **ab 599 €** |
| Keramik | ab 1049 € | **ab 899 €** |
| Graphen | auf Anfrage | **auf Anfrage (unverändert)** |

- **Graphen bleibt „auf Anfrage"** — Andreas-Entscheid 21.07. (Mike hatte 1049 diktiert,
  aber erst gegenchecken lassen). → offene Mike-Rückfrage.
- Das Intro „Individuelle Lackversiegelung ab 199 €" (index.html ~Z.315) war immer korrekt.
- **Damit ist Frageliste-Punkt 1 (Preiskonflikt 199 vs. 249) geschlossen.** Launch-Blocker weg.

## 🟡 Nutzfahrzeuge — Textnennung (Andreas-Entscheid: nur Text jetzt)
- Lead-Satz (Z.124): „… Youngtimer, Oldtimer **oder Nutzfahrzeug** …"
- Neue Pill „**Nutzfahrzeuge**" (nach Oldtimer, vor Firmen-Fuhrparks).
- Eine **eigene Fahrzeugklasse im Wizard** wurde **bewusst NICHT** gebaut → braucht Mike
  (eigene Klasse? welcher Aufschlag? Preise = Tabu).

## Geändert (1 Datei)
`site/index.html` — 6 Zeilen: Lead + 1 Pill + 4 Preise. Sicherheits-Diff: Sektionen 14/14,
IDs 32/32, sonst nichts. Backup: `site/index.html.backup_20260721_103153`.

## ⏸️ Offen — wartet auf Mike
1. **Graphen** — Preis „ab 1049 €" auszeichnen oder „auf Anfrage" lassen?
2. **Nutzfahrzeuge im Wizard** — eigene Fahrzeugklasse + Aufschlag, oder reicht Textnennung?
3. **Leasing-Infobox** (Wunsch 2) — Mikes Wording. Rechenbeispiel Leasingrückläufer
   (Nachbelastung vs. Aufbereitung). ⚠️ **4.000 € = Mikes Erfahrungswert, nicht belegt** →
   weich formulieren („schnell im vierstelligen Bereich"), keine feste Summe versprechen.
4. **Status an Mike** fällig — Textentwurf lag in der Session vor; verschickt Andreas.

## Unverändert offen (Launch-Vorbereitung)
- **Hoster IONOS vs. Strato für `info-rentus.de` — UNKLAR.** CLAUDE.md nennt IONOS, Übergabe
  18.07. markiert es als unbestätigt. **Vor Launch einloggen & prüfen**, wo die Domain liegt.
  Zugangsdaten fasst die KI nicht an. **BookingPress-Altbuchungen** vorher klären (Export).
- Google-Bewertungslink (steht auf `#`) · Detailtext-Übernahme alte Seite.
- Tabus unverändert (Preise / Fauler Hund / Launch-Domain).
