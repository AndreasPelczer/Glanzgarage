# Plan — Mikes Google-Kalender an den Buchungs-Wizard anbinden

**Stand 05.08.2026**

- **K7 (kleine Lösung) ist GEBAUT und LIVE** — „Termin eintragen"-Link in der
  WhatsApp-Anfrage. Siehe `uebergaben/2026-08-05-kalenderlink-k7.md`.
- **Stufe 2 (freie Halbtage anzeigen) wartet weiter auf Mikes Antworten**
  (`fragen-an-mike-2026-08-05-kalender.md`, K1–K6). Der Rest dieses Plans gilt
  unverändert — er beschreibt den Ausbau, nicht das schon Gebaute.

---

## Ziel

Der Wizard zeigt im Schritt „Wunschtermin" nur noch Halbtage an, an denen Mike
tatsächlich Zeit hat. Die Anfrage geht danach **wie bisher per WhatsApp** raus.

**Ausdrücklich kein Ziel: verbindliche Selbstbuchung.** Bei einer Aufbereitung hängen
Dauer und Preis vom Zustand des Fahrzeugs ab — deshalb gibt es den 3D-Check. Wer fest
bucht, erwartet auch einen festen Preis. Mike behält das letzte Wort; gewonnen wird die
Terminsuche, nicht die Zusage.

Andreas' Entscheidung 05.08.: Stufe 2 („freie Tage sehen, dann anfragen"),
Technik auf der eigenen Box.

## Warum das heute weh tut

Der Wizard fragt in Schritt 4 ein **freies Datumsfeld** plus vormittags/nachmittags ab
(`#wDatum`, `halbtag`) und schickt das als Text an WhatsApp. Der Kunde rät also einen
Termin. Passt er nicht, beginnt das Hin und Her — bei jeder einzelnen Anfrage.

## Architektur

```
Mikes Google-Kalender
        │  geheime iCal-Adresse, nur lesend
        ▼
   Mops-Box  ──►  wirft alle Details weg, rechnet auf Halbtage um
        │         (kein Kundenname, kein Betreff verlässt die Box)
        ▼
   verfuegbarkeit.json  ──►  git push ins Repo
        │
        ▼
   GitHub Pages  ──►  Wizard liest die Datei beim Öffnen
```

**Die Website fragt die Box nie live.** Sie liest eine kleine statische Datei. Das ist
bewusst so:

- Die Seite bleibt schnell und funktioniert, wenn die Box aus ist (Lehre aus dem stehenden
  Backup-iMac im Juli — Hardware zuhause fällt aus, und dann darf nicht die Kundenseite
  hängen).
- Mikes Kalender ist nie über das Internet erreichbar, auch nicht indirekt.
- Kein OAuth, kein Google-Cloud-Projekt, keine Token, die ablaufen.

## Warum iCal und nicht die Google-API

Google bietet pro Kalender eine **Privatadresse im iCal-Format** — eine geheime URL, die
den Kalender als Datei ausliefert. Das reicht vollständig für „wann ist belegt".

Die Alternative (Google Calendar API mit OAuth) bräuchte ein Cloud-Projekt, einen
Zustimmungsbildschirm, Tokens mit Ablaufdatum und regelmäßige Pflege — für dieselbe
Information. Die API lohnt sich erst, wenn wir **in** Mikes Kalender schreiben wollen.
Das wollen wir laut Zielsetzung nicht.

⚠️ Die iCal-Adresse ist ein Geheimnis wie ein Passwort. Sie gehört **auf die Box in eine
`.env`, nie ins Repo, nie in einen Chat.** Wer sie hat, liest Mikes kompletten Kalender.

## Datenformat

`site/assets/data/verfuegbarkeit.json`

```json
{
  "stand": "2026-08-05T06:00:00+02:00",
  "horizont_bis": "2026-09-16",
  "tage": {
    "2026-08-10": { "vm": "frei",   "nm": "belegt" },
    "2026-08-11": { "vm": "belegt", "nm": "belegt" },
    "2026-08-12": { "vm": "frei",   "nm": "frei"   },
    "2026-08-16": { "vm": "zu",     "nm": "zu"     }
  }
}
```

- `frei` / `belegt` / `zu` — `zu` heißt „Mike arbeitet da grundsätzlich nicht"
  (Sonntag, Urlaub, Feierabend). Für den Kunden sieht `belegt` und `zu` gleich aus; der
  Unterschied hilft bei der Fehlersuche.
