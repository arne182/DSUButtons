-
# Reenable DSU Buttons and Remotely Disable DSU on Rav4
I noticed the following distance and Lane Keeping Alert buttons no longer did anything after installing Open Pilot and disabling the DSU. I wanted them back. So through some help from the internet and Comma Slack folks, I figured out what was needed.

## Hardware List (see bottom for links)
* Arduino (I used a Duo)
* CAN bus SPI board
* Connectors and Wires
* Relay board if trying to disable/enable DSU

## Location
Just about everything is done behind the glove compartment. Squeeze in the sides of the glove compartment until it comes loose, then disconnect the arm holding it in by pulling it to the left. Then just lift it out.

The DSU is the left-most large connector you can see in the middle of where the glove compartment was removed.

Here's the connector. I've highlighted the pins for the buttons:
* 28 GND
* 23 Distance Control
*  5 Lane Keeping Assist Switch
* 10 Can2 High
* 11 Can2 Low
*  7 12V

![alt text](https://github.com/joeljacobs/DSUButtons/blob/master/Images/DSU_Connector.png "DSU Connector")

The buttons simply Conect to Ground when you press them. So this seemed like a good use for an Arduino.

But I also wanted my EON to be able to communicate with it, so CAN was the natural solution.
I already had a few CAN boards that use the SPI protocol to communicate with Raspberry Pi's and Arduinos, so I used one of those.

It was then just a matter of connecting the pins to the arduino's digital IO, connecting the CAN board to the Arduino, and connecting the CAN board to the CAN bus in the same DSU connector. 

## Automatically Disconnect/Reconnect DSU
I have two cars that can use Open Pilot, and I have to move it between them sometimes. When I do, I need to re-enable the DSU (as well as changes some stuff on the EON) each time. I added some relays to allow that to be more automatic in this circuit as well. 

## Code
I've included some amateur code for the Arduino that reads the buttons, writes CAN and controls the relays. 

For the CAN Bus SPI board I bought, you have to use a custom library on the Arduino, since the normal code doesn't support the slower clock speed (8mhz.) Here it is:
https://github.com/Flori1989/MCP2515_lib

3D printed Part 1612050 
https://www.thingiverse.com/thing:3170223
Connector F 90980-12422
https://www.toyotapartsdeal.com/oem/toyota~housing~connector~f~90980-12422.html

### Hardware Links
#### CAN Module I used:
https://www.amazon.com/gp/product/B015W4D9WY/ref=oh_aui_detailpage_o01_s01?ie=UTF8&psc=1

#### CAN Arduino Shield that I should have probably used instead:
https://www.amazon.com/gp/product/B06XWQ4WF9/ref=oh_aui_detailpage_o05_s00?ie=UTF8&psc=1
#### Relay Board:
https://www.amazon.com/SunFounder-Channel-Optocoupler-Expansion-Raspberry/dp/B00E0NTPP4/ref=sr_1_3?ie=UTF8&qid=1520026106&sr=8-3&keywords=2+relay
