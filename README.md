# Statistikprojekt: Analyse der Chicago 311 Service Requests im Jahr 2024

Dieses Projekt analysiert die Chicago 311 Service Requests für das Jahr 2024 mit dem Ziel, zeitliche, räumliche und inhaltliche Muster im Meldeverhalten zu identifizieren.  
Der Datensatz umfasst mehrere Millionen Einträge zu unterschiedlichen Service-Request-Typen (SR_TYPE), deren Zeitpunkt, Bearbeitungsdauer, geografische Koordinaten und weiter Spalten (39 Spalten). 

## Installation
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

pd.set_option('display.max_info_columns', 10_000)   # zeigt alle Spaltennamen in info()
pd.set_option('display.max_info_rows', 200_000)     # zeigt Zeileninfo, wenn nötig

#### Pfad zur Datei
path = Path("xxx")

#### Einfacher Import
df = pd.read_csv(path)

## Datenbasis
- Quelle: Chicago 311 Service Requests
- Zeitraum: 01.01.2024 – 31.12.2024
- Umfang: 1,913,929 Einträge

## Übersicht Notebooks

| Name | Beschreibung |
|--------|--------|
| `Overview.ipynb` | Überblick über den Datensatz, Struktur, erste Exploration, Missing Values |
| `Datensatz_Verkuerzung.ipynb` | Vorbereitung und Reduktion des Datensatzes für die Analyse |
| `Created_Date_Verteilung.ipynb` | Zeitliche Verteilung der Meldungen |
| `Created_Date_Hypothesentests.ipynb` | Hypothesentests zu zeitlichen Mustern |
| `Feiertagseffekt_Clean.ipynb` | Analyse des Feiertagseffekts mittels Regression und Anteilsvergleichen |
| `GeoDaten.ipynb` | Räumliche Analyse und Identifikation geografischer Hotspots |
| `Imputation_geodaten.ipynb` | Imputation fehlender geografischer Koordinaten |
| `Bearbeitungszeit_SR_Requests.ipynb` | Analyse der Bearbeitungszeiten für Service Requests |
| `README.md` | Projektübersicht |

## Key Findings


## Analyse: Bearbeitungszeiten der Service Requests

### Fragestellung

Diese Analyse untersucht die **Bearbeitungsdauer** der Chicago 311 Service Requests im Jahr 2024.  
Ziel ist es, die Leistungsfähigkeit des Systems zu bewerten und zentrale Einflussfaktoren auf die Bearbeitungszeit zu identifizieren.

Der analytische Fokus liegt auf **abgeschlossenen (completed) Service Requests**, da nur für diese eine vollständige Bearbeitungszeit vorliegt.

### Datenaufbereitung

- Die Bearbeitungszeit wird als Differenz zwischen `CREATED_DATE` und `CLOSED_DATE` berechnet.
- Eine Plausibilitätsprüfung zeigt **keine negativen Bearbeitungszeiten**.
- Fehlende Werte entstehen ausschliesslich durch **offene Service Requests** und stellen kein Datenqualitätsproblem dar.
- Die Zeitstempel sind chronologisch konsistent und direkt für die Analyse verwendbar.

### Methodik

- Deskriptive Analyse der Bearbeitungszeiten nach Status (`completed`, `open`, `canceled`)
- Untersuchung der Verteilungsform mittels Histogrammen, ECDFs und QQ-Plots
- Verwendung robuster Kennzahlen (Median, Quantile) aufgrund starker Rechtsschiefe
- Gruppenvergleiche:
  - nach Service-Request-Typ
  - nach zuständigem Department
  - nach Wochentag
- Einsatz nichtparametrischer Tests (Kruskal-Wallis, Dunn-Post-hoc)
- Analyse des Zusammenhangs zwischen täglichem Anfragevolumen (Workload) und Bearbeitungszeit
  - explorative OLS-Regression
  - Spearman-Rangkorrelation

### Zentrale Ergebnisse

- Die Bearbeitungszeiten abgeschlossener Service Requests sind **stark rechtsschief verteilt**.
- Der Median der Bearbeitungszeit ist sehr niedrig, während wenige Extremfälle die Verteilung dominieren.
- Abgebrochene Service Requests werden überwiegend sehr früh im Prozess beendet.
- Zwischen Service-Request-Typen bestehen **signifikante Unterschiede** in der Bearbeitungsdauer:
  - kurze Zeiten bei standardisierten und informationsbezogenen Anfragen,
  - lange Zeiten bei Bau-, Infrastruktur- und baumbezogenen Requests.
- Auch zwischen Departments zeigen sich klare Unterschiede, die die Typ-Effekte widerspiegeln.
- Der Median der Bearbeitungszeit ist über die Wochentage hinweg stabil. Unterschiede zeigen sich erst in höheren Quantilen.
- Ein höheres tägliches Anfragevolumen ist mit längeren Bearbeitungszeiten verbunden, der Effekt wird jedoch erst bei sehr hohen Workloads deutlich.


### Interpretation

Die Analyse zeigt ein insgesamt **stabiles und leistungsfähiges System**, bei dem Verzögerungen primär durch **strukturelle Faktoren** verursacht werden.  
Insbesondere komplexe Service-Request-Typen und behördlich regulierte Prozesse treiben lange Bearbeitungszeiten, während zeitliche Effekte wie Wochentage oder saisonale Schwankungen eine untergeordnete Rolle spielen.

