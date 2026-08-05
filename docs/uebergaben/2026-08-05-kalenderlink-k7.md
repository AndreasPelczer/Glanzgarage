# Übergabe 2026-08-05 (2) — „Termin eintragen"-Link in der Anfrage (K7)

> **Quelle:** Andreas' Entscheidung nach dem Kalender-Gespräch — erst die kleine Lösung
> bauen, die Stufe-2-Anzeige wartet auf Mike.
> **Kern:** Die WhatsApp-Anfrage endet mit einem Google-Kalender-Vorlagenlink. Ein Tipp,
> Termin steht. Kein Zugriff auf Mikes Kalender nötig.
> **Nebenbei:** ein Bestandsfehler im Wizard gefunden und behoben — der wog schwerer als
> das eigentliche Feature.
> **Zusammenhang:** `plan-kalenderanbindung.md` (K7), `fragen-an-mike-2026-08-05-kalender.md`.

## Ergebnis — LIVE

| Repo | Commit |
|---|---|
| Glanzgarage (Quelle) | `cb768e4` |
| info-rentus (Live-Domain) | `704c1a7` |
| deadrabbit-landing (pelczer.de) | `ce0385f` |

Beide Domains liefern `?v=40`, der Kalender-Code ist in der ausgelieferten `main.js`.

## Was gebaut wurde

Am Ende der WhatsApp-Anfrage steht jetzt:

```
📅 Termin eintragen: https://calendar.google.com/calendar/render?action=TEMPLATE&...
```

Der Link öffnet ein **vorausgefülltes Termin-Formular** in Google Kalender. Gespeichert
wird erst, wenn Mike auf Speichern tippt. Deshalb braucht es hier **kein OAuth, keine
Zugangsdaten, keinen Kalenderzugriff und keine Datenschutz-Änderung** — wir schreiben
nichts, wir schlagen etwas vor.

Enthalten sind: Fahrzeug, Leistung, Sonderleistungen, Preis, Kundenname, Telefon,
Abholung/Bringen und der Ort.

### Entscheidungen

- **Titel beginnt mit `ANGEFRAGT:`**, im Termintext steht „noch nicht bestätigt". Ein
  Eintrag, der wie ein fester Termin aussieht, wäre schlimmer als gar keiner — Mike
  könnte einen Tag blockiert glauben, den er nie zugesagt hat.
- **Ohne gewähltes Datum entsteht kein Link.** Wer „nach Absprache" schickt, bekommt
  keinen sinnlosen Termin auf irgendein Datum.
- **Ort:** bei „Bitte abholen" der Kundenort, sonst die Werkstatt (Am Karussell 4).
- **Zeiten über `ctz=Europe/Berlin`, nicht über UTC.** Mit UTC-Umrechnung wäre der
  Termin nach der Zeitumstellung um eine Stunde verrutscht.
- **Halbtags-Zeiten 8–12 / 13–18 Uhr** als Konstante `HALBTAG_ZEIT` oben in der Funktion,
  abgeleitet aus den Öffnungszeiten. Frage K3 an Mike kann das korrigieren — dann ist es
  eine Zeile.
- Der Link steht in der Nachricht, die der **Kunde** verschickt. Beide können ihn nutzen;
  für den Kunden ist ein Eintrag „ANGEFRAGT: …" genauso brauchbar. Deshalb neutral
  beschriftet statt „für Mike".

## ⚠️ Der eigentliche Fund: Auswahlknöpfe lösten nichts aus

Beim Test fiel auf: Ich hatte **Nachmittag** angeklickt, in der erzeugten Nachricht stand
**Vormittag**.

**Ursache:** Nur die Tippfelder (`wName`, `wTel`, `wMsg`, `wOrt`, `wDatum`) waren
registriert, die Auswahlknöpfe `halbtag` und `abhol` nicht. Wer nach dem Ausfüllen noch
auf „Nachmittag" oder „Ich bringe selbst" wechselte, verschickte trotzdem die
Voreinstellung — **und die Zusammenfassung, die der Kunde vor dem Absenden liest, zeigte
ebenfalls den alten Stand.** Der Kunde sah also nicht einmal, dass etwas nicht stimmte.

Das betraf **jede** Anfrage mit abweichender Wahl, seit es den Wizard gibt, und hat mit
dem Kalender-Link nichts zu tun. Mike hat also möglicherweise Anfragen bekommen, in denen
„Bitte abholen" stand, obwohl der Kunde selbst bringen wollte.

**Behoben:** beide Knopfgruppen lösen jetzt `summary()` und `links()` aus. Nebenbei
aktualisiert sich die Zusammenfassung jetzt auch beim Tippen im Datumsfeld — vorher wurde
dort nur der Link neu gebaut, die angezeigte Zusammenfassung blieb stehen.

## Prüfungen

- **Wizard automatisch durchgeklickt** (Kombi → Pro All in One → Ozonbehandlung → Ort,
  Datum, Name, Telefon, Hinweise → Nachmittag) und die erzeugte Nachricht ausgelesen.
  Alle Link-Parameter einzeln zerlegt und geprüft: Titel, Zeitspanne, Zeitzone, Details, Ort.
- **Gegenprobe ohne Datum:** kein Link, Text sagt „nach Absprache". Bestanden.
- **Nach dem Fix erneut:** „Nachmittag" steht in der Nachricht, Link zeigt 13–18 Uhr.
- Länge der fertigen Nachricht: ~810 Zeichen, wa.me-URL ~1230 Zeichen — unkritisch.
- Sektionszahl (14) und ID-Liste (32) unverändert, `index.html`-Diff nur Cache-Bump.
- Live per `curl` auf beiden Domains.

## Offen

- 🟡 **Von Mike ungetestet.** Der Link ist maschinell geprüft, aber noch nicht von Mike
  auf seinem Handy angetippt. Das sollte er einmal tun — dann sieht er auch, ob ihm die
  Angaben im Termin reichen.
- 🟡 **Halbtags-Zeiten sind eine Annahme** (8–12 / 13–18) aus den Öffnungszeiten. Frage K3.
- 🟢 **Nur Google.** Der Vorlagenlink ist Google-spezifisch. Falls Mike auf Apple- oder
  Outlook-Kalender wechselt, bräuchte es stattdessen eine `.ics`-Datei — die kann eine
  statische Seite nicht ohne Weiteres pro Anfrage erzeugen.
- 🟢 **Der Link enthält die Kundendaten in der Adresse.** Sie gehen erst an Google, wenn
  jemand den Link antippt — und stehen ohnehin schon in derselben WhatsApp-Nachricht.
  Verhältnismäßig, aber der Vollständigkeit halber notiert.
