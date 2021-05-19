# How to connect an Arduino to a real serial port (RS232)

# My custom adaptor circuit
## Computer TX to Arduino RX
For the challenge, I wanted to send data from my computer's real serial port (RS232 9-pin connector). For that, I created an adaptor circuit because the voltages on a computer serial port are different from what the Arduino expects. Don't connect the computer TX pin to the Arduino RX pin directly, it would fry your Arduino according to the documentation (https://www.arduino.cc/reference/en/language/functions/communication/serial/). 

You can watch my YouTube video where I show the circuit working: https://www.youtube.com/watch?v=u4Crk8dcw9A

This adaptor circuit translates -12V (logical 1 in RS232) to 5V and +12V (logical 0) to 0V.

Diagram:
![Circuit diagram](/circuit/diagram.jpg?raw=true)
I have represented the Arduino RX pin as a diode and resistor just to think about the circuit, but the actual hardware inside is probably different.

Photo:
![Circuit photo](/circuit/photo_annotated.jpg?raw=true)

## Arduino TX to computer RX
Altough it is not used by this program, I also created the circuit to perform serial communication in the reverse direction, ie. from Arduino TX pin to computer RS232 RX pin.

This adaptor circuit translates 0V to 5V (understood as logical 0 in RS232) and 5V to -9V (understood as logical 1 in RS232).

Diagram:
![Circuit diagram](/circuit/diagram_TX.jpg?raw=true)
I have represented the Arduino TX pin as a switch to +5V/GND just to think about the circuit, but the actual hardware inside is probably different.

Photo:
![Circuit photo](/circuit/photo_annotated_TX.jpg?raw=true)
(The GND of the RS232 port is connected to the Arduino GND off camera)

# Alternative: Use a decdicated IC like MAX232
The most obvious way to do that is to use an IC designed specifically for that.

MAX232 is an IC which allows to convert from/to Arduino RX/TX to a computer serial port (RS232). With this circuit, you don't need a 9V battery like with my homemade circuit above. Here is the setup with a MAX232A. All capacitors are 0.1 µF here, as required by the MAX232A data sheet. If you have another MAX232 variant, the capcitors values might be different, check it in the data sheet.

![Circuit wiring with MAX232 - diagram](/circuit/MAX232_diagram.jpg?raw=true)

![Circuit wiring with MAX232 - photo](/circuit/MAX232_photo.jpg?raw=true)
