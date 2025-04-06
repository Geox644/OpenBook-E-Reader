# OpenBook Reader
<br>
Proiectul consta in realizarea unui E-Book Reader accesibil, cu un ecran ce imita hartia, special pentru a avea un consum redus de energie, dar si pentru a oferi utilizatorului o excperinta cat mai placuta. Dispozitivul a fost proiectat de la zero: crearea schemei electrice cu componentele hardware, realizarea 2D PCB + procesul de rutare, trecerea in plan 3D.

<br>

## Diagrama Bloc
![Diagrama bloc](Images/Diagram.png)

<br>

## Descriere functionalitati hardware

### ESP32-C6

* Rol: ESP32-C6 este microcontroller-ul, practic creierul dispozitivului care controleaza toate componentele, gestioneaza si datele.
* Arhitectura: RISC-V 32-bit
* Protocoale: GPIO, I2C, UART, SPI,  PWM
* Memorie:  SD + Flash
* Consum: 10mA (idle), 200mA (WiFi activat)

### MAX17048

* Rol: MAX17048 este monitorul bateriei care vede in timp real nivelul bateriei LiPo si procentul de încarcare.
* Protocol: I2C, SCL: IO6, SDA: IO7

### MCP73831

* Rol: Controloeaza incarcarea eficienta acumulatorul LiPo prin port-ul USB.
* Capacitate: ~500 mA

### XC6204

* Rol: XC6204 reprezinta regulator-ul LDO care reduce tensiunea de la 5V la 3.3V pentru alimentarea sigura a componentelor.

### RTC – DS3231

* Rol: Tine ora exacta chiar si fara alimentare, pentru afisarea corecta a timpului.
* Protocol: I2C, SCL: IO6, SDA: IO7
* Consum: 0.15mA

### BME688 SENSOR

* Rol: Masoara temperatura, umiditatea, etc.
* Protocol: I2C, SCL: IO6, SDA: IO7
* Consum: 3.1mA

### Card SD

* Rol: Salvarea datelor pe dispozitiv.
* Protocol: SPI (pini IO18–IO22)
* Consum: 50mA

### W25Q512JV

* Rol: W25Q512JV este memoria Flash care stocheaza resurse statice folosite de sistemul dispozitivului.
* Protocol: SPI (pini IO18–IO21)
* Consum: 25mA

### Butoane

* Rol: Permite interactiunea manuala a utilizatorului cu dispozitivul.
* Exista 3 tipuri de butoane (Reset, Boot, Change)
* Protocol: GPIO

### E-INK DISPLAY

* Rol: Afisarea continutului/informatiilor de pe dispozitiv. Are un consum redus, deoarece foloseste putine semnale, curentul este necesar atunci cand imaginea se schimba si ramane afisata fara alimentare continua.
* Protocol: GPIO, SPI
* Consum: 40mA

  <br>

## Interfete si pinii de comunicare

| Componente | Interfete | Pini ESP32-C6 |
|-----------|--------------|-----------|
| I2C | BME688, RTC, Battery | SDA (IO22), SCL (IO21) |
| SPI | SD Card, E-paper, NOR Flash | SCK (IO6), MOSI (IO7), MISO (IO2) |
| GPIO | Buttons | IO0 - IO23 |
| USB | Conectare alte dispozitive / incarcare | USB_D+/D- (IO13/IO12) |

<br>

## Observatii

* Modelul 3D al TP-urilor a fost realizat manual de catre mine si au fost pizitionate pe marginea placutei, cat mai aproape de componentele cu care aveau lagaturi
* Au fost facute modificari de footprint pentru U4 – MAX17048G+T10, deoarece alimentarea VBAT a fost proiectata cu o latime de 0.3mm, iar footprint-ul avea o latime a pinilor de 0.27mm.
* Componentele, dimensiunile si pozitiile acestora respecta cat de mult se poate design-ul de referinta.
* ~140 vias PCB
  
<br>

## Bill Of Materials (BOM)

