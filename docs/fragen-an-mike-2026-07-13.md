# Fragen an Mike — Stand 13.07.2026

Gesammelt aus dem Falbe-Review, den offenen Klärungen (CLAUDE.md) und dem Tagesverlauf.
Zum Abhaken. Sortiert: erst was den **Launch blockiert**, dann Preise, Inhalte, Technik, Test.

---

## 1) Abo — der größte offene Brocken (Funnel-Bruch)
- **1a — Wie sollen Abos buchbar sein?** Aktuell springen die 4 „Abo buchen"-Knöpfe in den Buchungs-Wizard,
  aber dort gibt's nur die Pro-Pakete + Fauler Hund → wer ein Abo will, findet es nicht.
  **Codi-Empfehlung:** Abo-Knopf öffnet WhatsApp mit vorbefülltem Text („Interesse an Abo Premium, bitte um
  Rückruf") → Mike ruft zurück. Grund: Abo ist ein Dauer-Vertrag (Rhythmus/Abrechnung) und braucht eh ein
  kurzes Gespräch. Alternative wäre Abo als Wizard-Leistung — passt aber semantisch schlecht (Wizard fragt
  Abholung/Termin für Einzeljob). **Frage an Mike: Rückruf-Weg ok, oder anders gewünscht?**
- **1b — Preislogik der Abos stimmt nicht.** Abo **Plus 99,90 €/Monat** vs. Abo **Premium 149,90 €/¼-jährlich**
  (= ~50 €/Monat). Damit wäre das bessere Premium-Abo *halb so teuer* wie Plus. Übernahmefehler von der
  alten Seite (Intervall/Preis vertauscht)? Oder sind Premium/Deluxe quartalsweise *Leistungen* statt
  Monats-Abo? **Was gilt — je Abo: Preis + Intervall + was drin ist?**

## 2) Preise & Konditionen (nur Mike darf das festlegen)
- **2a — Versiegelungs-Preisliste:** Alt- und Neu-Texte widersprechen sich (Wachs / Teflon / Nano / Keramik /
  Graphen). **Welche Versiegelungsarten bietest du, zu welchem ab-Preis?**
- **2b — „Der Faule Hund":** Preis und Turnus? (Steht aktuell als „60–80 € / Wäsche" im Wizard.) **Stimmt das,
  und in welchem Rhythmus?**
- **2c — SUV/Bus/Van +20 %:** Der Aufschlag ist im Wizard eingerechnet. **Stimmt der Satz von 20 %?**
- **2d — Sonderleistungen:** Alle 10 Preise korrekt? Und: Soll der SUV-Aufschlag auch auf Sonderleistungen
  gelten (aktuell **nicht**) oder bleiben die fix?

## 3) Markenstimme & Inhalte
- **3a — „Sie" oder „Du"?** Die Seite mischt beides (Hero/Über-uns = Sie; Ablauf/FAQ/Buchung = Du).
  **Was passt zu dir — konsequent eins.** (In der Detailing-Szene wird meist geduzt.)
- **3b — Öffnungszeiten:** Fehlen komplett (Kontakt + Footer). **Welche Zeiten / „nach Vereinbarung, Mo–Sa
  erreichbar 8–18 Uhr"?**
- **3c — Google-Bewertungen:** Hast du welche? **Link?** (Aktuell steht der Bewertungs-Link auf „#".)
  Wenn ja: 2–3 O-Töne als Kundenstimmen wären ein starker Vertrauensanker.
- **3d — „Der Faule Hund" als eigene Karte** bei den Abos (nicht nur im Wizard versteckt)? Der Name allein
  ist Marketing. **Möchtest du das prominenter?**
- **3e — Detailtexte der alten Seite:** Gibt es Texte/Angaben von der alten Website, die noch übernommen
  werden sollen?

## 4) Rechtlich absichern
- **4a — „Scheinwerferaufbereitung nach StVZO":** Polierte Scheinwerfer sind zulassungsrechtlich ein
  Graubereich (Bauartzulassung). **Kannst du das „nach StVZO" belegen/absichern — oder sollen wir es
  weicher formulieren?** (Absolutaussagen wie „vollständig"/„garantiert" haben wir schon entschärft.)

## 5) Technik / Domain / Setup
- **5a — E-Mail:** Aktuell `info.rentus@web.de` neben der Domain `info-rentus.de` — wirkt halbfertig.
  Bei IONOS ist ein Postfach `info@info-rentus.de` in Minuten eingerichtet. **Umstellen?**
- **5b — Launch-Domain `info-rentus.de` (IONOS):** Vor dem Umschalten müssen die **BookingPress-Altbuchungen**
  geklärt werden (Export/Übernahme, dann abschalten). **Gibt es dort noch aktive Buchungen/Daten?**

## 6) Was Mike selbst am Handy testen sollte (2-Tage-Test)
- **6a** — Kompletten Wizard auf **seinem eigenen Handy** durchspielen, bis die **WhatsApp-Nachricht wirklich
  bei ihm ankommt** — inkl. Sonderleistungen. **Prüfen: stimmt der Preis in der Nachricht (v.a. mit SUV-Aufschlag)?**
- **6b** — Einmal versuchen, ein **Abo zu buchen** — dann sieht er selbst den Bruch aus Punkt 1.
- **6c** — WhatsApp-Knopf auf einem Gerät **ohne WhatsApp** testen (greift der Fallback?).
- **6d** — Hero-Video im **Mobilfunknetz** (nicht WLAN) laden — trifft echte Kunden.

---

### Nach Mikes Antworten (Codi-Fahrplan)
Feedback einarbeiten **und** den Wartbarkeits-Refactor in einem Rutsch — Pakete/Abos/Sonderleistungen/Preise
landen dann als **eine Datenquelle** in `config.js`, damit künftige Preisänderungen an *einer* Stelle passieren.
