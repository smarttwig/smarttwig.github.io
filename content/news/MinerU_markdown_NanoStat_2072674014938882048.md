---
title: NanoStat
date: 2025-10-15
category: 公司新闻
color: green
description: 北京杪荟枝农业科技有限公司于2025年10月正式成立，致力于为智慧农业领域提供创新解决方案，推动农业数字化转型。
image: /images/news/news1.png
---

# NanoStat: An open source, fully wireless potentiostat

![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/e7e4be0a62a60d64e536129cf6bb3009cbc3e67e30cd75dbf6dfc9f789534f69.jpg)


Shawn Chia-Hung Lee <sup>a</sup>, Peter J. Burke <sup>a,b,*</sup> 

<sup>a</sup> Department of Biomedical Engineering, University of California, Irvine, CA 92697, USA 

<sup>b</sup> Department of Electrical and Engineering and Computer Science, University of California, Irvine, CA 92697, USA 

## A R T I C L E I N F O

Keywords: Potentiostat Microcontroller Electrochemistry 

## A B S T R A C T

We present an open source, fully wireless potentiostat (the “NanoStat”) for applications in electrochemistry, sensing, biomedical diagnostics, and nanotechnology, based on only 2 integrated circuit chips: A digital mi crocontroller with integrated on board WiFi and file/web server hardware/software, and an analog front end This versatile platform is fully capable of all modern electrochemisty assays, including cyclic voltammetry, square wave voltammetry, chronoamperometry, and normal pulse voltammetry. The user interface is a web browser connected over http. All the code (firmware, HTML5, JavaScript) is hosted by the NanoStat itself without the need for any additional software. The total size is 4 × 40 × 20 mm and battery operation for 6 h is demonstrated, possible to extend to weeks or months in sleep mode. We anticipate that the applications of this could be very broad, from biomedical sensing in the clinic, to remote monitoring of unattended “motes”, to even possibly sensing aerial pathogens such as COVID in large public spaces without the need for anything other than a web browser for remote monitoring from anywhere in the world. Finally, we propose to use this software suite as a basis (kernel) of a fully open source, general purpose, web based electrochemistry software suite, abstracted from the hardware, which we call “OpenEChem". 

## 1. Introduction

Electrochemcal techniques interface the world of electricity to the world of chemistry utilizing a mature, broad, and powerful suite of tools for quantitative analysis of composition of analytes for sensing and biomedical diagnostics [1]. These include DNA, RNA, proteins, aptam ers, peptides lipids, metabolites, small molecules, trace metals, and even virions, such as the COVID19 virion. The simplest of the techniques require only resistance measurement whereas the most sophisticated techniques utilize signal processing and precise, coordinated timing of voltage and current waveforms (such as steps, pulses, etc. used in cyclic voltammetry, square wave voltammetry, chronoamperometry, and normal pulse voltammetry) with ultra low noise and ultra high gain analog circuitry to extract every possible piece of information about the analyte of interest. 

While typical tools are benchtop standalone instruments costings thousands to tens of thousands of dollars, recent trends in the literature have used smaller and smaller custom boards with fewer and fewer components [2]. In the extreme, all signal processing and data storage in the cloud is handled off-board, with the potentiostat only doing the most low level work, the digitized signals immediately broadcast as unprocessed digitized voltages for could based processing on custom software on wired or wireless linked PCs or smartphones. This often is sold as “wearable” electronics, although the battery is still an issue, and an external processor with signal processing capability, memory, and storage capacity is required, typically referred to ambiguously as “the cloud”. The control machine is either a smartphone of PC. It is advertised as “the cloud” because the PC /smartphone is connected directly to the internet, but the electrochemical sensor is NOT. While this technology definitely has its place and advantages, this is a bit misleading, because all smartphones and PCs are internet connected these days, so to call the electrochemical sensor connected to a PC or smartphone “cloud con nected” is almost a tautology. 

Furthermore, this approach requires custom, often proprietary soft ware specific to the operating system of the off-chip architecture (e.g. Windows, Linux, Android, iOS, etc.). The trend has been to use the lowest possible architecture on the sensing chip such as 8 bit Atmega or at best 32 bit ARM (STM32) microcontrollers, with minimal on board intelligence, memory for at most a few traces, no internet connectivity (only Bluetooth, which is peer to peer and requires custom software) and little to no ability to serve files or graphical representation of data. 

Many or most such solutions are proprietary, expensive, and closed source. The only open source solutions (reviewed in detail later in this paper) require the user to source the parts and separately solder them manually or place them manually and reflow solder them in their own labs. Both of these are laborious and require special skills. Manual pick and place is prone to error as the size of the components gets smaller, limiting the miniaturization potential. The alternative approach pre sented in this paper is to employ automated pick and place machines that, unlike human hands, can handle extremely small surface mount (SMD) parts as well as employ automated reflow soldering, which is more economical and can be ordered and manufactured from vendors at low cost without the need for any manual assembly at all. The boards come shipped to the user ready to operate “out of the box”. Finally, the CAD files are based on expensive, commercial, proprietary software that needs to be installed on a desktop PC for design changes. 

