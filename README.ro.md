# Integrare TCL Heat Pump pentru Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![GitHub Release](https://img.shields.io/github/v/release/solars-systems/tcl_heat_pump)](https://github.com/solars-systems/tcl_heat_pump/releases)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

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
* **Comutator Curbă Climatică Zona 1 (`wthCtlSw1`)**: Activează sau dezactivează compensarea climatică automată după temperatura exterioară.
* **Meniu Selector Curbă Climatică Încălzire Zona 1 (`select.heating_weather_curve_zone_1`)**: Selector prietenos cu toate curbele oficiale din manualul TCL ATW R290:
  - **Curbe Temperatură Joasă (Pardoseală)**: Curbele 1 – 8 (plaje tur între 24°C și 38°C) + Curba 9 (Inginerie)
  - **Curbe Temperatură Înaltă (Radiatoare / Calorifere)**: Curbele 1 – 8 (plaje tur între 35°C și 55°C) + Curba 9 (Inginerie)
* **Slider Numeric Curbă Încălzire (`htWthCtl1`)**: Reglaj direct numeric (0 – 17).
* **Slider Numeric Curbă Răcire (`clWthCtl1`)**: Reglaj direct curbe de răcire (0 – 8).

### 📊 Senzori și Telemetrie în Timp Real
* **Senzor Inteligent Curbă Climatică (`sensor.atw_heating_curve_1`)**: Afișează numele curbei active și atribute detaliate cu întregul tabel de temperaturi (de la -20°C la +20°C), tipul emițătorului (pardoseală vs radiatoare) și temperatura calculată pe tur.
* **Temperatură Țintă Calculată din Curbă (`htWtTGd1`)**: Temperatura pe tur calculată de pompa de căldură pe baza curbei climatice active.
* **Temperatură Retur Apă Zona 1 (`htWtTRr1`)**: Temperatura apei la întoarcerea din instalație.
* **Temperatură Retur Ambiental Cameră Zona 1 (`htEvTRr1`)**: Temperatura de retur aer din încăpere.
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
