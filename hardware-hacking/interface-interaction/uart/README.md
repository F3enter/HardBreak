# UART

## **Theory**

UART is a hardware communication protocol that facilitates serial communication between devices. It is commonly used in embedded systems for communication between a microcontroller and peripherals like sensors, GPS modules, or Wi-Fi modules. UART operates on two main lines: Transmit (TX) and Receive (RX), often accompanied by Ground (GND).

Pentesters may encounter UART interfaces during hardware security assessments and can use this interface to extract firmware, debug, or interact with the device at a low level.

### Configuration

UART can be configured in many different ways. Next, we want to introduce some key settings.

#### 1. **Baud Rate**

* The baud rate is the speed at which data is transmitted, measured in bits per second (bps). Common baud rates include 9600, 19200, 38400, 57600, and 115200 bps.
* Both devices communicating over UART must use the same baud rate. If there's a mismatch, data may become garbled.

#### 2. **Data Bits**

* This setting determines the size of each data packet. Common configurations are 7 or 8 data bits, with 8 being the most common.
* More data bits can carry more information per packet, but it also affects overall speed since each data frame becomes larger.

#### 3. **Parity**

* Parity is a form of error checking. The parity bit (if used) helps detect data corruption:
  * **None**: No parity bit is used (most common).
  * **Even**: The parity bit is set to make the total number of 1s in the frame even.
  * **Odd**: The parity bit is set to make the total number of 1s in the frame odd.
* Parity is useful for simple error checking but doesn’t catch all errors.

#### 4. **Stop Bits**

* Stop bits indicate the end of a data packet and help the receiver recognize the start and end of each frame.
* Common configurations are 1 or 2 stop bits, with 1 stop bit being standard in most UART communications.

#### 5. **Flow Control**

* Flow control ensures smooth data transfer, especially when one device sends data faster than the other can process it. The main types are:
  * **None**: No flow control; data flows freely (used when the receiving device can keep up with incoming data).
  * **Hardware (RTS/CTS)**: Uses Request to Send (RTS) and Clear to Send (CTS) pins to control data flow. When the receiver is ready, it signals the transmitter via the CTS line.
  * **Software (XON/XOFF)**: Uses special characters (XON to resume, XOFF to pause) embedded in the data stream to control data flow.

#### 6. **Other Configuration Options**

* Break Condition
  * A longer-than-usual period of low voltage to signal an intentional break in communication, used as a control signal in some setups.
* Idle State
  * Defines the line’s resting state when no data is transmitted, usually set to a high voltage.

## **Requirements**

1. Hardware
   * UART Adapter (USB-to-UART module such as FTDI, CH340, or CP2102)
   * Jumper wires
   * Multimeter (for checking pin continuity and voltage levels)
   * Soldering kit (if pins need to be exposed)
2. Software
   * Serial terminal programs like:
     * `minicom` (Linux)
     * `PuTTY` (Windows)
     * `screen` (macOS/Linux)
     * `picocom` (Linux)
   * Baud rate detection tools:
     * `baudrate.py` script or any other baud rate analyzer.

## **Usage:**

1.  UART Discovery

    * Often, UART ports are not labeled on the board. Use a multimeter to identify GND, then trial and error to locate RX/TX pins. Connect them to your UART-USB adapter using jumper cables.

    Command Example (minicom,screen)

    ```bash
    minicom -b 115200 -o -D /dev/ttyUSB0  #/dev/ttyUSB0 = your adapter
    screen /dev/ttyUSB0 115200
    ```

    This opens a serial session on `/dev/ttyUSB0` at a baud rate of 115200, a common default for UART devices.
2.  Baud Rate Detection (optional, you can also just try common ones: 115200, 9600 etc.)

    * Devices might communicate at different baud rates. Incorrect baud rates lead to garbage data on the terminal.

    Command Example

    ```bash
    python baudrate.py -p /dev/ttyUSB0
    ```

    This script tests various baud rates to find the correct one.
3. Firmware Extraction
   * UART can be used to extract firmware if the bootloader allows access to memory dumps.
   * check the "Firmware extraction using UART" section
4. Debugging and Command Injection
   * Some devices leave debug interfaces open on UART, which can allow pentesters to gain shell access or inject commands.
5. Bricking/Unbricking a Device:
   * UART access may allow you to recover a bricked device by flashing new firmware or interacting with the bootloader.

## Resources:

[https://wiki.emacinc.com/wiki/Getting\_Started\_With\_Minicom](https://wiki.emacinc.com/wiki/Getting\_Started\_With\_Minicom)\
[https://www.cyberark.com/resources/threat-research-blog/accessing-and-dumping-firmware-through-uart](https://www.cyberark.com/resources/threat-research-blog/accessing-and-dumping-firmware-through-uart)