In this work (Fig. 1), we present a fully open source hardware and software design for a potentiostat (the “NanoStat” that 1) can serve all the results via a web page hosted on board the microcontroller itself 2) can be connected to from the internet from anywhere in the world via a web browser and 3) contains only 2 key integrated circuits (ICs) other than the voltage regulator, battery management, and USB ICs and a handful of discrete components: (a) a single microcontroller with on board WiFi, memory and signal processing with a complete file system and (b) a single chip analog front end. The entire system fits on a 20 × 40 mm board, with battery operation can run 6 h in active mode and even longer in sleep mode. As such, this is the world’s smallest webserver (WiFi) enabled potentiostat. The cost is ultra low, under 25 dol lars in small quantities fully assembled with no soldering needed. The CAD designs are also provided on a free, web based CAD program accessible from anywhere in the world with only a web browser. 

We anticipate that the application of this could be very large, from biomedical sensing in the clinic, to remote monitoring of unattended “motes”, to even possibly sensing aerial pathogens such as COVID in large public spaces without the need for anything other than a web browser for remote monitoring from anywhere in the world. Further more, any lab with a modest budget, including garage DIY, K-12 edu cation, and modern university labs that need sophisticated electrochemical analysis with minimal investment of time and money, can benefit from this work. Finally, we propose to use this software suite as the kernel of a future open source, general purpose electrochemistry suite. Similar to how Linux is an open source operating system for modern PCs, abstracted from the hardware, we propose an open source project for electrochemistry, also abstracted from the hardware, and based on our initial foundational web-based work presented in this paper, which we call “OpenEChem”. 

## 2. System design

In this section we describe the overall design. Both the hardware and software source code are available on as S.I. and on a github repository as https://github.com/PeterJBurke/Nanostat. 

## 2.1. Hardware design

## 2.1.1. Hardware architecture

The hardware architecture (Fig. 2) is based on two core integrated circuits (ICs): The ESP32 (Espressif Systems) IC has integrated WiFi, 520 KiB SRAM, 8 bit DAC, 12 bit ADC, and dual core 32 bit processors operating at 240 MHz. We use the SOC version (ESP32-PICO-D4) which has integrated crystal oscillator and 4 MByte of integrated SPI flash memory. Note that this is the key enabling technology of this work, as it contains processing power and connectivity orders of magnitude improved over prior microcontroller based potentiostats. (See “Prior Art” discussion later in this paper). 

The analog front end (AFE) is an Analog Devices LMP91000, which has an integrated analog potentiostat to enable bipolar (both sign) cell operation from a single sign power supply. A digital (I2C) bus between the LMP91000 and the ESP32 controls the internal gain settings and other configurations of the potentiostat. Additional on board circuitry includes a chip for USB interface (which is not required if fully wireless operation is used), and a chip for battery power management to charge the battery from a USB connector, similar to a cell phone charger. An on board LED provides power status and a separate LED blinks every time a data point is taken. Fig. 7 shows the circuit board rendered. The number of distinct components is 29; the total number of components is 42. 

![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/37c84979012d58d0e3f994347667c7ff7184a29b93930e477bde54fd061731ca.jpg)



Fig. 1. NanoStat: A versatile wireless platform for electrochemistry, from traditional analytical chemistry to advanced nanotechnology based sensors (e.g. Datta et al. [3]), using only a web browser for access to a broad suite of software defined analytical tools.


![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/6c1dbc3927ed31427940046bfc9a44f3df542ada8ebddec54cfb3ac74e4877e2.jpg)



Fig. 2. 2 chip architecture schematic. One of the chips provides memory, firmware, WiFi connectivity, and hosts the website and file system for the user interface and analysis. The other chip provides the analog front end (AFE).


## 2.1.2. Hardware CAD

