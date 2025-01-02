# 18650 Battery Test Scripts

Dieses Repository enthält zwei Testskripte für 18650 Lithium-Ionen-Batterien:

## 1. Basic Test Script (18650_test.txt)
Ein grundlegendes Testskript für konstante Stromentladung mit:
- Konstante Stromstärke (standardmäßig 1A)
- Spannungsüberwachung
- Temperaturüberwachung
- Kapazitätsberechnung
- Sekundengenaues Datenlogging

## 2. Advanced Test Script (18650_advanced_test.txt)
Ein erweitertes Testskript mit mehreren Phasen:

### Testphasen:
1. **Konditionierung (15 min)**
   - Konstanter Strom bei 1A
   - Stabilisierung der Batterie

2. **Rampe Aufwärts (15 min)**
   - Lineare Erhöhung von 1A auf 2A
   - Sekundengenaue Messungen

3. **Maximallast-Haltephase (15 min)**
   - Konstanter Strom bei 2A
   - Prüfung der Stabilität unter Volllast

4. **Rampe Abwärts (15 min)**
   - Lineare Reduzierung von 2A auf 1A
   - Kontrollierte Lastreduzierung

5. **Finale Entladung**
   - Konstanter Strom bei 1A
   - Bis zum Erreichen der Mindestspannung

### Sicherheitsfunktionen (beide Skripte):
- Spannungsüberwachung (2,5V - 4,2V)
- Temperaturüberwachung (15°C - 50°C)
- Automatische Abschaltung bei Grenzwertüberschreitung
- Kontinuierliches Datenlogging

### Ausgabedaten:
- Spannung
- Strom
- Temperatur
- Kapazität (mAh)
- Testdauer
- CSV-Dateiexport

### Voraussetzungen:
- Batteriespannung > 3,7V zu Testbeginn
- Kalibrierte elektronische Last
- Temperaturmessung
