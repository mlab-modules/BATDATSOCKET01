# BATDATSOCKET01B - Li-ion power and Data Storage Module socket interface

The BATDATSOCKET01 is a complementary module to the battery and data module [BATDATUNIT01](https://github.com/mlab-modules/BATDATUNIT01). It is a reliable power source for extended durations, making it an integral component in a wide array of detectors or measuring systems.

## Applications

The BATDATUNIT01 is designed for versatility:

- As a **power module for semiconductor particle detectors** like the [AIRDOS04](https://github.com/UniversalScientificTechnologies/AIRDOS04), it ensures uninterrupted data acquisition in environmental monitoring.
- It can be integrated into **remote sensing stations**, where it provides consistent power and data logging capabilities for long-term ecological studies.
- In **automated weather stations**, the module's resilience and sensor suite offers valuable insights into meteorological conditions.
- The module can be deployed in **mobile robotics** for energy supply and environmental data collection, aiding navigation and decision-making processes.
- It is also ideal for **educational purposes**, as a hands-on tool to teach about energy management, data acquisition, and sensor integration.

For more detailed information on interfacing and protocols, please refer to the [BATDATUNIT01](https://github.com/mlab-modules/BATDATUNIT01) documentation.

## Design

Designed for convenience, the module allows for fast detachment from the measuring part without tools, streamlining the replacement process. Data can be downloaded during battery charging thanks to onboard memory.

![BATDATSOCKET01 top view](/doc/gen/img/BATDATSOCKET01-top.svg)

![BATDATSOCKET01 bottom view](/doc/gen/img/BATDATSOCKET01-bottom.svg)

### Internal structure

```mermaid

    flowchart TD

    subgraph a[User interfaceI]
        USB[[USB-C\nData + power]]
        UI1([User interface \n Button + battery indicator])
        UI2([User interface \n Button + 3x LED])
    
    end

    USB --USB --> USW

    FTDI -- I2C --> I2CSW
    FTDI -- UART <--> MCU
    MCU -- I2C --> I2CSW[I2C mux]
    MCU[Microcontroller]
    MCU --SPI <--> SDW
    MCU --> SDW

    subgraph one[Memory interface]
    SDR[SD card] <--> SDI
    SDI[SD card interface] <--> USW
    USW[USB-SWITCH]
    SDW[SD card \n SPI-SWITCH] <--> SDI
    end


    subgraph three[Digital part ]
    USW -- USB <--> FTDI[FTDI\nI2C + UART]

    I2CSW --I2C--> HYG[Hygrometer]
    I2CSW --I2C--> ALT[Pressure sensor]

    I2CSW -- I2C --> I2Cen

    end


    I2CSW --I2C--> GAUGE
    I2CSW --I2C--> charger


    GAUGE --> UI1
    MCU --> UI2

    GAUGE --> PWR3v3
    GAUGE --> PWR3v3E
    GAUGE --> PWR5vE
    
    USW -- USB <--> FTDI[FTDI\nI2C + UART]
    USB -- Power --> charger

    subgraph two[POWER sources]
    PWR3v3[Power supply\nfor internal 3.3V]
    PWR3v3E[Power supply\nfor detector 3.3V]
    PWR5vE[Power supply\nfor detector 5V]
    
    charger --> GAUGE[Accumulators gauge]
    GAUGE <==> LIION[(5x Li-ion cells)]
    end
    PWR3v3 --> MCU

    PWR3v3E --> DI
    PWR5vE --> DI

    I2Cen[I2C switch] -- I2C --> DI
    DI[[Detector interface]] == GPIO, SPI <==> MCU



```

## Connectivity

A durable connector brings together data and detection elements with impressive mechanical resilience. It hosts UART, I2C, SPI buses, and extra GPIO signals, providing extensive interfacing options with various systems.