The CAD file is designed using a web based CAD program called EasyEDA (https://easyeda.com/editor). In addition to providing web functionality usable from anywhere in the world, it has an integrated library of parts that can be assembled by the manufacturer, with real time information about stock status, something that is increasingly important in the modern chip shortage era. The CAD files, Gerber files, bill of materials, and pick and place instructions are provided in the online repository (https://github.com/PeterJBurke/Nanostat). These are sufficient for any user to have the board manufactured from their preferred vendor. In this project, the vendor JLCPCB (https://jlcpcb. com) was chosen due to their fast turnaround time (under 2 weeks from order to delivery), integrated supply chain with real time stock information for part availability, and low cost (under 25 dollars for a fully assembled board with all parts already soldered in place, including express international shipping). However, this project is suitable for any vendor, i.e. is vendor agnostic. 

PC board The PC board is FR4 with 0.5 oz. Cu traces, and is 4 layers (with ground plane and power plane the middle layers) to improve grounding, reduce $V _ { c c }$ bounce and improve shielding. 

Case Although not necessary for operation, the design for a 3d printed case is provided in the supplementary information files, both for with and without battery. 

Battery A lithium polymer (LiPo) battery of arbitrary size (in x y and z dimensions) can be purchased for any application. For this system we recommend matching the x-y dimensions to the board size, and z can be 1 to several mm. For example, model 402,040 is 4 × 40 × 20 mm, costs only a few dollars and has 280 mAh of capacity, enough for over 6 h (Fig. 3). Power through the USB is also possible using a larger battery (e. g. Voltaic Systems V25 6400 mAh USB Always On Battery), available for tens of dollars, with 6400 mAh capacity i.e. >50 h of active running (Fig. 8). 

## 2.2. Software design

The software consists of the C++ code for running the micropro cessor and the HTML5/JavaScript code for the web server/user interface (UI). Microsoft Visual Studio Code is used as the integrated development environment (IDE), with the PlatformIO add on, which is synced to the github repository (https://github.com/PeterJBurke/Nanostat) for easy development and web based deployment. The C++ code is about 4000 lines of code. The HTML5/JavaScript is about 1000 lines of code. 

## 2.2.1. Firmware design

As usual in software development, we rely heavily on existing li braries integrated into this project. The following libraries were used: 

![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/eaf9883db7de74a718254209f3a20f31c3273e9645b5dc77c4f271139455650b.jpg)



Fig. 3. Photographs of the NanoStat. Alligator clips are shown but in principle any electrode connection technique can be used. (The circuit board connector is a standard 0.1” DuPont connector.) The bottom left inset shows the 3d printed case with the optional LiPo battery taken out of the case (4 × 40 × 20 mm, 280 mAh).


linneslab/LMP91000 version 1.0.0; me-no-dev/AsyncTCP version 1.1.1; ottowinter/ESPAsyncWebServer-esphome version 1.2.7; bblanchon/ ArduinoJson version 6.17.3; links2004/WebSockets version 2.3.6; bbx10/DNSServer version 1.1.0. There is a well developed software li brary for the LMP91000, developed initially by github users icatcu (2014) and jorgenro1 (2015), and most recently in Hoilett et al. [4]. In addition, there are several libraries for web hosting and file transfer. Because the ESP32 runs FreeRTOS operating system under the hood, and because of its dual core CPU architecture, it is possible to have simul taneous WiFi connectivity and control of the potentiostat and electro chemistry. This has not previously been the case with any other wireless connected electrochemical system (see “Prior Art” section below). 

The main issue with this architecture is the limited number of bits for the resolution, and the non-linearity of the ADC and DACs. For the limited number of bits, the AFE takes the analog “drive” signal and scales it by from 2 to 24 percent, so the effective bit resolution is much smaller at the cell. For 3.3 V power supply and 8 bits the resolution is 12 mV. But when divided down to 2 percent it is effectively 0.1 mV at the cell. 

The non-linearity and offset of the ADCs is far from ideal (see SI). For this purpose, a calibration routine was developed. The user connects the RE/CE, floats the WE, and runs the calibration routine once. The firm ware calculates the effect slope and offset, stores these in the on board flash memory, and calls on these in each subsequent run. (See SI for description of calibration technique). 

## 2.2.2. Web design

HTML5, JavaScript, and CSS (Cascading Style Sheet) are used together with the Bootstrap framework and the Plottly graphing package for an intuitive, responsive, and mobile friendly user interface on both desktop and mobile browsers. The data is transferred over a WebSocket opened between the client and the server in binary form to the browser for display. 

## 2.2.3. End user perspective for electrochemistry

The following sweep types are supported in the present version: cyclic voltammetry, square wave voltammetry, chronoamperometry, and normal pulse voltammetry, current-voltage curves, “oscilloscope” mode with quantitative noise analysis (RMS, standard deviation), and a calibration mode. The user can enter various parameters for different types of sweeps, as well as simple IV curves, and noise tests (“oscillo scope mode”). The software displays “sweeping” on the browser and once the sweep is completed presents the data in a graphical format (Fig. 4), which the user can change the scale on, zoom, etc. An ASCII text data file is auto downloaded to the web browser’s disk at the end of each sweep. 

## 2.2.4. System administration

If there is no WiFi connection, the device creates its own WiFi access point called “NanoStatAP”. The user can connect to that access point and enter in the credentials of up to 3 WiFi SSIDs. The user can also perform over the air “OTA” firmware upgrades by uploading the binary firmware, as well as list and delete files in the on board directory structure. Fig. 10 shows the entire UI and menu tree/structure. 

![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/75f7cc7b4265ee958d6242a09a65d68def74d57cbb65c4e76050797a3e0b4302.jpg)



Fig. 4. Graphical user interface (GUI) of one of the website pages. A test IV curve of a 10 kΩ resistor is shown immediately after measurement. The user can zoom, autoscale, read off each point, export the graph, and many other graphical options, as shown in the icons on top of the graph region.


## 3. Performance

## 3.1. Test chemicals

In order to compare the performance of the NanoStat to industry standard, we performed electrochemistry experiments using a Ag/AgCl (3M KCl) Reference Electrode (BASi MF-2056), a platinum wire auxil iary electrode (BASi MW-1033), and a 3.0 mm diameter gold working electrode (BASi MF-2114) in a standard 100 mL Standard glass elec trochemical cell (BASi MF-1051). The redox active species used was 5 mM potassium ferricyanide in 100 mM KCl at room temperature. The same cell was measured in the same session with a high end Gamry potentiostat (Reference 600) for comparison. 

## 3.2. Results: analytical electrochemistry

Fig. 5 shows the results of the Gamry and NanoStat to measure cyclic voltammetry, square wave voltammetry, chronoamperometry, and normal pulse voltammetry. The parameters are the default parameters shown in the “Setup” page of Fig. 10. The results agree extremely well, demonstrating this work is consistent with the most high end analytical chemistry instrumentation available on the market today. 

## 3.3. Results: noise

Given the simplicity of the system, the close proximity of digital, RF, and analog circuits on the board (and on the IC itself), and the lack of any on board analog or digital filtering, the noise performance is astounding. When operated with a battery, the system noise is determined by the bit resolution of the ADC. We explain. 

For the trial data in Fig. 5, the Gamry is using extensive on board analog and digital filtering, whereas the simple NanoStat measures only one ADC reading per data point with no filtering. Each data measure ment takes 160 µs. In the firmware, it is possible to average more readings if the rate is sufficiently slow. 

In order to quantitatively test the noise, we use the “dc bias” sweep mode. In the “dc bias” sweep mode, the firmware sets the dc bias voltage to a user selectable value, then reads ADC with various number of readings per point 1, 5, 10, 50, 100, 500, 1000, at a user specified delay between points. In Fig. 6, we plot the data points with and without the battery for a 100 kΩ test resistor biased at 100 mV. One data point is taken every 10 ms. With the PC USB power, there is clearly 60 Hz noise (red trace). With the battery power, this 60 Hz noise is gone. For no averaging (1 ADC reading per point, beginning), the error is about 1 bit in the current. This corresponds to 2 nA, since the ADC is 12 bits (full scale voltage = 3.3 V), and the TIA resistor is set to its most sensitive value the LMP91000 offers: 350 kΩ. Occasional spikes in the ADC reading are seen of a few bits, a known flaw of the ESP32 with unclear origin. The later data (with 1000 ADC readings per point, corresponding 

![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/1df5a111dfcde6aa1be195316caef9e1d15a6a292eeacdc49c44553e0f5e4abb.jpg)



V


![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/999463612394841426a257f441f4e8a0c9a0e98d91c4e02a03a5919e1768c0f4.jpg)


![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/9cb674db3eac5a48701b8afd08faa82322f5e5c35c9f9519c01eb8f334e05dfe.jpg)



V


![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/0fd89ec8792b7d6fee41be44e8b07be03e3b28d259ae1c2d16ba8b9e8da606dc.jpg)



Fig. 5. Cyclic voltammetry, square wave voltammetry, chronoamperometry, and normal pulse voltammetry measured with the NanoStat (blue) and a high end Gamry commercial potentiostat (red).


![image](https://cdn-mineru.openxlab.org.cn/result/2026-07-02/2f3adf06-2576-43ff-a827-4ac34d90082c/41ed8825debb2ad3616a9a694c3de6e49f1cbad75a003beadb962300cece9fa2.jpg)



Fig. 6. Current vs. time ADC reading (output of LMP91000 TIA) with 100 kΩ test resistor biased at 100 mV. One data point is taken every 10 ms. The data point is the ADC reading with various number of readings per point 1, 5, 10, 50, 100, 500, 1000. The 60 Hz noise is clearly visible in the red trace. With battery power only. the noise is limited by the bit resolution of the ADCs.


to 160 ms per reading) shows noise much lower then the single bit level. Thus, with no signal averaging, the noise is limited by the bit resolution of the ADC, under battery use, with no 60 Hz noise. With USB power from a PC, the 60 Hz noise is much was much worse and needed to be filtered out digitally to get to the bit resolution of the ADC. This shows that a battery powered, wireless potentiostat (without any USB con nections to a PC) is a wise choice for low noise operation. 

Due to limits on the ADC of the ESP32, one needs to ensure − 0.75 V < VTIA < 1 V, where VTIA = RTIA * WE current. For WE currents of 10 s of microamps, we typically therefore use 35 kohm as the TIA resistance, which gives a noise of about 50 nA, as can be seen in Fig. 5. The “oscilloscope” panel allows the user to plot the readings in real time on the web browswer for a given bias voltage and number of points to read. We confirmed that 50 nA is the RMS current noise with no averaging using a 35 kohm TIA amplifier and a 10 kohm test resistor. 

For applications that need more than 12 bits of resolution on the WE current, other approaches with a separate ADC IC would be needed (see below, “Prior Art” section). For applications that need better than 2 nA of current sensitivity, it is possible with the NanoStat for a user to select an external TIA resistor and connect it to the PC board labeled C1/C2, for additional current sensitivity. This option is supported in the firm ware; the user just needs to select “external resistor” in the dropdown menu of TIA gain on the “setup” page (Fig. 10). 

## 4. Discussion

## 4.1. Prior art

Whitesides has laid out the criteria for an analytical electrochemical system to be considered open source [5]. His criteria for open source are clear and reasonable: “Description of a truly open-source potentiostat should include all details required to replicate the system....” including circuit design, component list, board layout file, and software (micro controller firmware and PC/tablet/smartphone control software). Many academic papers only partially meet this, meaning a lab new to the field or even an experienced lab cannot reproduce prior academic work due to lack of published details. In the old days of chemistry. if a reviewer was to accept a paper, it was required by the editor that the reviewer reproduce the results in his own lab. While this may no longer be economical, certainly publishing the details of the experimental process should be [6]. 

As far as we can tell, the concept of a DIY, general purpose, open source potentiostat was pioneered by Kevin Plaxco’s group at UCSB about 10 years ago [7] (the “CheapStat”). There, an open source (hardware and software) handheld potentiostat was described. The user is provided gerber files for a PC board to be manufactured, a list of components, and the user would solder the components (cost ∼$80 in low volume) by hand to the PCB. The firmware and PC software for the USB control was provided. The AFE was a custom op amp and the mi crocontroller an Atmel XMEGA, an 8 bit microcontroller with 12 bit ADCs/DACs, 8 KB RAM and up to 128 kB flash. Voltage resolution was sub mV and current resolution sub nA. 

Whitesides’ group at Harvard followed on with a cell phone con nected (via audio cable) potentiostat in 2014 [5] of cost (parts only) ∼$25 in large volume (>1000 pieces). There, they used an Atmega328 (Atmel) 8 bit microcontroller (popularized as an Arduino) with a wired interface to a cell phone through the audio jack, an external 16 bit ADCs/DACs, and an analog 4 pole low pass filter (LPF). Current reso lution was 1 nA. The parts list was provided but the software and PC board Gerber files/CAD files were not provided, so it was not open source. 

A high performance open source project with USB connection to a PC was published as “DStat” [8] in 2017. There, an ATXMEGA256A3U was used (an 8 bit microcontroller with 8 KB RAM and up to 128 kB flash), with custom AFE and 16 bit DAC IC and a 24 bit ADC IC. The AFE design was excellent: pA level currents could be measured. The user is provided the firmware, PC software, Gerber files for PCB manufacture, and a list of components to order. The user solders some components by hand and others with a flow solder technique after ordering the parts. 

Dobbelaere [9] created a larger voltage version (up to 8 V) with PIC16F1459 (an 8 bit microcontroller), 22 bit IC for ADC/DAC, custom AFE, intended for battery research. All the details of the project (including the schematic, PCB design, last of components, microcon troller firmware, and host computer software) are made available to the user. 

Whitesides’ group in 2018 [10] published the first wireless (Blue tooth) open source potentiostat, which simply replaced the Atmega328 with the “RFDuino”, and still used a custom AFE with separate 16 bit ADC and DAC ICs. RFDuino had an integrated BLE and Core0 Arm 32 bit processor. RFDuino, first marketed in 2013, is obsolete and almost impossible to source, and the company was acquired in 2016. A list of components to purchase as well as the PCB manufacturing files is pro vided (in addition to the firmware software and smartphone software), so the user can order the boards, parts, and manually solder the final system. 

In 2018 Lopin [11] made a potentiostat with a SOC. It was not wireless. It used internal integrated op amps with higher noise and offset than discreet, but was still fully functional at the microamp level. It used on chip 12 bit ADCs/DACs. 

In 2019, Jenkins et al developed the open source project “ABE-Stat” [12]. This project provided both WiFi and Bluetooth wireless connec tivity, as well as extended the analytical chemistry sweep methods to include electrochemical impedance spectroscopy (EIS), a valuable tool for capacitive rather than Faradaic current measurments [13]. A custom AFE was designed, and 24 bit ADC and 16 bit DAC ICs were used, as well as an IC for network analysis for EIS. Although a relatively powerful WiFi enabled microcontroller was part of the system (ESP8266), and external Bluetooth IC was used for communications with a custom Android app. As prior open source projects, a list of components to purchase as well as the PCB manufacturing files is provided (in addition to the firmware software and smartphone software), so the user can order the boards. parts, and manually solder the final system. 

In 2020 Glasscott et al published the SweepStat [14]. The system used an Arduino Teensy with 16 bit integrated ADC and 12 bit inte grated DAC, and a custom AFE. The interface to the PC is with USB and 

Labview software environment is used for the control. As prior open source projects, a list of components to purchase as well as the PCB manufacturing files is provided (in addition to the firmware software and smartphone software), so the user can order the boards, parts, and manually solder the final system. 

An AFE LMP91000 chip on an evaluation board (LMP91000EVM) and a BeagleBone microcontroller on a separate board were used to demonstrate a low-cost miniaturized potentiostat in 2014 by Bhansali [15]. Later, Hoilett [4] created a potentiostat 22 × 20 mm with an in tegrated AFE (LMP91000) run by an ARM based microcontroller (SAMD21, 256KB flash memory, 10 bit DAC, 12 bit ADC). It is open source, although not wireless. Their software and hardware are hosted on github and detailed assembly and operation instructions are pro vided. The parts and Gerber files are provided. Their recommendation is to have PCBWay manufacture and solder the components, which the user buys from DigiKey and ships to PCBWay. PCBWay allows Digikey sourced parts, they will source them for you and solder them in place. They also offer 2 side SMT, and connectors. 

Mercer [16] developed a potentiostat with an ESP32 development board, used external 16 bit ADCs, and a custom op-amp AFE design. The entire system was demonstrated in a breadboard setting, not integrated onto a single PCB, and demonstrated the concept to use a microcon troller with integrated WiFi for potentiostat work. An electronically controlled (with servo) pump was also used. 

An Ardruino board was used to control a custom home made AFE daughter board using discrete components and op-amps in ref. [17]. 

An open-source multi-channel potentiostat based on a Raspberry P with additional boards for each channel was demonstrated in ref. [18]. 

Additional related work that is not open source is given in refs. [19–25]. 

In addition to requiring high value skills (soldering tiny components) and being extremely inefficient, even for small numbers of boards, hand soldering has a significant limitation in the size of the board that can be made. Pick and place reflow soldering machines enable sub-mm accu racy in placement of parts whose size that far exceeds the ability of even the most trained human hand. Even compared to a technician manually soldering parts at minimum wage they also are more economical these days. And of course the reliability is much higher. 

Many of the above projects require manual soldering, a custom AFE design with multiple AFE op amp chips and resistors, separate ADC and DAC chips, and typically separate wireless chips, if at all. In addition the memory and processing ability of almost all the microcontrollers used so far is not able to provide on board analysis, nor on board file systems or website hosting capabilities. Finally they are mostly manually soldered and require at least three institutions to make : 1) the parts source 2) The PCB manufacturer 3) the soldering (also many times manual, a severe disadvantage described above). We use an integrated provider that does all of 1–3 in house, as well as provides the (free) web based CAD with integrated supply chain real time stock information, with low cost and under 2 week turnaround time. Finally our work has a battery power and USB power to charge the battery, which many of the above open source projects do not have. Finally, none of the above host a website on the potentiostat system and require the user to install custom software on a pc or smartphone. 

