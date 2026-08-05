# Übergabe 2026-08-05 — Aktions-Streifen am Handy erklärt sich jetzt selbst

> **Quelle:** Mikes Rückmeldung über Andreas — „auf dem PC mega, auf dem Handy nicht wirklich
> aussagekräftig, weil nichts beschrieben ist und nicht jeder weiß, was es mit dem Hund auf
> sich hat."
> **Kern:** Neues Feld `kurz` in `aktionen.js` — ein Halbsatz, der am Handy die Sache nennt.
> Rechner-Ansicht unangetastet. Live und per `curl` belegt.
> **Vorgeschichte:** baut auf `2026-08-04-aktions-werbung-fauler-hund.md`.

## Ergebnis — LIVE

| Repo | Commit |
|---|---|
| Glanzgarage (Quelle) | `83fd9ff` |
| info-rentus (Live-Domain) | `6814fc1` |
| deadrabbit-landing (pelczer.de) | `16e9873` |

Beide Domains liefern `?v=39` aus, `kurz` ist in der ausgelieferten `aktionen.js` enthalten.

## Was das Problem war

Kein Bug — ein **Zuschnitt-Fehler von gestern**. Am Handy sind `claim` und `text` per CSS
ausgeblendet, damit der Streifen flach bleibt. Übrig blieben Eyebrow, Name und Preis:

> MIKES RENNER · **DER FAULE HUND** · 99,90 € · [Jetzt anfragen]

Wer den Namen kennt, liest ein Angebot. Wer ihn nicht kennt, liest einen Hundenamen und
einen Preis — und nirgends steht, dass es ums Autowaschen geht. Am Rechner fiel das nicht
auf, weil dort der ganze Absatz danebensteht.

## Die Lösung

**Neues Feld `kurz`** in `site/assets/js/aktionen.js`:

```
kurz: "Außen waschen, innen saugen — schnell und günstig.",
```

- **Nur am Handy sichtbar.** Am Rechner `display: none` — dort steht `text`.
- **Auf zwei Zeilen begrenzt** (`-webkit-line-clamp: 2`). Ein zu langer Text wird
  abgeschnitten, statt den Streifen über den halben Bildschirm wachsen zu lassen.
- **Fällt auf `claim` zurück**, wenn das Feld fehlt — eine Aktion ohne `kurz` zeigt am Handy
  also weiterhin etwas an, nur weniger Passendes.
- Anleitung im Kopf von `aktionen.js` ergänzt, Faustregel drin: *verständlich für jemanden,
  der den Namen der Aktion noch nie gehört hat — die Sache nennen, nicht die Stimmung.*

**Nicht angefasst:** die Rechner-Ansicht (Mikes ausdrückliches Lob), Preis, Zeitraum,
die Fauler-Hund-Sektion weiter unten auf der Seite.

## Prüfungen

- **500 px** — Erklärzeile einzeilig.
- **echte 390 px** über iframe-Wrapper — zweizeilig, sauberer Umbruch, kein Überlauf,
  Knopf und Preis bleiben nebeneinander.
- **1280 px** — Rechner-Karte pixelgleich zum Vorzustand, `kurz` unsichtbar.
- Sektionszahl (14) und ID-Liste (32) unverändert; `index.html`-Diff sind ausschließlich
  die drei Cache-Bump-Zeilen.
- Vor dem Spiegeln per `md5` geprüft: beide Ziele waren exakt der alte Quellstand.
- Live per `curl` auf beiden Domains, danach Screenshot der echten Domain in Handybreite.

**Diesmal richtig herum gearbeitet:** gebaut in `Glanzgarage/site/`, dann gespiegelt —
im Gegensatz zum Verfahrensfehler vom 04.08.

### Kleine Falle am Rande
Ein Screenshot der **Live-Domain in einem `file://`-Wrapper-iframe** zeigt die Aktion nicht
(Cross-Origin plus Ladezeit sprengen das virtuelle Zeitbudget). Für Live-Belege direkt gegen
die URL schießen; der iframe-Wrapper für echte 390 px funktioniert nur gegen `localhost`.

## Offen

- 🟡 **Foto.** Weiterhin `dog.jpg` aus dem Bestand. Ein echtes Foto vom Faulen Hund wäre
  besser — am Handy ist das Bild nur 96 px breit, ein klares Motiv wirkt dort stärker.
- 🟡 **Nächste Aktion ab 01.11.2026** weiterhin offen mit Mike.
- 🟢 **Text der Erklärzeile** ist mein Vorschlag, nicht Mikes Wortlaut. Wenn er es anders
  sagen würde — eine Zeile in `aktionen.js`, sonst nichts.
