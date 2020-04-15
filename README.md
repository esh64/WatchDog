# WatchDog
A analog Watchdog circuit for Raspberry Pi and others Single Board Computers. It's not safe to count on Digital Circuits when facing extreme situations like high temperatures. It's make more critical when your computer is running remote and you cant' reset as you please when it freezes. The solution for this problem is a NE555-based Watchdog that is 100% analog and can face high temperatures and voltages.

# Why not using a microcontroller?
An Arduino board could be used for the watchdog, but it is way more expensive and it's a digital circuit. That means it would stop working properly when in high temperatures wich make it's useless in critical situations like in remote IOT aplications where you can't simply touch your Raspberry Pi board to reset when it freezes. Besides, the arduino board will require an power supply when the 555Circuit can simply be powered by the Raspberry PI

# How does this works?
There is two main components in the project:
  ![Inversion gate](https://raw.githubusercontent.com/esh64/WatchDog/master/invesionGate.png)
  * The logic inversion that truns high(3.3V) signal into low(0V) and low(0) signal into high(3.3V) signal. Its like a NOT logic gate port. 
  In this configuration the input signal in the base terminal must be 0V or 3.3V.
     * When it's 0V the transistor is in cut because the current in base and emitter terminals is both zeroes which means that the current in colector terminal is also zero, so the the voltage in the collector terminal is 3.3V because there is no current flowing throught the 10K resistor.
     * When it's 3.3V the transistor is in saturation region. This happens because base voltage is greater than emitter and collector voltage. When this happens there is a short circuit between collector terminal and emitter terminal that makes the 10k resistor dissipates the entirer voltage making the collector terminal voltage equals the emitter terminal wich is zero.
  ![555 IC](https://raw.githubusercontent.com/esh64/WatchDog/master/Screenshot_20200415_112907.png)
  * The 555 IC in the astable configuration:
    * There is a [great video on Youtube](https://youtu.be/i0SNb__dkYI?t=415) that explains how this IC works and how to project it for the right amount of time
  
  * When the 555 IC input(trigger port) is high, the capacitor will be fullied, so the times to discharges is reseted. Thats why the Raspberry Pi outpout must be permuting between 0 and 3.3V. Every time it's 3.3V it will charges the capactior and don't let it discharges, when it's 0V the capacitor will discharges.
  * So the Raspberry Pi can freezes when the output pin is high or low and thats why we need two 555 IC. One will receives the original input and the other will receives the inverse input so when the raspberry pi freezes with the output signal in 3.3V the negate input will be 0V allowing the capacitor to discharge
  * At last the two 555 IC output will be feeded at an inversion gate. it's works like a or logic gate. The diodes make sure that the current flow is unique. This "not logic gate" is necessary because the Raspberry Pi will turn off when the run pin is low.
  
  ![Or gate](https://raw.githubusercontent.com/esh64/WatchDog/master/Screenshot_20200415_115037.png)

# How to connect the circuit to the Raspberry Pi Port
  
  * Power on the circuit with 3.3V, this can be done by the 3.3V Raspberry Pi.
  * Connect the Raspberry Pi ground pin to the circuit's ground.
  * Chooses one GPIO Raspberry Pi to be the input signal and connect it to the circuit's input.
  * Connect the Circuits' output to the Raspberry Pi Run hole.
  