## 4.2. Scalability and future directions

## 4.2.1. Security

This version of the software uses HTTP, not HTTPs. For local area networks behind a firewall this is sufficient. However, for traffic over the public internet, it would be important to switch to enctryped traffic with the HTTPs protocol, and to enable a secure user login system. 

## 4.2.2. Further miniaturization

We estimate the size of the circuit board and the number of com ponents can be reduced by roughly a factor of two. First of all, the USB to 

UART interface was useful for developing the code (firmware, HTML5/ JavaScript) for this project. However, now that we have demonstrated over the air (OTA) WiFi firmware update capability, the USB PC inter face is superfluous and a simple UART interface (with off-chip FTDI to USB conversion) would save board space and components. Furthermore, the boot switches to put the microcontroller into firmware flash mode were found to be unnecessary with the IDE used in this work. These two changes could be easily implemented in future revisions of the board without any change in the software or functionality. 

## 4.2.3. Power management

The ESP32 supports a sleep mode, and wake on LAN mode. These could be implemented to preserve power when the NanoStat was not actively taking measurements. 

## 4.2.4. On board analysis

Due to the dual core 32 bit processors and generous onboard RAM, it is possible to incorporate just about any kind of signal processing and analysis a future user could design onto the microcontroller itself. In contrast to prior open source potentiostats (see below “Prior Art” sec tion), therefore, this project is therefore very scalable to advanced signal processing and on board analysis. 

