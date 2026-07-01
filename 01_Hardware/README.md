# 📁 Hardware-Entwicklung & Komponenten (Balancer-Modul)

Dieser Ordner enthält die vollständige elektrotechnische Dokumentation, das Platinen-Layout (KiCad) und die Spezifikationen der verwendeten Hardware-Komponenten für das Balancer-Modul (Ball-on-Beam).

## ⚡ Spannungsversorgung & Überwachung
Die primäre Energieeinspeisung der Steuerbox erfolgt über ein **24V-Hauptnetzteil** (Hauptversorgung für den Schrittmotor). Für die Logik- und Sensorpegel wird die Spannung über zwei **DC-DC-Wandler** heruntergeregelt:
* **24V ➡️ 9V:** Versorgung der Mikrocontroller (VIN).
* **24V ➡️ 5V:** Versorgung der Sensorik, Servos und Peripherie.

**Spannungsüberwachung:** Die drei Spannungsschienen (24V, 9V, 5V) werden permanent überwacht. Da die ESP32-Eingänge nur maximal 3,3V tolerieren, sind präzise **SMD-Widerstände als Spannungsteiler** vorgeschaltet.

---

## 🧠 Dual-Controller-Architektur
Um harte Echtzeit-Regelung und Netzwerkkommunikation sauber zu trennen, basiert die Steuerbox auf zwei miteinander kommunizierenden ESP32-Modulen:

### 1. ESP32-S3-ETH (Host- & Kommunikations-Controller)
* **Aufgabe:** Übergeordnete Steuerung, Ethernet-Kommunikation mit den anderen Kugelbahn-Modulen (SmartSort / Rückführung) und Statusüberwachung.
* **Peripherie:** * Ansteuerung eines **TFT-Displays** zur Status- und Fehleranzeige.
  * Analoge Überwachung der Spannungsversorgung (5V, 9V, 24V).
  * Status-LEDs (inkl. Vorwiderständen) und 2x **Boot-Taster** für manuelle Eingriffe.

### 2. ESP32-DevKitC (Regelungs-Controller)
* **Aufgabe:** Läuft im **MATLAB/Simulink-Kompatibilitätsmodus**. Er liest in festen Zeitschritten die Sensorwerte ein, berechnet den Regelalgorithmus und steuert die Aktoren direkt an.
* **Kommunikation:** Direkter Datenaustausch mit dem ESP32-S3-ETH zur Zustandssynchronisation.

---

## 🛠️ Sensorik & Aktorik (Komponentenübersicht)

Die Komponenten sind entlang des physikalischen Kugelverlaufs auf der 1,5 Meter langen Wippe aufgeteilt:

* **Kugel-Vereinzelung:**
  * **ToF-Sensor (Vereinzeler):** Optische Kugeldetektion im Einlauf via Lichtlaufzeit.
  * **Servo-Vereinzeler:** Gibt die Kugel mechanisch für die Wippe frei.
* **Die Wippen-Achse:**
  * **Schrittmotor + DM556 Treiber:** Leistungsstarker Antrieb zur dynamischen Anpassung des Wippen-Neigungswinkels.
  * **Kugelpositionssensor:** Kontinuierliche Positionsbestimmung der Kugel auf der Wippe (Ist-Wert für den Regler).
  * **Servo-Wippe:** Optionale mechanische Unterstützung/Arretierung der Achse.
* **Sicherheit & Begrenzung:**
  * **Endschalter (Kugelaufnahme / -abgabe):** Registrieren mechanisch den Eintritt und das Verlassen der Kugel an den Bahnenden.

---

## 📋 Datenblatt-Verzeichnis (Links)

Alle originalen Hersteller-Datenblätter sind im Unterordner `01_Datenblaetter/` hinterlegt:

| Komponente | Funktion im System | Dokument (Lokal) |
| :--- | :--- | :--- |
| **ESP32-S3-ETH** | Host, Ethernet & Display | [Datasheet_ESP32-S3-ETH.pdf](01_Datenblaetter/Datasheet_ESP32-S3-ETH.pdf) |
| **ESP32-DevKitC** | Simulink Echtzeit-Regler | [Datasheet_ESP32_DevKitC.pdf](01_Datenblaetter/Datasheet_ESP32_DevKitC.pdf) |
| **DM556** | Schrittmotortreiber | [Manual_DM556_Driver.pdf](01_Datenblaetter/Manual_DM556_Driver.pdf) |
| **ToF-Sensor** | Einlauf-Detektion | [Datasheet_ToF_Sensor.pdf](01_Datenblaetter/Datasheet_ToF_Sensor.pdf) |
| **DC-DC Wandler** | Spannungsregelung 5V / 9V | [Datasheet_DCDC_Converter.pdf](01_Datenblaetter/Datasheet_DCDC_Converter.pdf) |

---

## 📂 Ordnerinhalt
* `/01_Datenblaetter`: PDFs aller mechatronischen Komponenten.
* `/02_Schaltplan_Layout`: KiCad-Projektdateien, Schaltpläne (Schaltplan-Review), Gerber-Dateien für die Platinenfertigung und Stückliste (BOM).
* `/03_Gehaeuse`: CAD-Modelle und STL-Dateien für den 3D-Druck der Steuerbox.
