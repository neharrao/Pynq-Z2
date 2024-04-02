# PYNQ-Z2 Board Setup and Programming Guide
## Setting up the board
- Connect the micro-usb to the board and connect it to the laptop.
- Ensure the Jumper for power source selection is at USB and not REG. REG is used when the board is connected to an external DC power supply.
- Power on the board using the 'Power Switch' -> 'power on' LED (CR8) (red) lights up.
- Ensure the Jumper for boot mode selection is at SD.
- Insert the SD card (should contain the image (ub) and .bin file).
- 'DONE LED' (CR7) lights up, 'the two RGB LEDs (LD4 and LD5)' (blue) blink, and the '4 User Defined LEDs (LD0, LD1, LD2 & LD3)' blink and remain turned on.
- Booting complete.
- Note: If the LEDs do not light up after inserting the SD card, download the image again (from: `http://www.pynq.io/board.html`) and flash it to the SD card (using balenaEtcher).
- Connect the Ethernet cable to the board and laptop. 
-  In the laptop -> Control Panel -> Network and Internet -> Network and Sharing Centre -> Ethernet -> Properties -> Internet Protocol Version 4 (TCP/IPv4) - double click -> Check the 'Use the following IP address'. If 'IP address' field is empty, enter the IP: `192.168.2.10`. Subnet mask: `255.255.255.0`. -> Check the 'Use the following DNS server addresses:'. Preferred DNS server: `127.0.0.1`. -> Click OK. 
- In the browser -> go to -> `http://192.168.2.99:9090` (`192.168.2.99` is the IP address of the Pynq Z2 board, Jupyter server can be accessed using port 9090)
- Jupyter login page loads -> Enter password: xilinx -> Login
- Files/Running/Clusters page loads. 

## Blinking LED on a GPIO
- Open Jupyter Notebook: Access Jupyter Notebook as described above.
- Create a new notebook: Click on "New" and select "Python 3" to create a new Python 3 notebook -> Rename it to your project name.

The Python code below utilizes the PYNQ framework to control an LED connected to pin 7 (AR7) of the Arduino header on the PYNQ-Z2 board.

1. Import Necessary Modules: This imports the `BaseOverlay` class from the `pynq.overlays.base` module, the `Arduino_IO` class from the `pynq.lib.arduino` module, and the `sleep` function from the `time` module.
```python
from pynq.overlays.base import BaseOverlay
from pynq.lib.arduino import Arduino_IO
from time import sleep
```
2. Load the Base Overlay: This line initializes the base overlay of the PYNQ-Z2 board by loading the bitstream file named "base.bit". The base overlay provides access to various peripherals, including Arduino GPIO pins.
```python
base = BaseOverlay("base.bit")
```
3. Access GPIO Object: This line creates an `Arduino_IO` object named `arduino_pin_d0`, representing the GPIO pin connected to pin 7 (AR7) of the Arduino header. The third argument `'out'` indicates that the pin will be used as an output.
```python
arduino_pin_d0 = Arduino_IO(base.ARDUINO, 7, 'out')
```
4. Main Loop: This creates an infinite loop, ensuring that the code inside the loop runs continuously.
```python
While True:
```
5. Toggled LED State: Within the loop, these lines of code toggle the state of the LED connected to pin 7 (AR7) of the Arduino header.
- `arduino_pin_d0.write(1)` sets the pin to HIGH, turning the LED on.
- `sleep(0.5)` pauses execution for 0.5 seconds.
- `arduino_pin_d0.write(0)` sets the pin to LOW, turning the LED off.
- `sleep(0.5)` pauses execution for another 0.5 seconds.
```python
arduino_pin_d0.write(1)  # Turn on LED
sleep(0.5)               # Wait for 0.5 seconds
arduino_pin_d0.write(0)  # Turn off LED
sleep(0.5)               # Wait for 0.5 seconds
```
By repeating this sequence indefinitely within the loop, the LED connected to pin 7 (AR7) will blink on and off at a 1-second interval (0.5 seconds on, 0.5 seconds off).

## Control the User LED's using Switches on the board

The Python code below utilizes the PYNQ framework to control the LEDs and switches on the PYNQ-Z2 board.
1. Import Necessary Modules: This imports the `sleep` function from the `time` module and the `BaseOverlay` class from the `pynq.overlays.base` module.
```python
from time import sleep
from pynq.overlays.base import BaseOverlay
```
2. Load the Base Overlay: This imports the `sleep` function from the `time` module and the `BaseOverlay` class from the `pynq.overlays.base` module.
```python
base = BaseOverlay("base.bit")
```
3. Assign LED and Switch Objects: This assigns LED objects to variables `led0`, `led1`, `led2`, and `led3`, representing the four LEDs on the PYNQ-Z2 board 
```python
led0 = base.leds[0]
led1 = base.leds[1]
led2 = base.leds[2]
led3 = base.leds[3]
```
This assigns switch objects to variables `sw0` and `sw1`, representing the two switches on the PYNQ-Z2 board.
```python
sw0 = base.switches[0]
sw1 = base.switches[1]
```
4. Main Loop: This creates an infinite loop, ensuring that the code inside the loop runs continuously.
```python
while(True):
```
5. Switch State Check: This checks if switch SW0 is in the ON position. `sw0.read()` reads the state of switch SW0, and if it's `True`, it means the switch is ON.
```python
if (sw0.read() == True):
```
6. LED Control Based on Switch State: If switch SW0 is ON, this turns on LEDs LD0 and LD1 using the `on()` method of the LED objects.
```python
led0.on()
led1.on()
```
7. LED Control for Switch SW1: Similarly, the code checks the state of switch SW1 and controls LEDs LD2 and LD3 accordingly.
8. LED Turn Off: If switch SW0 is not ON, this turns off LEDs LD0 and LD1 using the `off()` method of the LED objects.
```python	
led0.off()
led1.off()
```
9. Sleep: This line introduces a small delay of 0.1 seconds before the loop iterates again. This delay helps in reducing the processing load and makes the LED blinking more perceptible.
```python
sleep(0.1)
```
This code continuously monitors the state of switches SW0 and SW1 on the PYNQ-Z2 board. If SW0 is ON, it turns on LEDs LD0 and LD1, and if SW1 is ON, it turns on LEDs LD2 and LD3. If the switches are not ON, it turns off the respective LEDs. This behavior allows users to control the LEDs using the switches on the board.

## Binary Tree on Pynq-Z2
