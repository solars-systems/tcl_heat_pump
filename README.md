# TCL Heat Pump Integration for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![GitHub Release](https://img.shields.io/github/v/release/solars-systems/tcl_heat_pump)](https://github.com/solars-systems/tcl_heat_pump/releases)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

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

---

## 📈 Official Weather Compensation Curves Reference

The integration digitizes the complete table from the official TCL ATW R290 manual:  
*Table of Heating Ambient Temperatures - Target Water Temperatures (Unit: °C)*.

### 1. Low Temperature Curves (Underfloor Heating)
| Curve No. | Name in Home Assistant | Ambient $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Emitter Type |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | `Curba 1 Pardoseală (32°C - 38°C)` | 38°C | 36°C | 35°C | 33°C | 32°C | Underfloor / Pardoseală |
| **1** | `Curba 2 Pardoseală (31°C - 37°C)` | 37°C | 35°C | 34°C | 32°C | 31°C | Underfloor / Pardoseală |
| **2** | `Curba 3 Pardoseală (29°C - 36°C)` | 36°C | 34°C | 33°C | 31°C | 29°C | Underfloor / Pardoseală |
| **3** | `Curba 4 Pardoseală (28°C - 35°C)` | 35°C | 33°C | 32°C | 30°C | 28°C | Underfloor / Pardoseală |
| **4** | `Curba 5 Pardoseală (27°C - 34°C)` | 34°C | 32°C | 31°C | 29°C | 27°C | Underfloor / Pardoseală |
| **5** | `Curba 6 Pardoseală (26°C - 32°C)` | 32°C | 31°C | 29°C | 28°C | 26°C | Underfloor / Pardoseală |
| **6** | `Curba 7 Pardoseală (25°C - 31°C)` | 31°C | 30°C | 28°C | 27°C | 25°C | Underfloor / Pardoseală |
| **7** | `Curba 8 Pardoseală (24°C - 29°C)` | 29°C | 28°C | 26°C | 25°C | 24°C | Underfloor / Pardoseală |
| **8** | `Curba 9 Pardoseală (Personalizată)` | *Custom* | *Custom* | *Custom* | *Custom* | *Custom* | Set in Engineering Menu |

### 2. High Temperature Curves (Radiators / Fan Coils)
| Curve No. | Name in Home Assistant | Ambient $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Emitter Type |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **9** | `Curba 1 Calorifere (50°C - 55°C)` | 55°C | 54°C | 52°C | 51°C | 50°C | Radiators / Calorifere |
| **10** | `Curba 2 Calorifere (48°C - 53°C)` | 53°C | 52°C | 50°C | 49°C | 48°C | Radiators / Calorifere |
| **11** | `Curba 3 Calorifere (47°C - 52°C)` | 52°C | 51°C | 49°C | 48°C | 47°C | Radiators / Calorifere |
| **12** | `Curba 4 Calorifere (45°C - 50°C)` | 50°C | 49°C | 47°C | 46°C | 45°C | Radiators / Calorifere |
| **13** | `Curba 5 Calorifere (43°C - 48°C)` | 48°C | 47°C | 45°C | 44°C | 43°C | Radiators / Calorifere |
| **14** | `Curba 6 Calorifere (40°C - 45°C)` | 45°C | 44°C | 42°C | 41°C | 40°C | Radiators / Calorifere |
| **15** | `Curba 7 Calorifere (38°C - 43°C)` | 43°C | 42°C | 40°C | 39°C | 38°C | Radiators / Calorifere |
| **16** | `Curba 8 Calorifere (35°C - 40°C)` | 40°C | 39°C | 37°C | 36°C | 35°C | Radiators / Calorifere |
| **17** | `Curba 9 Calorifere (Personalizată)` | *Custom* | *Custom* | *Custom* | *Custom* | *Custom* | Set in Engineering Menu |

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

---

## 📈 Tabelul Oficial de Curbe Climatice TCL ATW R290

Conform manualului oficial TCL ATW R290:  
*Table of Heating Ambient Temperatures - Target Water Temperatures (Unit: °C)*.

### 1. Curbe Temperatură Joasă (Încălzire în Pardoseală)
| Nr. Curbă | Denumire în Home Assistant | Temp. Ext $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Tip Emițător |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **0** | `Curba 1 Pardoseală (32°C - 38°C)` | 38°C | 36°C | 35°C | 33°C | 32°C | Pardoseală (Low Temp) |
| **1** | `Curba 2 Pardoseală (31°C - 37°C)` | 37°C | 35°C | 34°C | 32°C | 31°C | Pardoseală (Low Temp) |
| **2** | `Curba 3 Pardoseală (29°C - 36°C)` | 36°C | 34°C | 33°C | 31°C | 29°C | Pardoseală (Low Temp) |
| **3** | `Curba 4 Pardoseală (28°C - 35°C)` | 35°C | 33°C | 32°C | 30°C | 28°C | Pardoseală (Low Temp) |
| **4** | `Curba 5 Pardoseală (27°C - 34°C)` | 34°C | 32°C | 31°C | 29°C | 27°C | Pardoseală (Low Temp) |
| **5** | `Curba 6 Pardoseală (26°C - 32°C)` | 32°C | 31°C | 29°C | 28°C | 26°C | Pardoseală (Low Temp) |
| **6** | `Curba 7 Pardoseală (25°C - 31°C)` | 31°C | 30°C | 28°C | 27°C | 25°C | Pardoseală (Low Temp) |
| **7** | `Curba 8 Pardoseală (24°C - 29°C)` | 29°C | 28°C | 26°C | 25°C | 24°C | Pardoseală (Low Temp) |
| **8** | `Curba 9 Pardoseală (Personalizată)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

### 2. Curbe Temperatură Înaltă (Radiatoare / Calorifere)
| Nr. Curbă | Denumire în Home Assistant | Temp. Ext $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Tip Emițător |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **9** | `Curba 1 Calorifere (50°C - 55°C)` | 55°C | 54°C | 52°C | 51°C | 50°C | Calorifere / Radiatoare |
| **10** | `Curba 2 Calorifere (48°C - 53°C)` | 53°C | 52°C | 50°C | 49°C | 48°C | Calorifere / Radiatoare |
| **11** | `Curba 3 Calorifere (47°C - 52°C)` | 52°C | 51°C | 49°C | 48°C | 47°C | Calorifere / Radiatoare |
| **12** | `Curba 4 Calorifere (45°C - 50°C)` | 50°C | 49°C | 47°C | 46°C | 45°C | Calorifere / Radiatoare |
| **13** | `Curba 5 Calorifere (43°C - 48°C)` | 48°C | 47°C | 45°C | 44°C | 43°C | Calorifere / Radiatoare |
| **14** | `Curba 6 Calorifere (40°C - 45°C)` | 45°C | 44°C | 42°C | 41°C | 40°C | Calorifere / Radiatoare |
| **15** | `Curba 7 Calorifere (38°C - 43°C)` | 43°C | 42°C | 40°C | 39°C | 38°C | Calorifere / Radiatoare |
| **16** | `Curba 8 Calorifere (35°C - 40°C)` | 40°C | 39°C | 37°C | 36°C | 35°C | Calorifere / Radiatoare |
| **17** | `Curba 9 Calorifere (Personalizată)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

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