Optimierungspotenziale liegen daher weniger in der allgemeinen Prozesssteuerung, sondern gezielt bei einzelnen, besonders zeitintensiven Kategorien.


## Analyse: Feiertagseffekt

### Fragestellung

Die Analyse untersucht, ob sich das Meldeverhalten an **US-Feiertagen** von normalen Tagen unterscheidet.  
Im Fokus stehen zwei Fragen:

- Unterscheidet sich die **Anzahl** der eingehenden Service Requests an Feiertagen?
- Verändert sich die **Zusammensetzung der Service-Request-Typen (SR_TYPE)** an Feiertagen?

### Methodik

Die Daten werden auf **Tagesebene** aggregiert.  
Zur Kontrolle systematischer Effekte werden folgende erklärende Variablen verwendet:

- Dummy-Variablen für **Wochentage**
- Dummy-Variablen für **Monate**
- ein **linearer Zeittrend**

Zunächst wird ein Baseline-Modell ohne Feiertagsvariable geschätzt.  
Anschliessend wird das Modell um einen binären Feiertagsindikator erweitert.

Zusätzlich wird die Zusammensetzung der Meldungen analysiert, indem für jeden Feiertag die **prozentualen Anteile der SR_TYPE** mit deren durchschnittlichen Anteilen an Nicht-Feiertagen verglichen werden.  
Die relative Abweichung wird in einer Heatmap visualisiert.

### Zentrale Ergebnisse

- Das Baseline-Modell erklärt einen grossen Teil der Varianz der täglichen Meldungen (R² ≈ 0.73).
- Durch die Feiertagsvariable steigt die erklärte Varianz weiter an (R² ≈ 0.78).
- Der Feiertagseffekt ist statistisch signifikant, wird jedoch aufgrund der grossen Stichprobe primär über **Effektgrössen** interpretiert.
- Die Zusammensetzung der Service Requests unterscheidet sich an Feiertagen deutlich:  
  Einige SR_TYPE treten überdurchschnittlich häufig auf, andere gehen zurück.

### Interpretation

Feiertage beeinflussen nicht nur die Gesamtzahl der eingehenden Meldungen, sondern auch deren Struktur.  
Die Analyse liefert robuste Hinweise auf systematische Unterschiede im Meldeverhalten an Feiertagen.

---

## Analyse: Geodaten

### Fragestellung

Ziel der Geodatenanalyse ist es, **räumliche Muster** der Service Requests zu untersuchen und zu prüfen, ob sich geografische Hotspots identifizieren lassen.

### Methodik

Verwendet werden die **Latitude- und Longitude-Koordinaten** der Meldungen.  
Zur Visualisierung werden mehrere Darstellungen kombiniert:

- direkte Kartenvisualisierung,
- logarithmische Skalierung zur Abschwächung extremer Dichten,
- Begrenzung der Farbskala (vmax), um dominante Hotspots zu reduzieren.

Extreme Beobachtungen werden **nicht entfernt**, sondern gezielt visualisiert und interpretiert.

Zur Einordnung der Hotspots werden die Koordinaten aggregiert und die **Verteilung der SR_TYPE** an diesen Punkten analysiert.

### Zentrale Ergebnisse

- Die räumliche Verteilung der Meldungen ist stark konzentriert.
- Zwei besonders ausgeprägte Hotspots können identifiziert werden:

**Hotspot 1:**  
Fast ausschliesslich Meldungen des Typs *„311 INFORMATION ONLY CALL“*.  
Da sich an dieser Koordinate das 311 Operations Center sowie das Chicago Department of Public Health befinden, handelt es sich sehr wahrscheinlich um einen **Default-Wert im Erfassungsprozess**.

**Hotspot 2:**  
Dominiert durch *„Aircraft Noise Complaint“*-Meldungen.  
Die Lage nahe dem Flughafen und dem Department of Aviation spricht eher für einen **realen räumlichen Effekt**, auch wenn Default-Koordinaten nicht vollständig ausgeschlossen werden können.

### Interpretation

Die Geodatenanalyse zeigt, dass Hotspots unterschiedliche Ursachen haben können.  
Nicht jeder räumliche Cluster entspricht einem realen Effekt – eine kritische Interpretation ist zwingend erforderlich.  
Der bewusste Umgang mit Ausreissern durch Skalierung und Farbbegrenzung erlaubt es, diese Unterschiede sichtbar zu machen, ohne Daten zu verfälschen.

---

## Zentrale Erkenntnisse 

- Feiertage beeinflussen sowohl die **Häufigkeit** als auch die **Art** der Service Requests.
- Räumliche Hotspots können reale Effekte oder Artefakte der Datenerfassung sein.
- Statistische Signifikanz wird im Kontext der grossen Stichprobe interpretiert.
- Effektgrössen und Visualisierungen sind zentral für die inhaltliche Einordnung.

---

## Hinweise

- Die Notebooks sind kommentiert und einzeln ausführbar.
- Methodische Entscheidungen sind innerhalb der Notebooks dokumentiert (Fallnotizen).

---

## Erweiterung durch Anastasia und Katharina




