![PTT](./PTT1.png)

#### Introduction
During this whole corona-virus/isolation period, I'm spending a lot of time in virtual meetings.  Apps like Teams and Zoom offer a way to connect with my co-workers.  

For one-on-one meetings an open mic is fine but during group meetings I keep my mic muted unless I’m talking. This keeps my background noise from interrupting the speaker and is generally good form in group meetings.

I picked up an app called [Talk Toggle](https://www.microsoft.com/en-us/p/talk-toggle/9nrjcs6g10kt#activetab=pivot:overviewtab) to provide system wide Push-To-Talk functionality and set it to recognize F24 as its hotkey and remapped the menu key to send F24 with [AutoHotKey](https://www.autohotkey.com/).  

While I had a passing "push-to-talk" setup I kept forgetting to press the unmute hotkey when I started talking.  I needed something I could hold in my hands to force as a reminder.  A quick google and I saw that there were options but they all cost more then I wanted to pay or didn't work like I wanted.  I decided to build my own PTT solution.

#### Shopping List
1. [Handheld push button](https://www.ebay.com/itm/122657808383)
1. [Teensy USB Development Board (with pins)](https://www.pjrc.com/store/teensy_pins.html)
1. [USB Cable - A-Male to Mini-B Cord](https://www.pjrc.com/store/cable_usb_micro_b.html)
1. Miscellaneous wires.  I used an broken USB-C cable and some wires I had laying around.
1. [Heat shrink tube](https://www.amazon.com/560PCS-Heat-Shrink-Tubing-Eventronic/dp/B072PCQ2LW)

#### Assembly
Connect wires between the handheld push button poles and  pins `B1` and `GND`.  This is a simple switch so it doesn't matter which wire is connected to which pin.  I used a breadboard and some jumper wires to avoid soldering to the teensy board.  Heat shrink up any soldered wires too keep things safe and looking clean. 

#### Miscellanious links
1. [Inspiration](https://timmyomahony.com/blog/making-usb-push-buttons/)
1. [Parts](https://www.pjrc.com/teensy/td_keyboard.html)

#### Code
Start up [Teensyduino](https://www.pjrc.com/teensy/td_download.html) and load the below code onto the teensy.  It will detect when B1 is closed and will press `F24`.  With the above Talk Toggle, this will unmute the mics.  Release the PTT button and the teensy will release `F24`, reactivating mic mute.

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

#### Next Steps
Using the teensy for this project is a bit of overkill but I have plans.  I am going to make a macro-keyboard, similar to the [DIY Stream Deck](https://www.partsnotincluded.com/diy-stream-deck-mini-macro-keyboard/).  With it I will be able to switch between [OBS](https://obsproject.com/) streams and enable notification lights.
