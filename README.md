# Hamilton open-source thermostat

The Hamilton thermostat is an open-source single stage thermostat built around the [Hamilton](https://github.com/hamilton-mote/hw) and [Nucleum](https://github.com/lab11/nucleum) motes. The core motivation of the project is to integrate the hardware necessary to turn these low-cost and low-power motes into an energy efficient and feature-rich thermostat.

The Hamilton thermostat can control a standard single stage heating and/or cooling system with a circular fan (R/Rh/Rc, W, Y, and G wires). The design features a temperature sensor (AT30TS74), humidity/temperature sensor (HDC1080), PIR sensor (EKMB1101111), real-time clock (MCP7940N), and 1Mbit EEPROM (AT24CM01). Wireless communication is achieved via the 802.15.4 radio (built into the SAMR21 of the Hamilton mote) or the Bluetooth Low Energy (BLE) radio (built into the NRF51822 of the Nucleum mote). The thermostat can be powered by 3 AA batteries or by 24VAC power (requires a C wire). The design also includes a simple user interface with 4 buttons and LED-based temperature and setting indication. The user interface can easily be replaced with a custom board by connecting to the main thermostat board via pinouts for the I2C bus, power (+3V3/GND), 2 analog pins, and 4 digital pins.

### Parts Overview

* AT30TS74 (I2C Address 1001000): The AT30TS74 temperature sensor has a configurable resolution of 9 to 12 bits and a typical accuracy of +/- 1 degree Celsius.
* HDC1080 (I2C Address 1000000): The HDC1080 humidity and temperature sensor has a configurable resolution of 8, 11, or 14 bits for humidity and 11 or 14 bit for temperature. The typical accuracy for humidity and temperature is +/- 2% and +/- 0.2 degree Celsius, respectively.
* MCP7940N (I2C Address 1101111): The MCP7940N is a real-time clock/calendar with SRAM.
* AT24CM02 (I2C Address 10100###): The AT24CM02 provides 2-Mbits (256-Kbytes) of Serial EEPROM. (Note: The original version of the Hamilton thermostat included AT24CM01 chips)
* PCA9557 (I2C Address 0011000): The PCA9557 is an 8 channel I/O parallel port expander. The expander is used to control the 3 latching relays (EE2-3TNUH-L) which actuate the heating system, cooling system, and circular fan.
* TLC59116 (I2C Addresses 1100001, 1100011, and 1100111): The TLC59116 is a 16 channel constant-current LED sink driver with 256-step (8-bit) linear programmable brightness. The Hamilton thermostat interface board includes 3 TLC59116 chips.
* EKMB1101111: The EKMB1101111 is a fully integrated PIR motion sensor with 1µA low current consumption. The output of the PIR sensor is connected to pin PA06 of the SAMR21 chip and P05/AIN6 of the NRF51822 chip.

### Communication

Communication between the SAMR21 chip or the NRF51822 chip and the other components of the Hamilton thermostat is primarily achieved via the shared I2C bus. The SAMR21 chip is connected to the bus with pin PA16 as SDA and PA17 as SCL and the NRF51822 chip is connected with pin P07 as SDA and P01 as SCL. The I2C addresses of the components are noted in the list above.

Communication between the SAMR21 chip and the NRF51822 chip is achieved over an SPI link. The SAMR21 chip is design to use SERCOM2 with PA08/PAD[0] as MOSI, PA09/PAD[1] as MISO,
PA15/PAD[3] as SCK, and PA14/PAD[2] as chip select.  The NRF51822 chip provides flexibility in the assignment of SPI pins and is designed to use P30 as MOSI, P29 as MISO, P00 as SCK, and P28 as chip select.

Additionally, the SAMR21 and NRF51822 chips are connected by several additional traces.
* A6 (the PIR sensor output) to SAMR21 pin PA06 and NRF51822 pin P05/AIN6.
* A7 to SAMR21 pin PA07 and NRF51822 pin P06/AIN7.
* D18 to SAMR21 pin PA18 and NRF51822 pin P18.
* D19 to SAMR21 pin PA19 and NRF51822 pin P19. There is also an LED connected to D19.
* D24 to SAMR21 pin PA24 and NRF51822 pin P25.
* D25 to SAMR21 pin PA25 and NRF51822 pin P25.
If necessary, these traces can be physically cut on the bottom on the Hamilton thermostat main board to sever the connections to the NRF51822 chip.

Lastly, the Hamilton thermostat is capable of wireless communication via the 802.15.4 radio (built into the SAMR21 chip) and the BLE radio (built into the NRF51822 chip).

### Relays

The Hamilton thermostat main board includes 3 EE2-3TNUH-L dual-coil latching signal relays for actuating a heating system (W wire), cooling system (Y wire), and circular fan (G wire).  Each relay is rated for a coil voltage of 3VDC, a contact current of 2A, and a switching voltage of up to 250VAC. Because the EE2-3TNUH-L is a latching relay, it maintains the contact position indefinitely without requiring a current to be applied. Thus, each relay contains 2 coils (1 for setting and 1 for releasing) and a current pulse is applied to either coil to change the contact position. Note that applying a current to one of the coils over a long period of time will damage the relay.

Each relay is connected to either the W wire (heat), Y wire (cooling), or G wire (fan). Setting a relay will connect the respective wire to the R/Rh/Rc wire and releasing the relay will sever the connection. The relays are controlled via the PCA9557 8 channel I/O expander. The W relay is connected to IO0 for setting and IO1 for releasing, the Y relay to IO2 for setting and IO3 for releasing, and the G relay to IO4 for setting and IO5 for releasing.

Lastly, the Hamilton thermostat is capable of simultaneously connecting the G wire (fan) when the W relay or Y relay is set. Adding the R18 jumper will cause the W relay to connect both the W and G wires to R when the relay is set and adding the R17 jumper will cause the Y relay to connect the Y and G relays to R.

### Power

The Hamilton thermostat can be power by battery and/or 24VAC power (if a C wire is available). The Hamilton thermostat main board includes a 3 AA battery holder and a AP2210 3.3VDC 300mA LDO regulator. If necessary, regulator can be bypassed by adding the R1 jumper.

If a C wire is available, the thermostat can operate on 24VAC power. For AC/DC conversion, the main board includes a full wave bridge rectifier and an OKI-78SR 36VDC to 3.3VDC 1.5A buck converter. The thermostat is designed to allow for operation on 24VAC power with battery backup.

### User Interface

The Hamilton thermostat has a simple and intuitive user interface for communicating the current temperature measurement and set point and for enabling a user to perform basic operations like setting the mode (heating, cooling, off) and adjusting the set point. The interface board includes four buttons: one for setting the mode, two for adjusting the temperature set point, and one for setting a temporary temperature hold.

The board also includes 40 LEDs for denoting the thermostats settings. To communicate the current temperature measurement and set point, 32 of the LEDs are arranged in an arc around the center of the board (16 for the measurement and 16 for the set point). The LEDs are controlled via three TLC59116 16 channel LED sink drivers.

The interface board is connected to the main thermostat board via pinouts for the I2C bus (SDA and SCL), power (+3.3VDC and GND), analog pins A6 and A7, and digital pins D18, D19, D24, and D25.

### Transient Voltage Suppression (TVS)

To protect the thermostat components from voltage spikes and electrostatic discharge (ESD), the Hamilton thermostat main and interface boards include several transient-voltage-suppression diodes.

On the main board:
* Diode D2 protects the 24VDC output of the diode rectifier and the voltage input of the buck converter.
* Diode D15 protects the batteries and the voltage input of the LDO regulator.
* Diode D16 protects the 3.3V line.
* Integrated diodes U8 (IP4220CZ6) protects the I2C lines.

On the interface board:
* Diodes D41, D42, D43, and D44 protect the output of each button on the interface board.
* Diode D45 protects the 3.3V line.
* Integrated diodes U5 (IP4220CZ6) protects the I2C lines.

### Changelog
* Added IP4220CZ6 diode (U8) to BOM of main board
* Replaced AT24CM01 with AT24CM02 in BOM of main board