## 4.2.5. Device performance (resolution, shielding)

Here we touch on how device performance (resolution, shielding) can be improved for future design. The ADC and DAC bit resolution are set by the microcontroller, so improvement here could use a separate ADC with more bits. The shielding could be improved by providing replacing the plastic 3d printed case with a metal case (Faraday cage), however the case design would need to allow WiFi antenna access to fields outside the cage. 

## 4.3. Towards an open source suite of electrochemistry software

Each of the above projects listed in prior art has custom software, but it all basically performs the same core function for control/acquisition (run a sweep of voltage vs. time, measure current vs time). The UI is also similar. Therefore, many these papers are “reinventing the wheel” to a large extent. For this reason, we propose a hardware abstracted, unify ing suite of software we call “OpenEChem”. With a common, unifying set of software, developers of advanced electrochemistry and sensing hardware can focus on the more challenging aspects of the field, such as (for example) electrode functionalization, miniaturization, and appli cation specific demonstrations. 

Interestingly, the use of open source is also growing in the drone community. For example, we pioneered [26] and open source web based 4G connected drone technology using a cloud server to handle routing and encryption, and that is now adopted by the French drone company Anafi in the first 4G connected commercial drone in July, 2021. The drone community has also recently started adopting the ESP32 micro controller and the advantages of the above manufacturing capability to have an open source, long range, cheap lightweight radio control link for drones (https://github.com/openLRSng) with hundreds of miles of range for receivers costing under ten dollars and weighing less than 1 g with on board antenna. 

## 5. Conclusion

We presented an open source, fully wireless potentiostat (the “NanoStat”) for applications in electrochemistry, sensing, biomedical diagnostics, and nanotechnology, based on only 2 integrated circuit chips. This was demonstrated to have the same functionality as modern desktop based potentiostats. While this particular project can be used by any lab or user on a very small budget, enabling numerous applications and use cases, it lays the groundwork for a more ambitious, web based, open source software suite as the kernel of a general purpose, hardware abstracted electrochemistry software suite, which we propose as “OpenEChem”. 

## CRediT authorship contribution statement

Shawn Chia-Hung Lee: Data curation, Formal analysis, Investiga tion, Methodology, Validation, Visualization, Writing – review & edit ing. Peter J. Burke: Conceptualization, Data curation, Formal analysis, Funding acquisition, Investigation, Methodology, Project administra tion, Resources, Software, Supervision, Validation, Visualization, Writing – original draft, Writing – review & editing. 

## Declaration of Competing Interest

Authors declare that they have no conflict of interest. 

## Acknowledgments

We acknowledge support from the Army Research Office (W911NF 18-1-0076). 

## Supplementary material

Supplementary material associated with this article can be found, in the online version, at doi:10.1016/j.electacta.2022.140481. 

## References



[1] A.J. Bard, L.R. Faulkner, Electrochemical Methods. Fundamental and Applications, second ed., John Wiley and Sons, Inc., New York, 2001 https://doi.org/10.1039/ C4LC00091A. B978-0-08-098353-0.00002-6. 





[2] Y. Yang, W. Gao, Wearable and flexible electronics for continuous molecular monitoring, Chem. Soc. Rev. 48 (6) (2019) 1465–1491, https://doi.org/10.1039/ c7cs00730b 





[3] D. Datta. X. Meshik, S. Mukheriee, K. Sarkar, M.S. Choi, M. Mazouchi, S. Farid. Y. Y Wang PJ. Burke M Dutta M A Stroscio Submillimolar detection of adenosine monophosphate using graphene-based electrochemical aptasensor, IEEE Trans. Nanotechnol. (2017). https://doi,org/10.1109/TNANO.2016.2647715 





[4] O.S. Hoilett, J.F. Walker, B.M. Balash, N.J. Jaras, S. Boppana, J.C. Linnes, et al., Kickstat: a coin-sized potentiostat for high-resolution electrochemical analysis, Sensors 20 (8) (2020) 1–12, https://doi.org/10.3390/s20082407. 





[5] A. Nemiroski, D.C. Christodouleas, J.W. Hennek, A.A. Kumar, E.J. Maxwell, M. T. Fernandez-Abedul, ´ G.M. Whitesides, et al., Universal mobile electrochemica detector designed for use in resource-limited applications, Proc. Natl. Acad. Sci USA 111 (33) (2014) 11984–11989, https://doi,org/10.1073/pnas,1405679111. 





[6] M.D.M. Dryden, R. Fobel, C. Fobel, A.R. Wheeler, et al., Upon the shoulders of giants: open-source hardware and software in analytical chemistry, Anal. Chem. 89 (8) (2017) 4330–4338, https://doi.org/10.1021/acs.analchem,7b00485. 





[7] A.A. Rowe, A.J. Bonham. RJ. White, M.P. Zimmer, R.J. Yadgar. T.M. Hobza, J. W. Honea. L Ben-Yaacov. K W. Plaxco, et al., Cheapstat: an open-source. “do-ityourself” potentiostat for analytical and educational applications, PLoS One 6 (9) (2011), https://doi.org/10.1371/journal.pone.0023783. 





[8] M.D.M. Dryden, A.R. Wheeler, DStat: a versatile, open-source potentiostat for electroanalysis and integration. PLoS One 10 (10) (2015). https://doi,org 10.1371/iournal pone 0140349 





[9] T. Dobbelaere, P.M. Vereecken, C. Detavernier, A USB-controlled potentiostat galvanostat for thin-film battery characterization. HardwareX 2 (2017) 34–49. https://doi.org/10.1016/i.ohx.2017.08.001. 





[10] A. Ainla, M.P.S. Mousavi, M.N. Tsaloglou, J. Redston, J.G. Bell, M.T. Fern´andez-Abedul. G.M. Whitesides, et al., Open-source potentiostat for wireless 





electrochemical detection with smartphones, Anal. Chem. 90 (10) (2018) 6240–6246, https://doi.org/10.1021/acs.analchem.8b00850 





[11] P. Lopin, K.V. Lopin, PSoC-stat: a single chip open source potentiostat based on a programmable system on a chip, PLoS One 13 (7) (2018) 1–21, https://doi.org/ 10.1371/iournalpone.0201353 





[12] D.M. Jenkins, B.E. Lee, S. Jun, J. Reyes-De-Corcuera, E.S. McLamore, et al., ABE Stat, a fully open-source and versatile wireless potentiostat project including electrochemical impedance spectroscopy, J. Electrochem. Soc. 166 (9) (2019) B3056–B3065, https://doi.org/10.1149/2.0061909jes. 





[13] J. Li, P.J. Burke, Measurement of the combined quantum and electrochemical capacitance of a carbon nanotube, Nat. Commun. 10 (1) (2019) 3598, https://doi. org/10.1038/s41467-019-11589-9. 





[14] M.W. Glasscott, M.D. Verber, J.R. Hall, A.D. Pendergast, C.J. McKinney, J.E. Dick, et al., SweepStat: a build-it-yourself, two-electrode potentiostat for macroelectrode and ultramicroelectrode studies, J. Chem. Educ. 97 (1) (2020) 265–270, https:/ doi.org/10.1021/acs.jchemed.9b00893. 





[15] A.F.D. Cruz, N. Norena, A. Kaushik, S. Bhansali, A low-cost miniaturized potentiostat for point-of-care diagnosis, Biosens. Bioelectron. 62 (V) (2014) 249–254, https://doi.org/10.1016/j.bios.2014.06.053. 





[16] C. Mercer, R. Bennett, P. Conghaile, J.F. Rusling, D. Leech, Glucose biosensor based on open-source wireless microfluidic potentiostat, Sens. Actuators, B 290 (2019) 616–624, https://doi.org/10.1016/j.snb.2019.02.031. 





[17] Y.C. Li, E.L. Melenbrink, G.J. Cordonier, C. Boggs, A. Khan, M.K. Isaac, L. K. Nkhonjera, D. Bahati, S.J. Billinge, S.M. Haile, R.A. Kreuter, R.M. Crable, T. E. Mallouk, An easily fabricated low-cost potentiostat coupled with user-friendly software for introducing students to electrochemical reactions and electroanalytical techniques, J. Chem. Educ. 95 (9) (2018) 1658–1661, https://doi org/10.1021/acs.jchemed.8b00340. 





[18] P. Pansodtee, J. Selberg, M. Jia, M. Jafari, H. Dechiraju, T. Thomsen, M. Gomez, M. Rolandi, M. Teodorescu, The multi-channel potentiostat: development and evaluation of a scalable mini-potentiostat array for investigating electrochemical reaction mechanisms, PLoS One 16 (2021) 1–14, https://doi.org/10.1371/journal. pone.0257167. 





[19] D.P. Rose, M.E. Ratterman, D.K. Griffin, L. Hou, N. Kelley-Loughnane, R.R. Naik, J. A. Hagen, I. Papautsky, J.C. Heikenfeld, et al., Adhesive RFID sensor patch for monitoring of sweat electrolytes, IEEE Trans. Biomed. Eng. 62 (6) (2015) 1457–1465, https://doi.org/10.1109/TBME.2014.2369991. 





[20] J. Kim, S. Imani, W.R. de Araujo, J. Warchall, G. Vald´es-Ramírez, T.R.L.C. Paix˜ao, P.P. Mercier, J. Wang, Wearable salivary uric acid mouthguard biosensor with integrated wireless electronics, Biosens. Bioelectron. 74 (2015) 1061–1068, https://doi.org/10.1016/j.bios.2015.07.039. 





[21] G.F. Giordano, M.B.R. Vicentini, R.C. Murer, F. Augusto, M.F. Ferr˜ao, G.A. Helfer, A.B. da Costa, A.L, Gobbi, LW. Hantao, R.S. Lima, et al., Point-of-use electroanalytical platform based on homemade potentiostat and smartphone for multivariate data processing. Electrochim. Acta 219 (2016) (2016).170–177 https://doi org/10.1016/i electacta 2016.09.157 





[22] S. Imani, A.J. Bandodkar, A.M.V. Mohan, R. Kumar, S. Yu, J. Wang, P.P. Mercier, et al., A wearable chemical-electrophysiological hybrid biosensing system for realtime health and fitness monitoring, Nat. Commun. 7 (2016) 1–7, https://doi.org/ 10.1038/ncomms11650. 





[23] R. Pruna, F. Palacio, A. Baraket, N. Zine, A. Streklas, J. Bausells, A. Errachid M. Lopez, ´ et al., A low-cost and miniaturized potentiostat for sensing of biomolecular species such as TNF-α by electrochemical impedance spectroscopy, Biosens. Bioelectron. 100 (2018) 533–540. https://doi,org/10.1016/j. bios,2017.09.049. 





[24] V. Bianchi, A. Boni, S. Fortunati, M. Giannetto, M. Careri, I. De Munari, et al., A Wi-Fi cloud-based portable potentiostat for electrochemical biosensors, IEEE Trans. Instrum. Meas. 69 (6) (2020) 3232–3240, https://doi.org/10.1109 TIM.2019.2928533. 





[25] S.D. Adams, E.H. Doeven, K. Quayle, A.Z. Kouzani, MiniStat: development and evaluation of a mini-potentiostat for electrochemical measurements. IFEE Access 7 (2019).31903–31912,https://doi org/10.1109/ACCESS 2019.2902575 





[26] P.J. Burke, A safe, open source, 4G connected self-flying plane with 1h flight time and all up weight (AUW) < 300 g: towards a new class of internet enabled UAVs, IEEE Access 7 (2019) 67833–67855, https://doi.org/10.1109 ACCESS.2019.2917851 

