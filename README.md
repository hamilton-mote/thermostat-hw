# Hamilton open-source thermostat

The Hamilton thermostat is an open-source single stage thermostat built around the [Hamilton](https://github.com/hamilton-mote/hw) and [Nucleum](https://github.com/lab11/nucleum) motes. The core motivation of the project is to integrate the hardware necessary to turn these low-cost and low-power motes into an energy efficient and feature-rich thermostat.

The Hamilton thermostat can control a standard single stage heating and/or cooling system with a circular fan (R/Rh/Rc, W, Y, and G wires). The design features a temperature sensor (AT30TS74), humidity/temperature sensor (HDC1080), PIR sensor (EKMB1101111), real-time clock (MCP7940N), and 1Mbit EEPROM (AT24CM01). Wireless communication is achieved via the 802.15.4 radio (built into the SAMR21 of the Hamilton mote) or the BLE radio (built into the NRF51822 of the Nucleum mote). The thermostat can be powered by 3 AA batteries or by 24VAC power (requires a C wire). The design also includes a simple user interface with 4 buttons and LED-based temperature and setting indication. The user interface can easily be replaced with a custom board by connecting to the main thermostat board via pinouts for +3V3/GND, I2C bus, 2 analog pins, and 4 digital pins.

### Sensors

### Communication

SPI, connected pins

### Data Collection/Storage

### Relays

Jumpers

### Power

AC, Regulators, Jumpers

### User Interface

### Transient Voltage Suppression
