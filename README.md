# TCL Heat Pump Integration for Home Assistant

<p align="center">
  <img src="logo.png" alt="TCL Heat Pump" width="450">
</p>

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![GitHub Release](https://img.shields.io/github/v/release/solars-systems/tcl_heat_pump)](https://github.com/solars-systems/tcl_heat_pump/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Language / Limbă:**  
[🇬🇧 English Version](#-english-version) • [🇷🇴 Versiunea în Limba Română](#-versiunea-în-limba-română)

---

# 🇬🇧 English Version

## About This Integration
**TCL Heat Pump** is a dedicated Home Assistant custom integration providing full monitoring and remote control for **TCL Air-to-Water (ATW) Heat Pumps** (including R290 and R32 models), split ACs, and compatible TCL climate devices connected via the TCL Home cloud.

> [!NOTE]
> **Fork & Credits Notice:**  
> This integration is a specialized fork of the [`ha-tcl-home-unofficial-integration`](https://github.com/nemesa/ha-tcl-home-unofficial-integration) developed by **[@nemesa](https://github.com/nemesa)**.  
> Extended and maintained by **[Solars Systems](https://github.com/solars-systems)** to add complete write control, switches, target temperature regulation, and weather compensation curves for **TCL ATW Heat Pumps**.

---

## Features

### 🎮 Heat Pump Remote Control (Switches & Regulators)
* **Zone 1 Power Switch (`pwrSw1`)**: Turn Zone 1 space heating/cooling on or off.
* **Domestic Hot Water (DHW / ACM) Switch (`lvHWtSw`)**: Enable or disable domestic hot water tank preparation.
* **ECO Mode Switch (`ECOStatus`)**: Toggle ECO energy-saving mode.
* **PTC Electric Heater Switch (`PTC`)**: Control auxiliary electric booster heater.
* **Target Water Temp Zone 1 (`htWtTWd1`)**: Adjust target flow temperature for Zone 1 heating (20°C – 65°C).
* **Target Room Temp Zone 1 (`htEvTGd1`)**: Set target ambient / room temperature for Zone 1 (16°C – 32°C).
* **Target DHW Water Temp (`lvHWtT`)**: Set target domestic hot water cylinder temperature (30°C – 60°C).
* **Weather Compensation Zone 1 Switch (`wthCtlSw1`)**: Enable / disable automatic weather compensation heating curve.
* **Heating Weather Curve Zone 1 Dropdown (`select.heating_weather_curve_zone_1`)**: Friendly curve selector matching the official TCL ATW R290 manual:
  - **Low Temperature Curves (Underfloor Heating)**: Curves 1 – 8 (flow range 24°C to 38°C) + Curve 9 (Custom Engineering)
  - **High Temperature Curves (Radiators)**: Curves 1 – 8 (flow range 35°C to 55°C) + Curve 9 (Custom Engineering)
* **Heating Weather Curve Slider (`htWthCtl1`)**: Direct numeric curve index (0 – 17).
* **Cooling Weather Curve Slider (`clWthCtl1`)**: Direct numeric cooling curve index (0 – 8).

### 📊 Comprehensive Telemetry & Sensors
* **Smart Weather Curve Sensor (`sensor.atw_heating_curve_1`)**: Displays active curve name with full state attributes (lookup table from -20°C to +20°C, emitter type, and calculated water target).
* **Curve Target Water Temp Zone 1 (`htWtTGd1`)**: The dynamic target flow temperature calculated by the heat pump from the active weather curve.
* **Return Water Temp Zone 1 (`htWtTRr1`)**: Actual return water temperature from the heating circuit.
* **Return Room Temp Zone 1 (`htEvTRr1`)**: Ambient room return air temperature.
* **Zone 1 Flow Temperature (`currentTemp1`)**: Actual flow water temperature.
* **Zone 2 & Zone 3 Water Temperatures**: Secondary and tertiary heating circuits if equipped.
* **DHW Tank Temperature (`currentTempDomestic`)**: Actual domestic hot water storage cylinder temperature.
* **Operating Mode & Compressor State**: Real-time mode (Heating / Cooling / DHW / Auto) and online status.
* **Average Running Power (`Average Running Power`) [W]**: Dynamic average electrical power consumed during operation ($W = \frac{\text{Today Energy [kWh]}}{\text{Today Running Hours}} \times 1000$). Note: Since the TCL Cloud API does not provide a 1-second instantaneous power register, this entity dynamically computes the active running average power (switching to 15W standby when turned off), suitable for linking to power flow dashboards.
* **Total Energy Consumption (`Total Energy Consumption`) [kWh]**: Cumulative lifetime electrical energy consumed by the heat pump.

---

## 📈 Official Weather Compensation Curves Reference

The integration digitizes the complete table from the official TCL ATW R290 manual:  
*Table of Heating Ambient Temperatures - Target Water Temperatures (Unit: °C)*.

### 1. Low Temperature Curves (Underfloor Heating)
| Curve No. | Name in Home Assistant | Ambient $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Emitter Type |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | `Curve 1 Underfloor (32°C - 38°C)` | 38°C | 36°C | 35°C | 33°C | 32°C | Underfloor Heating |
| **1** | `Curve 2 Underfloor (31°C - 37°C)` | 37°C | 35°C | 34°C | 32°C | 31°C | Underfloor Heating |
| **2** | `Curve 3 Underfloor (29°C - 36°C)` | 36°C | 34°C | 33°C | 31°C | 29°C | Underfloor Heating |
| **3** | `Curve 4 Underfloor (28°C - 35°C)` | 35°C | 33°C | 32°C | 30°C | 28°C | Underfloor Heating |
| **4** | `Curve 5 Underfloor (27°C - 34°C)` | 34°C | 32°C | 31°C | 29°C | 27°C | Underfloor Heating |
| **5** | `Curve 6 Underfloor (26°C - 32°C)` | 32°C | 31°C | 29°C | 28°C | 26°C | Underfloor Heating |
| **6** | `Curve 7 Underfloor (25°C - 31°C)` | 31°C | 30°C | 28°C | 27°C | 25°C | Underfloor Heating |
| **7** | `Curve 8 Underfloor (24°C - 29°C)` | 29°C | 28°C | 26°C | 25°C | 24°C | Underfloor Heating |
| **8** | `Curve 9 Underfloor (Custom / Engineering)` | *Custom* | *Custom* | *Custom* | *Custom* | *Custom* | Set in Engineering Menu |

### 2. High Temperature Curves (Radiators / Fan Coils)
| Curve No. | Name in Home Assistant | Ambient $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Emitter Type |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **9** | `Curve 1 Radiators (50°C - 55°C)` | 55°C | 54°C | 52°C | 51°C | 50°C | Radiators / Fan Coils |
| **10** | `Curve 2 Radiators (48°C - 53°C)` | 53°C | 52°C | 50°C | 49°C | 48°C | Radiators / Fan Coils |
| **11** | `Curve 3 Radiators (47°C - 52°C)` | 52°C | 51°C | 49°C | 48°C | 47°C | Radiators / Fan Coils |
| **12** | `Curve 4 Radiators (45°C - 50°C)` | 50°C | 49°C | 47°C | 46°C | 45°C | Radiators / Fan Coils |
| **13** | `Curve 5 Radiators (43°C - 48°C)` | 48°C | 47°C | 45°C | 44°C | 43°C | Radiators / Fan Coils |
| **14** | `Curve 6 Radiators (40°C - 45°C)` | 45°C | 44°C | 42°C | 41°C | 40°C | Radiators / Fan Coils |
| **15** | `Curve 7 Radiators (38°C - 43°C)` | 43°C | 42°C | 40°C | 39°C | 38°C | Radiators / Fan Coils |
| **16** | `Curve 8 Radiators (35°C - 40°C)` | 40°C | 39°C | 37°C | 36°C | 35°C | Radiators / Fan Coils |
| **17** | `Curve 9 Radiators (Custom / Engineering)` | *Custom* | *Custom* | *Custom* | *Custom* | *Custom* | Set in Engineering Menu |

---

## Installation via HACS

1. Make sure [HACS](https://hacs.xyz/) is installed in your Home Assistant instance.
2. In HACS, open the three-dot menu in the upper right corner and select **Custom repositories**.
3. Add the repository URL:
   ```text
   https://github.com/solars-systems/tcl_heat_pump
   ```
   * **Category / Type**: `Integration`
4. Click **Add**, find **TCL Heat Pump** in the list, and click **Download**.
5. Restart Home Assistant.
6. Go to **Settings > Devices & Services > Add Integration**, search for **TCL Heat Pump**, and enter your TCL Home account credentials.

---

## Acknowledgements
Special thanks to [@nemesa](https://github.com/nemesa) for creating the initial reverse-engineered TCL Home cloud integration.

---
---

# 🇷🇴 Versiunea în Limba Română

## Despre această integrare
**TCL Heat Pump** este o integrare completă pentru Home Assistant proiectată special pentru monitorizarea și controlul complet de la distanță al **pompelor de căldură aer-apă TCL (ATW)** (inclusiv modelele cu agent frigorific R290 și R32), aparatelor de aer condiționat și altor echipamente climatice conectate în cloud-ul TCL Home.

> [!NOTE]
> **Notă despre Fork și Merite:**  
> Această integrare este un fork specializat al proiectului [`ha-tcl-home-unofficial-integration`](https://github.com/nemesa/ha-tcl-home-unofficial-integration) realizat de **[@nemesa](https://github.com/nemesa)**.  
> Modificată și întreținută de **[Solars Systems](https://github.com/solars-systems)** pentru a adăuga controlul complet prin comutatoare, reglajul temperaturilor țintă și compensarea climatică (curbe meteo) pentru **pompele de căldură TCL ATW**.

---

## Funcționalități

### 🎮 Controlul Pompei de Căldură (Comutatoare & Temperaturi)
* **Comutator Alimentare Zona 1 (`pwrSw1`)**: Pornește sau oprește încălzirea/răcirea pe circuitul Zonei 1.
* **Comutator Apă Caldă Menajeră / ACM (`lvHWtSw`)**: Activează sau dezactivează prepararea apei calde în boiler.
* **Comutator Mod ECO (`ECOStatus`)**: Activează sau dezactivează modul economic.
* **Comutator Rezistență Electrică PTC (`PTC`)**: Comandă rezistența electrică auxiliară de încălzire.
* **Reglaj Temperatură Apă Tur Zona 1 (`htWtTWd1`)**: Setează temperatura țintă pe tur pentru Zona 1 (20°C – 65°C).
* **Reglaj Temperatură Ambientală Cameră Zona 1 (`htEvTGd1`)**: Setează temperatura dorită în cameră / termostat (16°C – 32°C).
* **Reglaj Temperatură Boiler ACM (`lvHWtT`)**: Setează temperatura dorită a apei calde menajere (30°C – 60°C).
* **Comutator Curbă Climatică Zona 1 (`wthCtlSw1`)**: Activează sau dezactivează compensarea climatică automată după temperatura exterioară.
* **Meniu Selector Curbă Climatică Încălzire Zona 1 (`select.heating_weather_curve_zone_1`)**: Selector prietenos cu toate curbele oficiale din manualul TCL ATW R290:
  - **Curbe Temperatură Joasă (Pardoseală)**: Curbele 1 – 8 (plaje tur între 24°C și 38°C) + Curba 9 (Inginerie)
  - **Curbe Temperatură Înaltă (Radiatoare / Calorifere)**: Curbele 1 – 8 (plaje tur între 35°C și 55°C) + Curba 9 (Inginerie)
* **Slider Numeric Curbă Încălzire (`htWthCtl1`)**: Reglaj direct numeric (0 – 17).
* **Slider Numeric Curbă Răcire (`clWthCtl1`)**: Reglaj direct curbe de răcire (0 – 8).

### 📊 Senzori și Telemetrie în Timp Real
* **Senzor Inteligent Curbă Climatică (`sensor.atw_heating_curve_1`)**: Afișează numele curbei active și expune ca atribute întregul set de date termice (`curve_points`), tipul emițătorului și temperatura calculată pe tur.
* **Temperatură Țintă Calculată din Curbă (`htWtTGd1`)**: Temperatura pe tur calculată dinamic de pompa de căldură pe baza curbei climatice active.
* **Temperatură Retur Apă Zona 1 (`htWtTRr1`)**: Temperatura apei la întoarcerea din instalație.
* **Temperatură Retur Ambiental Cameră Zona 1 (`htEvTRr1`)**: Temperatura de retur aer din încăpere.
* **Temperatura pe tur a Zonei 1 (`currentTemp1`)**: Temperatura apei livrată pe tur.
* **Temperaturile pe circuitele Zonei 2 și Zonei 3**: Circuite auxiliare dacă sunt prezente.
* **Temperatura actuală din boilerul de ACM (`currentTempDomestic`)**: Temperatura curentă din rezervorul de apă caldă menajeră.
* **Stare Compresor și Conexiune**: Modul activ și starea online/offline.
* **Putere Medie de Funcționare (`Average Running Power`) [W]**: Puterea electrică medie absorbită în timpul funcționării active ($W = \frac{\text{Energie Azi [kWh]}}{\text{Ore Funcționare Azi}} \times 1000$). Notă: Deoarece protocolul cloud TCL nu oferă un registru de wați instantaneu secundă de secundă, acest senzor calculează dinamic media puterii de funcționare activă (trecând la 15W standby la oprire), ideal pentru diagramele de flux energetic Solars Systems.
* **Consum Total Energie (`Total Energy Consumption`) [kWh]**: Energia electrică totală cumulată de la punerea în funcțiune.

---

## 📈 Tabelul Oficial de Curbe Climatice TCL ATW R290

Conform manualului oficial TCL ATW R290:  
*Table of Heating Ambient Temperatures - Target Water Temperatures (Unit: °C)*.

### 1. Curbe Temperatură Joasă (Încălzire în Pardoseală)
| Nr. Curbă | Denumire în Home Assistant | Temp. Ext $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Tip Emițător |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | `Curve 1 Underfloor (32°C - 38°C)` | 38°C | 36°C | 35°C | 33°C | 32°C | Pardoseală (Low Temp) |
| **1** | `Curve 2 Underfloor (31°C - 37°C)` | 37°C | 35°C | 34°C | 32°C | 31°C | Pardoseală (Low Temp) |
| **2** | `Curve 3 Underfloor (29°C - 36°C)` | 36°C | 34°C | 33°C | 31°C | 29°C | Pardoseală (Low Temp) |
| **3** | `Curve 4 Underfloor (28°C - 35°C)` | 35°C | 33°C | 32°C | 30°C | 28°C | Pardoseală (Low Temp) |
| **4** | `Curve 5 Underfloor (27°C - 34°C)` | 34°C | 32°C | 31°C | 29°C | 27°C | Pardoseală (Low Temp) |
| **5** | `Curve 6 Underfloor (26°C - 32°C)` | 32°C | 31°C | 29°C | 28°C | 26°C | Pardoseală (Low Temp) |
| **6** | `Curve 7 Underfloor (25°C - 31°C)` | 31°C | 30°C | 28°C | 27°C | 25°C | Pardoseală (Low Temp) |
| **7** | `Curve 8 Underfloor (24°C - 29°C)` | 29°C | 28°C | 26°C | 25°C | 24°C | Pardoseală (Low Temp) |
| **8** | `Curve 9 Underfloor (Custom / Engineering)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

### 2. Curbe Temperatură Înaltă (Radiatoare / Calorifere)
| Nr. Curbă | Denumire în Home Assistant | Temp. Ext $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Tip Emițător |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **9** | `Curve 1 Radiators (50°C - 55°C)` | 55°C | 54°C | 52°C | 51°C | 50°C | Calorifere / Radiatoare |
| **10** | `Curve 2 Radiators (48°C - 53°C)` | 53°C | 52°C | 50°C | 49°C | 48°C | Calorifere / Radiatoare |
| **11** | `Curve 3 Radiators (47°C - 52°C)` | 52°C | 51°C | 49°C | 48°C | 47°C | Calorifere / Radiatoare |
| **12** | `Curve 4 Radiators (45°C - 50°C)` | 50°C | 49°C | 47°C | 46°C | 45°C | Calorifere / Radiatoare |
| **13** | `Curve 5 Radiators (43°C - 48°C)` | 48°C | 47°C | 45°C | 44°C | 43°C | Calorifere / Radiatoare |
| **14** | `Curve 6 Radiators (40°C - 45°C)` | 45°C | 44°C | 42°C | 41°C | 40°C | Calorifere / Radiatoare |
| **15** | `Curve 7 Radiators (38°C - 43°C)` | 43°C | 42°C | 40°C | 39°C | 38°C | Calorifere / Radiatoare |
| **16** | `Curve 8 Radiators (35°C - 40°C)` | 40°C | 39°C | 37°C | 36°C | 35°C | Calorifere / Radiatoare |
| **17** | `Curve 9 Radiators (Custom / Engineering)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

---

## Instalare prin HACS

1. Asigură-te că ai [HACS](https://hacs.xyz/) instalat în Home Assistant.
2. În HACS, deschide meniul din colțul din dreapta-sus (trei puncte) și alege **Custom repositories** (Depozite personalizate).
3. Adaugă URL-ul depozitului:
   ```text
   https://github.com/solars-systems/tcl_heat_pump
   ```
   * **Categorie / Type**: `Integration`
4. Apasă pe **Add**, găsește **TCL Heat Pump** în listă și apasă **Download**.
5. Repornește Home Assistant.
6. Mergi la **Setări > Dispozitive și servicii > Adaugă integrare**, caută **TCL Heat Pump** și introdu datele contului TCL Home.

---

## Mulțumiri
Mulțumiri deosebite lui [@nemesa](https://github.com/nemesa) pentru dezvoltarea inițială a protocolului de comunicare cu cloud-ul TCL Home.
