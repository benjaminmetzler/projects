
####Introduction
I'm spending a lot of times in virtual meetings while in isolation.  Apps like Teams and Zoom offer a way to connect with my co-workers during this time.  One on one that's fine, but during meetings I mute my microphone to keep others from hearing about my dog barking or some other sound.  

I picked up an app called [Talk Toggle](https://www.microsoft.com/en-us/p/talk-toggle/9nrjcs6g10kt#activetab=pivot:overviewtab) as it provides a system wide mute.  I set it to recognize F24 as its hotkey and then remapped capslock and menu key to send F24 with [AutoHotKey](https://www.autohotkey.com/).  

All that done I had a passing "push-to-talk" setup, but I kept forgetting to press the keyboard when I wanted to talk.  I realized that I needed something I could hold in my hands to force me to remember to press it.  A quick google and I saw that there were options but they all cost more then I wanted to pay or they didn't work like I wanted.  I decided to build my own PTT solution.

####Shopping List
1. [Handheld push button](https://www.ebay.com/itm/122657808383)
1. [Teensey USB Development Board (with pins)](https://www.pjrc.com/store/teensy_pins.html)
1. USB Cable - A-Male to Mini-B Cord.  The first one I tried was a "charger only" type.  Forturnately I had another.
1. Miscellanious wires.  I used an broken USB-C cable and some extra jumper wires I had laying around.
1. [Heat shrink tube](https://www.amazon.com/560PCS-Heat-Shrink-Tubing-Eventronic/dp/B072PCQ2LW)

####Assembly
Connect the handheld push button to pins `B1` and `GND`.  I used jumper wires to avoid soldering to the teensy board.  Tie a knot in the cable near the push button to avoid pulling on the solder joints.

####Miscellanious links:

1. [link](https://www.pjrc.com/teensy/td_keyboard.html)
1. [link](https://timmyomahony.com/blog/making-usb-push-buttons/)

####Code
Start up [Teensyduino](https://www.pjrc.com/teensy/td_download.html) and load the below code onto the teensy.  It will 
````c
#include <Bounce.h>

Bounce buttonPTT = Bounce(PIN_B1, 10);

void setup() {
  pinMode(PIN_B1, INPUT_PULLUP); // PTT button
  pinMode(PIN_D6, OUTPUT); // LED
}

void loop() {
 
  buttonPTT.update();

  // Map PTT to F24
  if (buttonPTT.fallingEdge()) {
    Keyboard.press(KEY_F24);  
    digitalWrite(PIN_D6, HIGH); // LED ON
  }
  if(buttonPTT.risingEdge()){
    Keyboard.release(KEY_F24);
    digitalWrite(PIN_D6, LOW); // LED OFF
  }
}
````