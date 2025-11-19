# 18650 Battery Test Scripts

Dieses Repository enthält zwei Testskripte für 18650 Lithium-Ionen-Batterien, inspiriert von und basierend auf den Community-Beiträgen aus dem TestController-Projekt.

📖 **[English Testing Guide Available](TESTING_GUIDE.md)** - Comprehensive guide for testing and qualifying 18650 cells for reuse

## Ursprung und Credits

Diese Scripts basieren auf der Arbeit von **Pukker** und anderen Entwicklern aus der TestController-Community. Die Original-Scripts und weitere nützliche TestController-Ressourcen finden sich hier:
- [TestController User Scripts von Pukker](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html#Battery_test_with_DL24/PX100_loads_by_Pukker)

### Pukker's Original Script (inkludiert)

**Battery Test with IntResistance Check AutosaveTussen.txt** - Das Original-Script von Pukker aus der TestController-Community ist in diesem Repository enthalten. Es bietet erweiterte Funktionen wie:
- Internal Resistance Measurement (0.2C und 1C Tests)
- Autosave-Funktionalität für Charts, CSV und Logs
- Umfangreiche Sicherheitsüberwachung (Temperatur, Spannung, Zeit)
- Flexible Export-Optionen
- Math-Expressions für Power, Capacity, Energy und Load Resistance

**Quelle:** [Pukker's Battery Test Scripts](https://lygte-info.dk/project/TestControllerUserScripts1%20UK.html#Battery_test_with_DL24/PX100_loads_by_Pukker)

## Testskripte

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