| Componenta | Link | Datasheet |
|-----------|--------------|-----------|
| CAPACITOR 0402 | [Model](https://componentsearchengine.com/part-view/CC0402MRX5R5BB106/YAGEO) | [Datasheet](https://componentsearchengine.com/Datasheets/2/CC0402MRX5R5BB106.pdf) |
| Capacitor 0805| [Model](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/schottky-3165252.pdf) |
| CPH3225A | [Model](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) |
| RCL CPOL 3528 | [Model](https://www.snapeda.com/parts/TAJB475K025RNJ/AVX/view-part/?ref=dk&t=capacitor%203528&con_ref=None) | [Datasheet](https://s3.amazonaws.com/snapeda/datasheet/TAJB475K025RNJ_AVX.pdf) |
| Resistor 0402 | [Model](https://componentsearchengine.com/part-view/R0402%201%25%20100%20K%20(RC0402FR-07100KL)/YAGEO) | [Datasheet](https://www.yageo.com/upload/media/product/products/datasheet/rchip/PYu-RC_Group_51_RoHS_L_12.pdf) |
| MBR0530 DIODE | [Model](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/schottky-3165252.pdf) |
| PGB1010603MR DIODE | [Model](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse%20Inc./datasheet/) |
| BUTTON | [Model](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) | [Datasheet](https://industry.panasonic.com/global/en/downloads?tab=catalog&small_g_cd=203&part_no=EVQPUJ02K) |
| LED 0603 | [Model](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/view-part/?ref=search&t=LED%200603) | [Datasheet](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/datasheet/) |
| BME680 Sensor | [Model](https://www.digikey.ro/en/models/7401317) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) |
| SJ | [Model](https://grabcad.com/library/solder-jumpers-1) | [Datasheet](https://grabcad.com/library/solder-jumpers-1) |
| ESP32C6 Varistor 1812 (PFMF) | [Model](https://ro.mouser.com/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D) | [Datasheet](https://www.snapeda.com/parts/RC0603JR-070RL/Yageo/datasheet/) |
| FH34SRJ-24S-0.5SH (J1) | [Model](https://www.snapeda.com/parts/FH34SRJ-24S-0.5SH(99)/Hirose/view-part/) | [Datasheet](https://www.snapeda.com/parts/FH34SRJ-24S-0.5SH(99)/Hirose%20Connector/datasheet/) |
| USB4110-GF-A (J2) | [Model](https://componentsearchengine.com/part-view/USB4110-GF-A/GCT%20(GLOBAL%20CONNECTOR%20TECHNOLOGY)) | [Datasheet](https://gct.co/files/drawings/usb4110.pdf) |
| QWIIC Connector (J3) | [Model](https://www.snapeda.com/parts/PRT-14417/SparkFun/view-part/) | [Datasheet](https://www.snapeda.com/parts/PRT-14417/SparkFun%20Electronics/datasheet/) |
| 112A-TAAR-R03 (J4)| [Model](https://www.snapeda.com/parts/112A-TAAR-R03/Attend/view-part/) | [Datasheet](https://www.snapeda.com/parts/112A-TAAR-R03/Attend/datasheet/) |
| 744043680 (L1)| [Model](https://ro.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D) | [Datasheet](https://www.we-online.com/components/products/datasheet/744043680.pdf) |
| DMG2305UX-7 (Q1&Q2) | [Model](https://componentsearchengine.com/part-view/DMG2305UX-7/Diodes%20Incorporated) | [Datasheet](https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay%20Siliconix/datasheet/) |
| SI1308EDL-T1-GE3 (Q3) | [Model](https://componentsearchengine.com/part-view/SI1308EDL-T1-GE3/Vishay) | [Datasheet](https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay%20Siliconix/datasheet/) |
| W25Q512JVEIQ (U1) | [Model](https://www.snapeda.com/parts/W25Q512JVEIQ/Winbond+Electronics/view-part/?ref=eda) | [Datasheet](https://www.winbond.com/resource-files/W25Q512JV%20SPI%20RevB%2006252019%20KMS.pdf) |
| ESP32C6 WROOM-1-N8 (U2) | [Model](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) |
| DS3231SN (U3) | [Model](https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/DS3231SN%23/Analog%20Devices/datasheet/) |
| MAX17048G+T10 (U4) | [Model](https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/MAX17048G+T10/Analog%20Devices/datasheet/) |
| MCP73831 Power Management (U5) | [Model](https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/view-part/) | [Datasheet](https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/datasheet/) |
| BD5229G-TR (IC1) | [Model](https://www.digikey.ee/en/models/658502) | [Datasheet](https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd52xxg-e.pdf) |
| XC6220A331MR-G (IC4) | [Model](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) | [Datasheet](https://product.torexsemi.com/system/files/series/xc6220.pdf) |
