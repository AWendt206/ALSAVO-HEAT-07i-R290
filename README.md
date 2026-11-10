<div align="center">

# ALSAVO iHEAT · 7" Touch Display

### Eigenes Wärmepumpen-Cockpit für Home Assistant

![Status](https://img.shields.io/badge/Projekt-Vorank%C3%BCndigung-59636e?style=flat-square)
![Hardware](https://img.shields.io/badge/Modell-ALSAVO_HEAT_07i9-343c46?style=flat-square)
![Kältemittel](https://img.shields.io/badge/K%C3%A4ltemittel-R290-485563?style=flat-square)
![Display](https://img.shields.io/badge/Display-Waveshare_7_Zoll-59636e?style=flat-square)
![Plattform](https://img.shields.io/badge/Plattform-Home_Assistant-465d70?style=flat-square)

**Heizen verstehen. Hydraulik sichtbar machen. Energieverbrauch analysieren.**

![Projektankündigung mit realer ALSAVO-Wärmepumpe](assets/projektankuendigung.png)

</div>

> [!IMPORTANT]
> **Projektstatus: Konzeptphase (November 2026).** Diese README ist eine Vorankündigung. Die dargestellten Temperatur- und Energiewerte sind **Beispielwerte**. Es existiert derzeit **keine hier veröffentlichte betriebsfertige Firmware** und keine bestätigte Registerbelegung für eine aktive ALSAVO-Steuerung. Die Entwürfe sind Visualisierungen, keine Screenshots aus dem Livebetrieb.

## Warum dieses Projekt?

Die **ALSAVO HEAT 07i9** ist eine R290-Luft/Wasser-Monoblock-Wärmepumpe mit nominell rund 7 kW Heizleistung. Das Projekt soll ihre technischen Betriebsdaten unabhängig und lokal in **Home Assistant** verfügbar machen und auf einem eigenen **7-Zoll-Touchdisplay** als grafisches Cockpit darstellen.

Der Schwerpunkt liegt auf dem **Heizbetrieb**: Vor- und Rücklauf, Spreizung, Verdichter, Abtauung, Heizkreise, Hydraulik, Stromverbrauch und perspektivisch die Effizienzbewertung. Die Warmwasserbereitung erfolgt in der beschriebenen Installation über eine **separate Brauchwasserwärmepumpe** und gehört daher nicht zur Hauptanzeige dieser ALSAVO-Oberfläche.

Optisch orientiert sich das Konzept an meinem Avarma-BWWP-Displayprojekt, verwendet hier jedoch ein eigenständiges **Graphit-/Silbergrau-Design** mit dezenten Akzenten für Wärme (Bernstein) und Rücklauf/Kälte (Eisblau).

## Reale Anlage und Hardware

![Foto der installierten ALSAVO HEAT 07i9](assets/alsavo_heat_07i9_real.jpg)

**Praxisaufbau (vereinfacht):**

- **Wärmepumpe:** ALSAVO HEAT 07i9, R290-Monoblock, 7-kW-Klasse
- **Erdgeschoss:** Fußbodenheizung mit Mischer und eigener Umwälzpumpe
- **Obergeschoss:** Heizkörperkreis
- **Pufferspeicher:** NAD 200 l als Reihenpuffer im Rücklauf
- **Erweiterung:** zusätzlicher Pufferspeicher als mögliche spätere Ausbaustufe
- **Datenplattform:** Home Assistant
- **Busanbindung:** RS485-/Protokollanalyse geplant, konkrete Nutzbarkeit noch zu prüfen
- **Touchpanel:** vorgesehen ist ein Waveshare ESP32-S3-Touch-LCD-7B mit 1024 × 600 Pixeln; ESPHome/LVGL als bevorzugter technischer Ansatz, abhängig von der Praxistauglichkeit

## Erste Displaykonzepte

Die folgenden Ansichten sind **statische UI-Entwürfe in der nativen Zielauflösung 1024 × 600**. Die Texte und Messwerte sollen zeigen, wie die fertige Oberfläche wirken könnte.

![Vier Displayansichten im grauen Design](assets/konzeptuebersicht.png)

### 1 · Betriebsübersicht

![Betriebscockpit für die ALSAVO iHEAT](assets/uebersicht.png)

**Geplant:** Vorlauf, Rücklauf, Temperaturspreizung, Außentemperatur, Sollwert, Betriebszustand und Status von Verdichter, Abtauung und Heizkreispumpe. Die Zustände **Heizen**, **Bereitschaft**, **Abtauung** und **Störung** sollen klar unterscheidbar werden.

### 2 · Hydraulik und Heizkreise

![Vereinfachtes Hydraulikschema mit FBH, Heizkörpern und Reihenpuffer](assets/hydraulik.png)

**Geplant:** schematische Darstellung des Vor- und Rücklaufs, der Fußbodenheizung im Erdgeschoss, des Heizkörperkreises im Obergeschoss sowie des Reihenpuffers. Die Anzeige ist ein **Funktionsschema, keine hydraulische Montage- oder Ausführungszeichnung**.

### 3 · Analyse und Diagnose

![ALSAVO Diagnose- und Kommunikationsansicht](assets/analyse.png)

**Geplant:** Temperaturverläufe, Verdichterfrequenz, Abtauereignisse, Verdichterstarts, Laufzeiten und Fehlerdiagnose, **soweit die Datenquellen tatsächlich verfügbar sind**. Fehlende Register werden ausdrücklich als nicht verfügbar markiert, nicht künstlich gefüllt.

### 4 · Energie und Effizienz

![Energieansicht mit Stromverbrauch und Home-Assistant-Daten](assets/energie.png)

**Geplant:** elektrische Momentanleistung, Tages-/Monatsverbrauch, Lastverlauf, PV-Überschuss und Batteriespeicherstatus. COP-/Arbeitszahlwerte werden nur dann angegeben, wenn neben der elektrischen Leistung auch belastbare thermische Messwerte oder hinreichend transparente Schätzverfahren vorliegen.

### Optionale 3D-Designstudie

![Konzeptionelle Geräteansicht des Touchpanels](assets/designstudie_geraeteansicht.png)

*Zusätzliche grafische Designstudie; alle dort gezeigten Werte und Bedienelemente sind illustrativ.*

## Geplante Systemarchitektur

```mermaid
flowchart TB
    A[ALSAVO HEAT 07i9<br/>Originalsteuerung] --> B[RS485 / Protokollanalyse<br/>zunächst ohne Schreibzugriffe]
    B --> C[ESP32-S3<br/>Busmonitor / Datenschnittstelle]
    C --> D[Home Assistant]
    E[Stromzähler] --> D
    F[PV / Batteriespeicher] --> D
    D --> G[Waveshare 7-Zoll Touchdisplay<br/>ESP32-S3 / LVGL]
```

**Wichtig:** Das Schema beschreibt die angestrebte Architektur. Eine funktionierende und freigegebene ALSAVO-RS485-/Modbus-Anbindung wird hier **nicht vorweggenommen**. Der vorhandene Regler soll zunächst unverändert bleiben. Aktive Schreibzugriffe und Parameteränderungen sind gesondert zu verifizieren und sicherheitstechnisch zu bewerten.

## Geplante Funktionen

| Bereich | Ziel |
|---|---|
| Betrieb | Klarer Heiz-, Bereitschafts-, Abtau- und Fehlerstatus |
| Temperaturen | Vorlauf, Rücklauf, Spreizung, Außentemperatur und Sollwert |
| Heizkreise | FBH EG, Heizkörper OG, Umwälzpumpen und Mischer, soweit messbar |
| Analyse | Verdichterstarts, Laufzeiten, Abtauungen und Zeitreihen |
| Energie | Elektrischer Verbrauch, PV-Daten und optional Effizienzkennzahlen |
| Bedienung | Spätere Touch-Steuerung nur für bestätigte und zulässige Funktionen |
| Wartung | Kommunikationsstatus, Ausfälle und Sensor-Plausibilität |

## Roadmap

- [x] Reale ALSAVO HEAT 07i9 als Projektgrundlage dokumentiert
- [x] Eigenständiges graues UI-Design für vier Displayseiten erstellt
- [x] Hydraulikschema der Heizkreise als Konzept visualisiert
- [x] GitHub-Vorankündigung mit realem Gerätefoto vorbereitet
- [ ] Display-Hardware und ESPHome-/LVGL-Kaltstartverhalten testen
- [ ] ALSAVO-Schnittstelle und Busprotokoll zuverlässig identifizieren
- [ ] Lesende Datenanbindung an Home Assistant nachweisen
- [ ] Echte Sensorwerte und Energiezähler zuordnen
- [ ] Betriebszustände und Abtauereignisse validieren
- [ ] Bedienung und Parametrierung nur nach geprüfter Freigabe entwickeln
- [ ] Langzeittest, Dokumentation und erste veröffentlichbare Version

## Technische Hinweise

Die Vorankündigung ist **kein Schaltplan und keine Anleitung zum Eingriff in das Kältesystem**. Die R290-Anlage enthält ein brennbares Kältemittel und elektrische Komponenten. Änderungen an Steuerung oder elektrischen Anschlüssen dürfen die Schutzfunktionen des Herstellers nicht beeinträchtigen. Einstellungen, Messungen und Eingriffe an sicherheitsrelevanten Komponenten gehören in qualifizierte Hände.

**Datenqualität geht vor Grafik:** Wo Register fehlen, Sensoren unbekannt sind oder berechnete Arbeitszahlen nicht validiert wurden, zeigt die spätere Oberfläche einen entsprechenden Hinweis an.

## Links und Quellen

- [Hersteller: ALSAVO HEAT 07i9](https://www.alsavo.com/de/products/alsavo-r290-7kw-air-source-heat-pump-monoblock-inverboost-multifunctional-hot-water)
- [Heat Pump KEYMARK – ALSAVO HEAT 07i9](https://www.heatpumpkeymark.com/en/?cHash=70cd73849c9959f126bb721de04cb8f7&tx_pskeymark_frontend%5Baction%5D=generatePdf&tx_pskeymark_frontend%5Bcontroller%5D=Frontend&tx_pskeymark_frontend%5Bsubtype%5D=5571&type=109126)
- [ESPHome](https://esphome.io/) · [Home Assistant](https://www.home-assistant.io/)
- [Entwicklerprofil AWendt206](https://github.com/AWendt206)

---

**Unabhängiges Community-Projekt.** Keine offizielle Software von ALSAVO, Waveshare, ESPHome oder Home Assistant. Fortsetzung und erste technische Ergebnisse folgen in diesem Repository.
