# Strömung

**Flüssigkeit zum Anfassen.** Eine Strömungsrechnung in Echtzeit im Browser: Tinte mit der Maus
umrühren, eine Kármánsche Wirbelstraße hinter einem Zylinder zusehen — und zwei Zahlen nachmessen,
die außerhalb dieses Blattes feststehen.

→ **[Blatt öffnen](https://ssims437.github.io/stroemung/)**

- **Tinte**: mit gedrückter Maustaste hineinfahren und rühren, ~45 Bilder je Sekunde bei 160 × 80
  Zellen
- **Wirbelstraße**: gleichmäßige Anströmung, ein Zylinder, und dahinter lösen sich Wirbel
  abwechselnd ab — in der Drehungsansicht das klassische rot-blaue Bild
- **Taylor-Green**: ein Wirbelfeld, das nichts tut als zu zerfallen — mit der Theoriekurve darüber
- **Prüflauf** — sieben Zeilen, darunter die Zerfallsrate gegen `exp(−2νk²t)` und die
  Strouhal-Zahl gegen 0,20

## Die beiden Prüfsteine

**Der Zerfall.** Für den Taylor-Green-Wirbel gibt es eine geschlossene Lösung: Die Amplitude fällt
wie `exp(−2νk²t)`. Gemessen fällt sie **immer schneller** — und die Differenz ist bei vier
verschiedenen Zähigkeiten **dieselbe Zahl**:

| gesetzte Zähigkeit ν | Rate gemessen | Rate laut Theorie | Differenz als Zähigkeit |
|---|---|---|---|
| 0 | 0,259 | 0,000 | 3,3·10⁻³ |
| 0,0005 | 0,298 | 0,039 | 3,3·10⁻³ |
| 0,001 | 0,336 | 0,079 | 3,3·10⁻³ |
| 0,002 | 0,413 | 0,158 | 3,2·10⁻³ |

Streuung: **1,0 %**. Mit dieser Zugabe zusammen stimmt die Formel also auf ein Prozent. Und die
Zugabe ist keine Physik, sondern das Verfahren — sie schrumpft mit feinerem Gitter in **erster
Ordnung** (4,4 → 3,3 → 2,2 ·10⁻³ bei 48², 64², 96²).

**Die Wirbelstraße.** Hinter einem angeströmten Zylinder lösen sich Wirbel abwechselnd ab. Die
Ablösefrequenz f, der Durchmesser D und die Anströmung U bilden eine Zahl ohne Einheit:
**St = f·D/U ≈ 0,20** — seit Strouhal 1878 gemessen, über einen weiten Bereich fast konstant.
Dieses Blatt misst sie an der Querbewegung drei Durchmesser hinter dem Zylinder:

| Gitter | Zellen je Durchmesser | gemessenes St |
|---|---|---|
| 144 × 72 | 13 | 0,180 |
| 192 × 96 | 17 | 0,185 |
| 288 × 144 | 26 | **0,196** |

Der Wert wächst mit der Auflösung auf die Literatur zu. Deshalb summen Freileitungen im Wind, und
deshalb bekommen hohe Schornsteine Wendel aufgeschweißt.

## Was der Prüflauf zeigt

| Behauptung | Ergebnis |
|---|---|
| **nach dem Ausgleich quillt nichts mehr** | Quellstärke 1,6·10¹ → **8,8·10⁻⁴** · Faktor **1,8·10⁴** |
| **der Zerfall folgt der Theorie — plus einer festen Zugabe** | 4 Zähigkeiten · Zugabe konstant auf **1,0 %** |
| die Zugabe schrumpft mit feinerem Gitter | **erste Ordnung** in der Zellgröße |
| aus nichts wird nichts | ruhendes Becken, 40 Schritte · größte Geschwindigkeit **exakt 0** |
| **die Farbe bleibt nicht erhalten** | 15 % → 8 % mit feinerem Gitter · am Zeitschritt fast nichts |
| **die Wirbelstraße schwingt, und die Zahl stimmt** | St 0,180 → 0,185, wachsend gegen 0,20 |
| dieselbe Rechnung, dasselbe Ergebnis | Unterschied exakt 0 |

## Was mich das gekostet hat

**„Quellenfrei" war zuerst eine leere Behauptung.** Die erste Fassung stand auf einem *gemeinsamen*
Gitter — Druck und Geschwindigkeit auf denselben Punkten, wie in Stams berühmtem Code von 1999.
Gemessen: Der Druckausgleich senkte die Quellstärke von 15,9 auf **10,5**. Nach achtzig Durchgängen.
Das ist kein Löserproblem, sondern ein Gitterproblem: Mit zentralen Differenzen sieht der Druck nur
den **übernächsten** Nachbarn, und ein Schachbrettmuster bleibt für ihn unsichtbar. Die Lösung ist
das **versetzte Gitter** — Geschwindigkeiten auf den Zellwänden, Druck in der Mitte. Danach:
15,9 → **0,00088**. Faktor 18 000 statt Faktor 1,5.

**Zwanzig Durchgänge machen es schlimmer als gar keine.** Der Löser arbeitet mit
Überrelaxation — er schießt bei jedem Durchgang absichtlich über das Ziel hinaus, weil er sonst
für die großräumigen Anteile ewig braucht. Der Preis steht in der Prüfzeile: nach 20 Durchgängen
ist die Quellstärke **größer** als vorher (68 statt 16), nach 60 bei 0,76, nach 150 bei 0,00088.
Wer zu früh aufhört, hat es schlimmer gemacht — im laufenden Bild ist das kein Problem, weil jeder
Zeitschritt dort weitermacht, wo der letzte aufgehört hat.

**Der Wirbel zerfiel viel zu schnell — und das war die interessanteste Messung.** Bei ν = 0 dürfte
gar nichts zerfallen; gemessen wurde eine Rate von 0,26. Der Reflex ist, einen Fehler zu suchen.
Es ist aber kein Fehler, sondern der Preis des Verfahrens: Beim Mitnehmen fragt jeder Punkt, wo
das, was jetzt hier ist, vorher war — und **mittelt** zwischen den vier umliegenden Werten. Diese
Mittelung verwischt bei jedem Schritt ein wenig, und das wirkt exakt wie Zähigkeit. Statt das
kleinzureden, misst das Blatt sie: **3,3·10⁻³**, konstant über alle Zähigkeiten, mit erster
Ordnung schrumpfend.

**Eine Spalte Versatz an der Naht.** Beim zyklischen Rand wickelte ich die Geschwindigkeiten über
die *Feldlänge* — die ist bei u aber Nx+1, weil die erste und die letzte Spalte dieselbe Wand
sind. Gewickelt werden muss über die Zahl der **Zellen**. Der Fehler betraf nur einen schmalen
Streifen an der Naht und war in keiner Messung sichtbar; gefunden habe ich ihn beim Aufschreiben
der Kommentare, nicht beim Rechnen.

**Und die Farbe wurde mehr.** Die Prüfzeile hieß erst „die Farbe bleibt erhalten" und meldete
+12 %. Semi-Lagrangesches Mitnehmen ist **nicht mengenerhaltend**: An einer Staustelle greifen
viele Zellen in dieselbe Spitze, und die Summe wächst. Die Zeile heißt jetzt, was sie misst — und
zeigt, dass der Fehler am Gitter hängt (15 → 8 %) und am Zeitschritt fast gar nicht (12,1 →
12,0 %). Er kommt also aus der räumlichen Mittelung, nicht aus der Zeit. Wer Masse exakt erhalten
will, braucht ein anderes Verfahren.

**Was das Blatt nicht kann:** keine dritte Dimension, keine freien Oberflächen (kein Wasserglas,
kein Tropfen), keine Turbulenzmodelle, keine Wärme und keinen Auftrieb, keine bewegten Hindernisse.
Die Zeitintegration ist erster Ordnung, und die Wandbehandlung am Zylinder ist ein Treppenmuster —
für die Ablösefrequenz reicht das, für Widerstandsbeiwerte nicht. Und der Druckausgleich ist ein
einfacher Iterationslöser; für größere Gitter wäre ein Mehrgitterverfahren die richtige Antwort.

## Technik

Eine einzelne HTML-Datei. Kein Build, keine Bibliothek, nichts verlässt den Browser.
Versetztes Gitter, semi-Lagrangesches Mitnehmen, Druckausgleich mit rot-schwarzer Überrelaxation,
Canvas 2D, hell und dunkel.

## Die ganze Sammlung

Alle Blätter nach Feld geordnet, jedes mit eigenem Repo:
**[ssims437.github.io](https://ssims437.github.io/)**

## Lizenz

MIT