- `stand` — wann die Box zuletzt gerechnet hat.
- `horizont_bis` — wie weit die Box gefüllt hat (Vorschlag: 6 Wochen). Danach zeigt der
  Wizard „auf Anfrage" statt zu behaupten, es sei frei.

## Sicherungen (die eigentliche Arbeit)

| Fall | Verhalten |
|---|---|
| Datei fehlt oder ist kaputt | Wizard verhält sich wie heute: freies Datumsfeld |
| `stand` älter als 3 Tage | Wie heute, plus dezenter Hinweis „Termine auf Anfrage" |
| Datum jenseits `horizont_bis` | Wählbar, aber als „auf Anfrage" gekennzeichnet |
| Alle Halbtage belegt | Kein Sackgassen-Bildschirm — „Bitte anfragen, wir finden was" |

Die Regel dahinter: **Eine veraltete Verfügbarkeit ist schlimmer als gar keine.** Wenn die
Seite im November noch Augusttermine anbietet, verliert Mike Kunden, statt welche zu
gewinnen. Lieber zurück auf das freie Feld.

## Bauabschnitte

**A — Sichtbarer Teil (geht ohne Mike)**
Anzeige im Wizard-Schritt 4, gespeist aus einer Beispieldatei. Inklusive aller Sicherungen
oben. Ergebnis: Andreas kann Mike zeigen, wie es aussieht, bevor irgendein Zugang
existiert. Der WhatsApp-Text bekommt den gewählten Halbtag wie gehabt.

**B — Box-Dienst (braucht K1–K4 aus dem Fragenzettel)**
Kleines Skript neben `mops-api`: iCal holen → Termine auf Halbtage legen → Arbeitszeiten
und Sperrzeiten anwenden → JSON schreiben → ins Repo committen und pushen. Läuft per
systemd-Timer, z. B. stündlich. Schreibt nur diese eine Datei, nichts anderes.

**C — Trockenlauf, dann scharf**
Die Box erzeugt die Datei **eine Woche lang, ohne dass die Seite sie nutzt.** Mike
vergleicht: Stimmt „belegt" mit der Wirklichkeit? Erst wenn das passt, wird die Anzeige
eingeschaltet. Dieser Schritt ist nicht optional — siehe Risiko 1.

## Risiken

1. **Mikes Kalender ist die Wahrheit — oder eben nicht.** Wenn er Termine im Kopf hat
   statt im Kalender, zeigt die Seite frei, wo längst ein Wagen steht. Dann ist das
   Ergebnis schlechter als heute. Deshalb Abschnitt C. **Das ist das Hauptrisiko, und es
   ist kein technisches.**
2. **Dauer je Leistung fehlt.** Ohne die Angabe „Deluxe = 2 Tage" kann die Seite nicht
   sagen, ob ein einzelner freier Halbtag überhaupt reicht. Erste Ausbaustufe darf das
   ignorieren (nur Halbtag anzeigen), aber dann muss Mike beim Bestätigen aufpassen.
3. **Privat und geschäftlich im selben Kalender.** Blockiert dann auch der Zahnarzt.
   Sauberer wäre ein eigener Kalender „RENT US" — aber nur, wenn Mike ihn wirklich pflegt.
   Ein zweiter, halb gepflegter Kalender ist schlimmer als ein voller.
4. **Box fällt aus.** Abgefangen durch die Stand-Prüfung, aber jemand muss es merken.
   Vorschlag: Der Box-Dienst meldet sich, wenn er dreimal hintereinander nicht durchkommt.
5. **Datenschutz.** In dieser Bauform verlassen **keine Kundendaten** die Seite Richtung
   Google — wir lesen nur. Die Datenschutzerklärung braucht trotzdem einen Satz dazu,
   sobald die Anzeige live ist. Wenn später doch in Mikes Kalender geschrieben werden soll,
   ist das eine neue Bewertung (dann verarbeitet Google Kundendaten für Mike).

## Wann sich das nicht lohnt

Wenn Mike weniger als etwa fünf Anfragen pro Woche hat oder ohnehin gern telefoniert,
ist der Aufwand größer als der Gewinn. Dann reicht die kleine Variante: ein
„In Kalender eintragen"-Link in der WhatsApp-Anfrage, damit er nicht abtippen muss.
**Ehrlich prüfen, bevor Abschnitt B gebaut wird.**

## Nächster Schritt

Andreas spricht mit Mike (`fragen-an-mike-2026-08-05-kalender.md`). Danach entscheiden:
Abschnitt A vorziehen (zum Zeigen) oder direkt A+B, sobald die Zugangsdaten da sind.
