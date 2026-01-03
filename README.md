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
- Umfang: 1’913’929 Beobachtungen
- Anzahl Variablen: 39
  
Zentrale untersuchte Variablen:
- Service-Request-Typ (SR_TYPE)
- Zeitstempel (Created / Closed Date)
- Status
- Bearbeitungsdauer
- Geografische Koordinaten
## Projektstruktur & Vorgehen

Das Projekt ist modular aufgebaut. Jedes Notebook bearbeitet eine klar abgegrenzte Fragestellung und ist eigenständig ausführbar.

## 1.⁠ ⁠Datenverständnis & Vorbereitung
- Erste Exploration der Datenstruktur
- Prüfung von Missing Values und Datentypen
- Reduktion des Datensatzes auf das Jahr 2024

## 2. Imputation

## 3. Vertiefung in einzelne Themen

### 3.1 ⁠Zeitliche Analysen
- Untersuchung der zeitlichen Verteilung der Meldungen
- Analyse von Tages-, Wochen- und Monatseffekten
- Durchführung ausgewählter Hypothesentests zu zeitlichen Mustern

### 3.2 Analyse der Bearbeitungsdauer
- Analyse der Bearbeitungszeiten abgeschlossener Service Requests inklusive Verteilungsform und zeitlicher Muster (Saisonalität, Wochentage, Feiertage)
- Vergleich der Bearbeitungszeiten zwischen Service-Request-Typen (SR_TYPE) anhand robuster Kennzahlen (Median, IQR)
- Globaler Gruppenvergleich mittels Kruskal–Wallis-Test mit anschliessender deskriptiver Post-hoc-Analyse

### 3.3⁠ ⁠Feiertagseffekt
- Aggregation der Daten auf Tagesebene
- Modellierung der täglichen Meldungsanzahl unter Kontrolle von Wochentagen, Monaten und Zeittrend
- Ergänzende Analyse der Zusammensetzung der Service-Request-Typen an Feiertagen

### 3.4⁠ ⁠Räumliche Analyse
- Untersuchung der geografischen Verteilung der Meldungen
- Identifikation und Interpretation räumlicher Hotspots
- Bewusster Umgang mit Ausreissern durch Skalierung und Visualisierung

## Übersicht Notebooks

| Name | Creator | Beschreibung | 
|--------|--------|--------|
| `Overview.ipynb` | Anastasia, Katharina, Amelia | Überblick über den Datensatz, Struktur, erste Exploration, Missing Values |
| `Datensatz_Verkuerzung.ipynb` | Anastasia | Vorbereitung und Reduktion des Datensatzes für die Analyse auf das Jahr 2024 |
| `Created_Date_Verteilung.ipynb` | Katharina | Zeitliche Verteilung der Meldungen |
| `Created_Date_Hypothesentests.ipynb` | Katharina | Hypothesentests zu zeitlichen Mustern |
| `Feiertagseffekt_Clean.ipynb` | Amelia | Analyse des Feiertagseffekts mittels Regression und Anteilsvergleichen |
| `GeoDaten.ipynb` | Amelia | Räumliche Analyse und Identifikation geografischer Hotspots |
| `Imputation_geodaten.ipynb` | Katharina | Imputation fehlender geografischer Koordinaten |
| `Bearbeitungszeit_SR_Requests.ipynb` | Anastasia | Analyse der Bearbeitungszeiten für Service Requests |
| `README.md` | Anastasia, Katharina, Amelia | Projektübersicht |


---

## Hinweise

- Die Notebooks sind kommentiert und einzeln ausführbar.
- Methodische Entscheidungen sind innerhalb der Notebooks dokumentiert (Fallnotizen).






