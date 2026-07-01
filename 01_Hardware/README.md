# 📁 Hardware-Entwicklung & Komponenten (Balancer-Modul)

Dieser Ordner enthält die vollständige elektrotechnische Dokumentation, das Platinen-Layout (KiCad) und die Spezifikationen der verwendeten Hardware-Komponenten für das Balancer-Modul (Ball-on-Beam).

## ⚡ Spannungsversorgung & Überwachung
Die primäre Energieeinspeisung der Steuerbox erfolgt über ein **24V-Hauptnetzteil** (Hauptversorgung für den Schrittmotor). Für die Logik- und Sensorpegel wird die Spannung über zwei **DC-DC-Wandler** heruntergeregelt:
* **24V ➡️ 9V:** Versorgung des Servomotors Wippe.
* **24V ➡️ 5V:** Versorgung der Servomotors Vereinzler.

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

Die mechatronischen Komponenten des Moduls teilen sich in sensorische Erfassungsorgane und ausführende Aktoren auf:

* **Sensorik (Erfassung):**
  * **Kugelpositionssensor:** Kontinuierliche Erfassung der exakten Ist-Position der Kugel auf der 1,5 Meter langen Wippe als primäre Regelgröße.
  * **Hallsensor:** Erfasst den aktuellen Neigungswinkel der Wippen-Achse (Winkelmessung), um dem Regler die Orientierung der Bahn zurückzumelden.
  * **ToF-Sensor:** Optische Abstandsmessung via Lichtlaufzeit zur Kugeldetektion im Einlaufbereich des Vereinzeler-Moduls.
  * **Endschalter (Kugelaufnahme / Kugelabgabe):** Mechanische Taster zur sicheren Zustandserkennung, wenn eine Kugel das Modul betritt oder verlässt.

* **Aktorik (Ausführung):**
  * **Schrittmotor + DM556 Treiber:** Der Hauptantrieb zur hochpräzisen und dynamischen Verstellung des Neigungswinkels der Wippen-Linearachse.
  * **Servo-Vereinzeler:** Steuert die mechanische Freigabe der wartenden Kugeln vor dem Eintritt in die Regelstrecke.
  * **Servo-Wippe:** Ein zusätzlicher Stellmotor, welcher direkt an der Wippenmechanik für spezifische Stell- oder Arretieraufgaben zuständig ist.
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
