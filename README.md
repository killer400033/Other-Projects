### A collection of my most notable electronics projects

# Power Electronics

## Tesla Coil

The tesla coil went through many iterations but was always a CW SSTC (Continuous Wave Solid State Tesla Coil). 
The first iteration was simple, consisting of a half-bridge driven by an Arduino for signalling. This meant the tuning of the Tesla coil was always off, and would be drastically effected by objects near the topload. It was also only driven by a 60V external DC PSU.

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/9b3a312e-3d07-44d3-ad36-703b3fdbf5e5" width="400">

The second iteration only slightly improved the coil by increasing the top load size to a more standard shape. Additionally, the power was increased to ~120V DC using a large transformer plugged into the wall.

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/bf1afdfc-6f05-46eb-bf37-158e59911f10" width="400">

The third iteration however was where the largest difference was made, as after doing some research I decided to add a PLL feedback system to drive the coil rather than an Arduino. The PLL system was based on the 4046 IC, and I used phase comparator 2 meaning no phase shift along the VCO range. This meant that the coil tuning was much better, and wouldn't be affected by objects near the top load changing its capacitance. Additionally, this meant with the right tuning, I could achieve zero current switching drastically reducing ringing, hence allowing me to increase the input voltage all the way to ~330V DC (rectified wall power).

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/a3fab938-60de-4123-bd47-257b0c4cbd19" width="400">

However, due to the increased power output, noise problems started to appear. Hence I made the whole thing fool-proof. Firstly, I moved the PLL feedback circuit from a protoboard to a custom PCB (designed in EasyEDA and printed by JLCPCB) with full galvanic isolation between the power and logic side. Secondly, I added a metal shielding box around the control board for further noise isolation.

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/bd87070c-e410-43da-8b08-45bec3fb4cd0" width="400">
<img src="https://github.com/killer400033/Other-Projects/assets/37206981/20ad45cc-2a31-4a03-9f25-fd292fdbec68" width="400">

Now, with all the extra power, the secondary coil started overheating, so I was forced to make a much larger coil with thicker copper wire. This also meant upsizing the primary coil, topload etc.

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/fe2be34f-4235-431c-b9c6-1bb22d52f794" width="400">
<img src="https://github.com/killer400033/Other-Projects/assets/37206981/c6dd459c-5b32-4ac6-815e-4ee2220afc6d" width="400">

Along with this, I also upgraded the Half-bridge to a Full bridge, doubling the voltage across the primary. I also did some further research on how to reduce inductances in the Full bridge design, and finally built the following:

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/f1818620-9843-434b-b08b-fe8455f53976" width="400">

Hence, the latest iteration is able to produce pretty impressive arcs, however is prone to blowing up due to ringing in the MOSFETs.

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/d0ca7c86-2889-454d-b9fa-bd8acc099ad9" width="700">

# Embeded Systems (Code can be found above)

## DAC

Platform: Atmega328p/Atmega168p

This was a simple, fast DAC I made to convert a PWM signal into the "equivalent" analog signal. It worked by using Timer 1 in input capture mode to capture the PWM signal pulse width, and then write the corresponding values to 8 digital out pins connected to a resistor ladder, making this an 8-bit DAC. The only problem with this was whenever the PWM signal was at 0% or 100%, resulting in no edges for Timer 1 to capture. And as the whole thing is interrupt driven, this would cause the DAC to glitch/ stutter.

## ESC

Platform: Atmega328p/Atmega168p

This was my first attempt at making BLDC motor driver. It uses 6 step commutation. For the driver circuitry I used the ir2113 and some N Channel MOSFETs. With this setup I had individual outputs for both the high side and low side MOSFETS, but only 3 were PWM. Hence I implemented soft switching with the high side staying on and low side switching for any 1 of the 6 steps. The main code was interrupt-driven, where the positional hall effect sensor would cause an interrupt whenever the motor moved to the next step. But this meant the code wouldn't run if the motor wasn't moving, so a "startup sequence" was also added to get the motor moving.

## Keyboard

Platform: Seeduino

This was some code I wrote for my brother's keyboard which he had build, based on a Seeduino. I wrote a simple loop to check all the keys for presses and update accordingly. I also wrote an algorithm for some RGB effects, fully taking into account key positions for accurate positioning of the effects.

## Lathe raspberry pi and Lathe Lead Screw

Platform: Arduino DUE and Raspberry Pi

In highschool we got a Lathe, but it lacked most of the change gears in the back and so basically couldn't do any threading or power feed. Hence these 2 projects were made to motorise the lead screw. The Lathe_Raspberry_Pi project is the interface and communicates to the Arduino through I2C. It ran an full UI interface on a 800x600 monitor, with inputs done by a keypad as shown:

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/8dd1cb18-9010-4a77-a58f-709fabad939b" width="400">
<img src="https://github.com/killer400033/Other-Projects/assets/37206981/1ec9a15b-5f39-43e3-be79-5842f442d02b" width="400">
<img src="https://github.com/killer400033/Other-Projects/assets/37206981/61dffea5-2e77-4f9d-b79e-a160a078c375" width="400">

Once a setting was chosen using the keypad, the data would be sent to the Arduino Due using I2C, which would then send signals to a stepper motor whilst also keeping track of an encoder (Arduino Due had hardware encoder support which made this much faster). The encoder was connected to where the old change gears went, hence keeping track of the chuck position. With this, the Arduino due would apply a multiplier according to the given threading/ power feed ratio and spin the stepper motor accordingly. The motor and encoder setup can be seen below:

<img src="https://github.com/killer400033/Other-Projects/assets/37206981/9b5a8ff4-cf02-4533-b711-5e847d6fde43" width="600">

All in all, there were 3 modes to choose from; Metric Threading, Imperial Threading and Power Feed. In all modes, soft start and stop functionality was added to avoid the stepper motor skipping steps.

## Soldering Iron Display and Soldering Iron PWM

Platform: Atmega328p/Atmega168p

This was a super simple soldering iron station station I made, with one Arduino to control the PWM output, and another to control a display. I made it by opening a 24V Power supply and hijacking the switching MOSFETs gate signal, and driving it with the Arduino instead. I implemented a super simple control loop that sort of resembles a PI loop but isn't really one.

## TV Surround Sound Control

Platform: Atmega328p/Atmega168p

This was a TV sound system controller I made to control our "custom" sound system. It had both a physical interface (a laptop trackpad) and a wireless interface through a phone (achieved using Blynk). For the trackpad interface, I had to code some crude gesture recognition for sliding, circles and tapping. And then for feedback, there was an LED light strip/ circle made of WS2812 LEDs, which I also coded many animations for.
