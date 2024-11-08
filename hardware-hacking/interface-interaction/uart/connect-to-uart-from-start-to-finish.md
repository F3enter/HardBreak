# Connect to UART- From start to finish

To interact with a UART interface, you would need:

* A multimeter
* an USB to UART TTL adapter
* jumper cables
* and in some cases: a soldering station

After opening the device follow:

1. Get an overview of what is available of the PCB board
   1. Checkout which chips are used&#x20;
      1. Google the datasheet of each chip you find (model should be printed on top of the chip)
      2.  It can be useful to take a picture of the PCB and label everything you can identify

          <figure><img src="../../../.gitbook/assets/komplett-beschriftet (1).png" alt=""><figcaption><p>Example layout of an PCB</p></figcaption></figure>
   2.  Check for connector or test pads (can be quick wins to find a UART/JTAG etc.)&#x20;

       1. Even better if we find actual pins, where we can connect jumper cables to:

       <figure><img src="../../../.gitbook/assets/image (31).png" alt="" width="257"><figcaption><p>Uart pins exposed</p></figcaption></figure>

       1. Also JTAG (where we need more pins) are very interesting targets

       <figure><img src="../../../.gitbook/assets/image (67).png" alt="" width="82"><figcaption><p>JTAG-Connector</p></figcaption></figure>

       <figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>
2.  Check the pinout for the connectors:

    1.  Put your multimeter in continuity mode (often a "sound" symbol):

        <figure><img src="../../../.gitbook/assets/image (6).png" alt="" width="507"><figcaption><p>Multimeter in continuity mode</p></figcaption></figure>
    2. This mode will check if there is a direct link between two points on the PCB
    3. Put one probe on the connector pad you want to test
    4.  The other one goes on the chip (datasheet will tell you what pins are used for UART/SPI/JTAG)

        <figure><img src="../../../.gitbook/assets/image (5).png" alt="" width="287"><figcaption><p>How to probe</p></figcaption></figure>

    You need to find the GND (ground), TX (transmit) and RX(receive) pins to communicate with UART.
3.  Now you need to connect the pins using jumper cables to the UART-USB-TTL adapter (make sure RX -> TX and TX->RX, as they have to be reversed). This can be done by soldering the cables onto the connector pins, plug them in or use clamps.

    <figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption><p>UART connection</p></figcaption></figure>
4. On your PC use the following command to communicate over UART (you may have to adjust the baud rate)

{% tabs %}
{% tab title="Linux" %}
```bash
sudo minicom -D /dev/ttyUSB0 -b 115200
sudo picocom -b 115200 -r -l /dev/ttyUSB0
```
{% endtab %}

{% tab title="Windows" %}
**Using PuTTY (Windows)**:

* Select “Serial” and enter the COM port (e.g., COM3) and baud rate (115200).
{% endtab %}
{% endtabs %}

5. If you see readable data: You done it correctly!

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption><p>example bootlog </p></figcaption></figure>

**Congrats!** You found your first serial connection! Check out the UART chapter on how to use this to dump the firmware from the device.
