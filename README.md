# Automatisiertes Kugelbahn-System – Fokus: Balancer-Modul (Ball-on-Beam)

## Über das Projekt
Dieses Repository dokumentiert ein mechatronisches Folgeprojekt am Labor für Mechatronik und Regelungstechnik der Hochschule Osnabrück. 

Das Gesamtsystem ist als geschlossener Kreislauf konzipiert. Die Kugeln durchlaufen kontinuierlich eine dreiteilige, modular aufgebaute automatisierte Kugelbahn:

1. **Magazin & SmartSort-Modul:** Zu Beginn werden die Billardkugeln in einem Magazin gesammelt und von dort aus einzeln dem Sortierbereich zugeführt. Ein kamerabasiertes KI-System zur Bildverarbeitung erkennt die optischen Merkmale der Kugeln. Anhand dieser Klassifizierung ordnet ein 2-Achs-Handhabungsarm die Kugeln einer von drei Führungen zu.
2. **Rückführungsmodul:** Über diese Führungen rollen die Kugeln weiter zum Rückführungsmodul. Ein mechanischer Lift transportiert die Kugeln vertikal nach oben auf eine höhere Ebene, von wo aus sie eigenständig zum Vereinzeler des darauffolgenden Moduls rollen.
3. **Balancer-Modul (Ball-on-Beam):** Dieses Modul bildet den Kern und Schwerpunkt dieser Arbeit. Nach der gezielten Freigabe der Kugel durch den Vereinzeler gelangt diese auf eine bewegliche Wippe. Die Kugel wird entlang dieser 1,5 Meter langen Achse transportiert, während ein Schrittmotor den Neigungswinkel der Wippe kontinuierlich anpasst.

**Die Kernaufgabe meines Projekts:** Da dieses Kugel-auf-Balken-System inhärent instabil ist, besteht die Herausforderung darin, das System präzise zu regeln. Im Rahmen der Projektphase wird zunächst eine zentrale Steuerbox zur Signalverarbeitung und Motoransteuerung entwickelt (Teil I). Darauf aufbauend wird im Zuge der Bachelorarbeit eine robuste Regelungsstrategie mittels MATLAB-Simulation entworfen und an der realen Anlage in Betrieb genommen (Teil II), um einen dauerhaft fehlerfreien Kreislaufbetrieb zu garantieren.

---

## Repository-Struktur & Inhalt

Dieses Repository ist in funktionale Bereiche unterteilt. Nachfolgend finden Sie eine Übersicht, welche Inhalte und Daten in den jeweiligen Ordnern hinterlegt sind:

### 📁 01_Hardware
Hier befindet sich die vollständige elektrotechnische und mechanische Entwicklung der Steuerbox.
* **Datenblätter:** Technische Spezifikationen aller verwendeten Komponenten (z. B. Schrittmotor, ToF-Sensor, Mikrocontroller).
* **Schaltplan & Layout:** Die in KiCad entwickelten Platinen-Layouts (PCB), Schaltungsentwürfe sowie die Stücklisten (BOM) für die Fertigung.
* **Gehäuse:** CAD- und STL-Dateien für das Gehäuse der Steuerbox (optimiert für den 3D-Druck).

### 📁 02_Software_Firmware
Enthält den gesamten Programmcode für den realen Anlagenbetrieb sowie die Systemeinrichtung.
* **Steuerbox_Firmware:** Der produktive C/C++ Code für den Mikrocontroller, welcher im normalen Regelbetrieb die Sensorwerte einliest und den Motor ansteuert.
* **Kalibrierung:** Spezifische Testskripte zur Sensor- und Aktorkalibrierung, um Messfehler (z. B. des ToF-Sensors) vorab softwareseitig zu kompensieren.

### 📁 03_Modellierung_Simulation
Die theoretische und simulative Basis für den Reglerentwurf.
* **Modellierung:** Mathematische Herleitungen und die physikalischen Differentialgleichungen des Kugel-auf-Balken-Systems.
* **Matlab_Simulink:** `.m`-Skripte und `.slx`-Simulationsmodelle. Hier wird das System virtuell abgebildet, um Reglerstrukturen methodisch zu bedaten und eine Stabilitätsanalyse durchzuführen.
* **Messdaten:** Reale Protokolle und Schwingungsdaten aus der Anlage, die für den Abgleich zwischen Modell und Realität (Systemidentifikation) genutzt werden.

### 📁 04_Regelungsstrategie
Dokumentation und Code-Basis der untersuchten Regelungsansätze.
* Hier sind die verschiedenen evaluierten Regelungskonzepte hinterlegt (z. B. klassischer PID-Regler, Kaskadenregelung oder Zustandsregler). Es wird dokumentiert, wie die Strategien auf Störeinflüsse reagieren (z. B. beim transienten Eintritt der Kugel in das Modul) und welche Strategie die beste Stabilität liefert.

### 📁 05_Dokumentation
Begleitmaterialien und schriftliche Ausarbeitungen.
* Enthält den schriftlichen Projektbericht (Teil I: Steuerbox-Entwicklung & Teil II: Bachelorarbeit-Konzept inklusive Pflichtenheft und Projektplanung) sowie wichtige Systemgrafiken und Schaubilder.
