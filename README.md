# How to connect an Arduino to a real serial port (RS232)
For the challenge, I wanted to send data from my computer's real serial port (RS232 9-pin connector). For that, I created an adaptor circuit because the voltages on a computer serial port are different from what the Arduino expects. Don't connect the computer TX pin to the Arduino RX pin directly, it would fry your Arduino according to the documentation (https://www.arduino.cc/reference/en/language/functions/communication/serial/). 

You can watch my YouTube video where I show the circuit working: https://www.youtube.com/watch?v=u4Crk8dcw9A

The goal of these circuits is to convert voltages between the two incompatible standards:
* negative voltage in RS232 (any value from −15 to −3 V) <=> 5V in Arduino
* positive voltage in RS232 (any value from +3 to +15 V) <=> 0V in Arduino

I present my own adapter circuit and compare them to the "standard" alternative which is to use a dedicated IC like MAX232A for instance (see bottom of this page for how to use this IC).

# My custom adaptor circuit: computer TX to Arduino RX
This direction is quite easy and requires little components. All you need is a few resistors and PNP transistors.

Diagram:
![Circuit diagram](/circuit/diagram.jpg?raw=true)
I have represented the Arduino RX pin as a diode and resistor just to think about the circuit, but the actual hardware inside is probably different.

Here is what the signal looks like (top = input signal from the computer / bottom = output signal from my circuit to the Arduino). Measure was taken at 115200 bauds.
![Input and output signals measured](/benchmarks/RS232%20to%20Arduino/115200%20bauds%20with%20custom.jpg?raw=true)
The output looks very similar to what you would get with a MAX232A IC (look at the files in the repo if you want to see the other screenshot).

# My custom adaptor circuit: Arduino TX to computer RX
This adaptor circuit translates 0V to 5V (understood as logical 0 in RS232) and 5V to -9V (understood as logical 1 in RS232).

What makes it difficult is the fact that you need to provide negative voltage (which is why I use an 9V battery). I am to fully happy with this circuit, as the 100 ohm resistor heats a lot. Use it at your own risk. See below for a "cheating" version which does not have this issue.

Diagram:
![Circuit diagram](/circuit/diagram_TX.jpg?raw=true)
I have represented the Arduino TX pin as a switch to +5V/GND just to think about the circuit, but the actual hardware inside is probably different.

Photo:
![Circuit photo](/circuit/photo_annotated_TX.jpg?raw=true)
(The GND of the RS232 port is connected to the Arduino GND off camera)

# My "cheating" adaptor circuit for Arduino TX to computer RX
I noticed that in practice my computer consider 0V to be equivalent to a negative voltage, so we can simply invert the signal (5V to 0V / 0V to 5V) and it works perfectly in my case!

This is the result (top = input signal from Arduino / bottom = output signal from my circuit to the computer):
![Input and output signals measured](/benchmarks/Arduino%20to%20RS232/9600%20bauds%20with%20custom%20cheating.jpg?raw=true)

This is actually cleaner than with a MAX232A, which gives this output (top = input signal from Arduino / bottom = output signal from MAX232A to the computer).
![Input and output signals measured](/benchmarks/Arduino%20to%20RS232/9600%20bauds%20with%20custom%20cheating.jpg?raw=true)
Setup used for this measure is shown below.

# Alternative: Use a decdicated IC like MAX232
The most obvious way to do that is to use an IC designed specifically for that.

MAX232 is an IC which allows to convert from/to Arduino RX/TX to a computer serial port (RS232). With this circuit, you don't need a 9V battery like with my homemade circuit above (unless you use the "cheating" version). Here is the setup with a MAX232A. All capacitors are 0.1 µF here, as required by the MAX232A data sheet. If you have another MAX232 variant, the capcitors values might be different, check it in the data sheet.

![Circuit wiring with MAX232 - diagram](/circuit/MAX232_diagram.jpg?raw=true)

![Circuit wiring with MAX232 - photo](/circuit/MAX232_photo.jpg?raw=true)
