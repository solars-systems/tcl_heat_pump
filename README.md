# TCL Heat Pump Integration for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![GitHub Release](https://img.shields.io/github/v/release/solars-systems/tcl_heat_pump)](https://github.com/solars-systems/tcl_heat_pump/releases)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

<details open>
<summary><b>English Version</b></summary>

## About This Integration
**TCL Heat Pump** is a dedicated Home Assistant integration providing full monitoring and remote control for **TCL Air-to-Water (ATW) Heat Pumps**, split ACs, and compatible TCL climate devices connected via the TCL Home cloud.

> [!NOTE]
> **Fork & Credits Notice:**  
> This integration is a specialized fork of the [`ha-tcl-home-unofficial-integration`](https://github.com/nemesa/ha-tcl-home-unofficial-integration) developed by **[@nemesa](https://github.com/nemesa)**.  
> Extended and maintained by **[Solars Systems](https://github.com/solars-systems)** to add complete write control, switches, and target temperature regulation for **TCL ATW Heat Pumps**.

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
* **Heating Weather Curve Zone 1 (`htWthCtl1`)**: Select heating weather compensation curve (Curve 1 – 9).
* **Cooling Weather Curve Zone 1 (`clWthCtl1`)**: Select cooling weather compensation curve (Curve 1 – 8).

### 📊 Comprehensive Telemetry & Sensors
* Zone 1 flow temperature (`currentTemp1`)
* Zone 2 & Zone 3 water temperatures
* DHW Tank actual temperature (`currentTempDomestic`)
* Current operating mode & compressor status
* Online / offline connectivity state

---

## Installation via HACS

1. Make sure [HACS](https://hacs.xyz/) is installed in Home Assistant.
2. In HACS, open the three-dot menu in the upper right corner and select **Custom repositories**.
3. Add the repository URL:
   ```text
   https://github.com/solars-systems/tcl_heat_pump
   ```
   * **Type**: `Integration`
4. Click **Add**, find **TCL Heat Pump** in the list, and click **Download**.
5. Restart Home Assistant.
6. Go to **Settings > Devices & Services > Add Integration**, search for **TCL Heat Pump**, and enter your TCL Home account credentials.

---

## Acknowledgements
Special thanks to [@nemesa](https://github.com/nemesa) for creating the initial reverse-engineered TCL Home cloud integration.

</details>

<details>
<summary><b>Versiunea în Limba Română</b></summary>

## Despre această integrare
**TCL Heat Pump** este o integrare completă pentru Home Assistant proiectată special pentru monitorizarea și controlul complet de la distanță al **pompelor de căldură aer-apă TCL (ATW)**, aparatelor de aer condiționat și altor dispozitive compatibile conectate în cloud-ul TCL Home.

> [!NOTE]
> **Notă despre Fork și Merite:**  
> Această integrare este un fork specializat al proiectului [`ha-tcl-home-unofficial-integration`](https://github.com/nemesa/ha-tcl-home-unofficial-integration) realizat de **[@nemesa](https://github.com/nemesa)**.  
> Modificată și întreținută de **[Solars Systems](https://github.com/solars-systems)** pentru a adăuga controlul complet prin comutatoare și reglajul temperaturilor țintă pentru **pompele de căldură TCL ATW**.

---

## Funcționalități

### 🎮 Controlul Pompei de Căldură (Comutatoare & Temperaturi)
* **Comutator Alimentare Zona 1 (`pwrSw1`)**: Pornește sau oprește încălzirea/răcirea pe circuitul Zonei 1.
* **Comutator Apă Caldă Menajeră / ACM (`lvHWtSw`)**: Activează sau dezactivează prepararea apei calde în boiler.
* **Comutator Mod ECO (`ECOStatus`)**: Activează sau dezactivează modul economic.
* **Comutator Rezistență Electrică PTC (`PTC`)**: Comandă rezistența electrică auxiliară de încălzire.
* **Reglaj Temperatură Apă Zona 1 (`htWtTWd1`)**: Setează temperatura țintă pe tur pentru Zona 1 (20°C – 65°C).
* **Reglaj Temperatură Ambientală Cameră Zona 1 (`htEvTGd1`)**: Setează temperatura dorită în cameră / termostat (16°C – 32°C).
* **Reglaj Temperatură Boiler ACM (`lvHWtT`)**: Setează temperatura dorită a apei calde menajere (30°C – 60°C).
* **Comutator Curbă Climatică Zona 1 (`wthCtlSw1`)**: Activează sau dezactivează compensarea climatică după temperatura exterioară.
* **Curbă Climatică Încălzire Zona 1 (`htWthCtl1`)**: Selectează curba de încălzire (Curba 1 – 9).
* **Curbă Climatică Răcire Zona 1 (`clWthCtl1`)**: Selectează curba de răcire (Curba 1 – 8).

### 📊 Senzori și Telemetrie în Timp Real
* Temperatura pe tur a Zonei 1 (`currentTemp1`)
* Temperaturile pe circuitele Zonei 2 și Zonei 3
* Temperatura actuală din boilerul de ACM (`currentTempDomestic`)
* Modul curent de lucru și starea pompei
* Starea conexiunii cloud (online / offline)

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

</details>
