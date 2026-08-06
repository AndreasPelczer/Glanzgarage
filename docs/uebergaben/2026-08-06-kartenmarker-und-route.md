# Übergabe 2026-08-06 — Kartenmarker stand 20 km daneben + „Route starten"

> **Quelle:** Andreas' Frage — „Warum nutzen wir nicht die Google-Karte? Dann wäre die
> Navigation einfacher."
> **Kern:** Die Frage führte zu einem gravierenderen Fund: **Der Marker der Standortkarte
> zeigte seit dem Launch auf einen Punkt rund 20 km entfernt.** Korrigiert, plus den
> Knopf gebaut, der für Navigation eigentlich zuständig ist.
> **Ergebnis der ursprünglichen Frage:** Google-Karte vorerst nicht — Begründung unten.

## ⚠️ Der Fund

Die eingebettete OpenStreetMap-Karte in der Kontakt-Sektion hatte den Marker auf
**49.756 / 9.962**. Gegenprobe über Nominatim:

> Mariengrotte, Oberer Kirchbergweg, **Heidingsfeld, Würzburg**

Das sind rund **20 km** von der Werkstatt entfernt. Schlimmer noch: Der Kartenausschnitt
(`bbox` Längengrad 9,90–10,02) enthielt Remlingen **nicht einmal** — wer auf die Karte
sah, bekam Würzburg und einen Marker im Nirgendwo.

Das stand so seit dem Launch am 26.07. auf der Live-Domain.

**Jetzt:** Mitte der Straße „Am Karussell" — **49.80273 / 9.69900**, über Nominatim
bestimmt (Straßenausdehnung ermittelt, Mittelpunkt berechnet), Ausschnitt eng gezogen.

⚠️ **Hausnummern sind für diese Straße in den OSM-Daten nicht erfasst** — per Overpass
geprüft, es gibt dort keine einzige. Der Marker liegt deshalb auf Straßenmitte (etwa auf
Höhe Nr. 14). **Andreas' Entscheidung: so lassen, Mike bestätigt die genaue Einfahrt
später.** Dann nur die beiden Zahlen im `marker=`-Parameter ersetzen.

## Neu: Knopf „Route starten"

Der eigentliche Wunsch war einfachere Navigation — und dafür war die Karte nie
zuständig. **Kunden navigieren nicht von einer eingebetteten Karte, sie tippen auf
„Route".** Einen solchen Knopf gab es auf der ganzen Seite nicht; die Adresse stand nur
als Text da. (Der einzige Google-Link auf der Seite ist der *Bewertungs*-Link.)

- Sitzt direkt unter der Karte in der Kontakt-Sektion.
- Übergibt die **Adresse als Text**, nicht die Koordinaten. Google und Apple haben
  bessere Adressdaten als OSM und finden die Hausnummer 4, die in der Karte fehlt.
  Mit Koordinaten würde die Navi den Kunden auf die Straßenmitte schicken.
- Im HTML steht Google Maps (läuft überall, auch ohne JavaScript). `main.js` schaltet
  auf **Apple Karten** um, wenn das Gerät ein iPhone/iPad/Mac ist — dann öffnet sich die
  App statt einer Browserseite.
- Datenschutzerklärung um einen Satz ergänzt (Abschnitt 7): Der Knopf lädt nichts nach,
  sondern öffnet erst nach Klick den Kartendienst des Geräts — analog zum bestehenden
  WhatsApp-Absatz.

## Warum keine Google-Karte (vorerst)

Eine eingebettete Google-Karte lädt beim **Seitenaufruf** Google-Server und überträgt die
IP jedes Besuchers. Das braucht in Deutschland eine aktive Einwilligung — also eine
Zwei-Klick-Lösung mit Platzhalter plus Datenschutz-Absatz. Geschätzt 3–4 Stunden gegenüber
45 Minuten für Markerkorrektur und Routen-Knopf.

**Andreas' Entscheidung: erst das Günstige, Google-Karte später neu bewerten.** Wenn die
korrigierte OSM-Karte reicht, entfällt der Aufwand ganz.

## Prüfungen

- Marker per **Nominatim** gegengeprüft (vorher/nachher) und per **Overpass** bestätigt,
  dass keine Hausnummern vorliegen.
- OSM-Embed-URL liefert 200.
- Kontakt-Sektion bei 1220 px und 500 px angesehen: Karte lädt, Marker mittig, Knopf
  sitzt sauber und nimmt am Handy die volle Breite.
- Sektionszahl 14 unverändert, IDs 32 → 33 (nur der neue `routeLink`).
- Vor dem Spiegeln per `md5` geprüft: beide Ziele waren exakt der alte Quellstand.

### ⏳ Auslieferung hing an GitHub, nicht an uns
Zum Zeitpunkt dieser Übergabe meldete githubstatus.com **„Incident with Pages –
Deployment Lag"** (Pages: `degraded_performance`, Actions: `partial_outage`). Beide
Deploy-Repos standen minutenlang auf `building`. Alles ist gepusht; die Auslieferung
zieht nach, sobald GitHub wieder normal läuft. **Vor dem Abhaken bitte gegenprüfen:**

```
curl -s https://info-rentus.de/ | grep -o 'marker=49[^&"]*'
```
Erwartet: `marker=49.80273%2C9.69900`

### Testmethodik — zwei Fallen, die Zeit gekostet haben
1. **Die Aktions-Werbung legt sich über jeden Screenshot.** Im Test-Wrapper vorher
   `localStorage.setItem('rentus_aktion_gesehen', …)` setzen (gleicher Origin genügt).
2. **`.reveal`-Elemente sind unsichtbar, bis der Observer greift** — und das Einblenden
   verschiebt danach das Layout. Richtige Reihenfolge: erst `.in` setzen **und warten**,
   dann `scrollIntoView`. Sonst landet der Screenshot in der Galerie statt beim Kontakt.
3. Die OSM-Einbettung braucht inzwischen **WebGL**. Headless mit `--disable-gpu` zeigt
   „WebGL ist erforderlich" — mit `--use-gl=swiftshader` erscheint die Karte. Kein Fehler
   der Seite, aber ein Hinweis: auf sehr alten Geräten ohne WebGL sieht ein Besucher
   statt der Karte einen Hinweistext. Der Routen-Knopf funktioniert dort trotzdem.

## Offen

- 🟡 **Mike bestätigt die genaue Einfahrt.** Bis dahin Straßenmitte.
- 🟡 **Von Mike ungetestet:** „Route starten" auf seinem Handy — und einmal von einem
  fremden Gerät aus, damit klar ist, dass die Navi wirklich am richtigen Hof landet.
- 🟢 **Google-Karte** als bewusst vertagte Option, siehe oben.
- 🟢 **Der Routen-Knopf steht bisher nur in der Kontakt-Sektion.** Denkbar wäre er auch
  im Fußbereich oder in der Anfrage-Bestätigung — erst mal beobachten, ob er gebraucht wird.
