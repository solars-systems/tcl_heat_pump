# Integrare TCL Heat Pump pentru Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![GitHub Release](https://img.shields.io/github/v/release/solars-systems/tcl_heat_pump)](https://github.com/solars-systems/tcl_heat_pump/releases)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

**Limbă / Language:**  
[🇷🇴 Versiunea în Limba Română](#despre-această-integrare) • [🇬🇧 English Version (README.md)](README.md)

---

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
* **Slider Numeric Curbă Încălzire (`htWthCtl1`)**: Reglaj direct numeric (1 – 18).
* **Slider Numeric Curbă Răcire (`clWthCtl1`)**: Reglaj direct curbe de răcire (1 – 8).

### 📊 Senzori și Telemetrie în Timp Real
* **Senzor Inteligent Curbă Climatică (`sensor.atw_heating_curve_1`)**: Afișează numele curbei active și expune ca atribute întregul set de date termice (`curve_points`), tipul emițătorului și temperatura calculată pe tur.
* **Temperatură Apă pe Tur Reală (`sensor.water_flow_temp_zone_1` / `htWtTRr1`)**: Temperatura reală a apei pe TUR livrată în instalația de încălzire.
* **Temperatură Retur Apă Estimată (`sensor.estimated_return_water_temp_zone_1`)**: Temperatură de retur estimată calculată automat cu 4°C mai mică decât turul apei ($\Delta T \approx 4^\circ\text{C}$), ideală pentru monitorizarea circuitului de încălzire.
* **Temperatură Țintă pe Tur din Curbă (`sensor.curve_target_flow_temp_zone_1` / `htWtTGd1`)**: Temperatura pe tur calculată dinamic de pompa de căldură pe baza curbei climatice active.
* **Temperatură Ambientală Cameră (`sensor.room_temperature_zone_1` / `currentTemp1`)**: Temperatura ambientală reală măsurată în încăpere (living).
* **Temperatură Țintă Cameră (`htEvTGd1`)**: Temperatura ambientală dorită setată pentru încăpere.
* **Temperatură Referință Ambientală (`htEvTRr1`)**: Temperatura de referință a camerei.
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
| **1** | `Curba 1 Pardoseală (32°C - 38°C)` | 38°C | 36°C | 35°C | 33°C | 32°C | Pardoseală (Low Temp) |
| **2** | `Curba 2 Pardoseală (31°C - 37°C)` | 37°C | 35°C | 34°C | 32°C | 31°C | Pardoseală (Low Temp) |
| **3** | `Curba 3 Pardoseală (29°C - 36°C)` | 36°C | 34°C | 33°C | 31°C | 29°C | Pardoseală (Low Temp) |
| **4** | `Curba 4 Pardoseală (28°C - 35°C)` | 35°C | 33°C | 32°C | 30°C | 28°C | Pardoseală (Low Temp) |
| **5** | `Curba 5 Pardoseală (27°C - 34°C)` | 34°C | 32°C | 31°C | 29°C | 27°C | Pardoseală (Low Temp) |
| **6** | `Curba 6 Pardoseală (26°C - 32°C)` | 32°C | 31°C | 29°C | 28°C | 26°C | Pardoseală (Low Temp) |
| **7** | `Curba 7 Pardoseală (25°C - 31°C)` | 31°C | 30°C | 28°C | 27°C | 25°C | Pardoseală (Low Temp) |
| **8** | `Curba 8 Pardoseală (24°C - 29°C)` | 29°C | 28°C | 26°C | 25°C | 24°C | Pardoseală (Low Temp) |
| **9** | `Curba 9 Pardoseală (Personalizată)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

### 2. Curbe Temperatură Înaltă (Radiatoare / Calorifere)
| Nr. Curbă | Denumire în Home Assistant | Temp. Ext $\le -20^\circ\text{C}$ | $-10^\circ\text{C}$ | $0^\circ\text{C}$ | $+10^\circ\text{C}$ | $\ge 20^\circ\text{C}$ | Tip Emițător |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **10** | `Curba 1 Calorifere (50°C - 55°C)` | 55°C | 54°C | 52°C | 51°C | 50°C | Calorifere / Radiatoare |
| **11** | `Curba 2 Calorifere (48°C - 53°C)` | 53°C | 52°C | 50°C | 49°C | 48°C | Calorifere / Radiatoare |
| **12** | `Curba 3 Calorifere (47°C - 52°C)` | 52°C | 51°C | 49°C | 48°C | 47°C | Calorifere / Radiatoare |
| **13** | `Curba 4 Calorifere (45°C - 50°C)` | 50°C | 49°C | 47°C | 46°C | 45°C | Calorifere / Radiatoare |
| **14** | `Curba 5 Calorifere (43°C - 48°C)` | 48°C | 47°C | 45°C | 44°C | 43°C | Calorifere / Radiatoare |
| **15** | `Curba 6 Calorifere (40°C - 45°C)` | 45°C | 44°C | 42°C | 41°C | 40°C | Calorifere / Radiatoare |
| **16** | `Curba 7 Calorifere (38°C - 43°C)` | 43°C | 42°C | 40°C | 39°C | 38°C | Calorifere / Radiatoare |
| **17** | `Curba 8 Calorifere (35°C - 40°C)` | 40°C | 39°C | 37°C | 36°C | 35°C | Calorifere / Radiatoare |
| **18** | `Curba 9 Calorifere (Personalizată)` | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | *Configurabil* | Setare Meniu Inginerie |

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
