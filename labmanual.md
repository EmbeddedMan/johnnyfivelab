# Lab Manual

### Table of Contents

 * [Lab 1: Blink LED](#Lab-1:-Blink-LED)
 * [Lab 2: Serial Communications]()
 * [Lab 3: Sense Analog Read Temperature]()
 * [Lab 4: Control LED Strip]()
 * [Lab 5: Arduino Firmata and Node.js: Read Temperature]()
 * [Lab 6: Firmata and LED Strip]()
 * [Lab 7: DEMO]()




## Lab 1: Blink LED
### Lab 1: Procedure

* Start up Arduino IDE
* Open up the ‘blink.ino’ source file from:
 ```C:\MASTERs\20003\arduino\blink\```
* Connect Fubarino Mini to USB port
* Put Fubarino Mini in bootloader mode (while holding PRG, press/release RESET)
* Make sure Menu item Tools --> Port has the correct serial port selected
* Click Upload button in IDE
* After compile/upload complete, confirm green LED on FB Mini is blinking


### Lab 1: Core Runtime Functions

```c
pinMode(pin, dir)
```

Sets pin direction and drive type

```c
digitalRead(pin)
```

Reads the state of a digital pin

```c
digitalWrite(pin, val)```

Sets a digital pin to specified state

```c
delay(ms)```

Delay for specified number of milliseconds


### Lab 1: Code – Blink LED
```c
//Setup the inputs and outputs and run once  
void setup() {
  pinMode(PIN_LED1, OUTPUT);
}  
// the loop function runs over and over again forever
void loop() {
  digitalWrite(PIN_LED1, HIGH);    
  delay(750);               
  digitalWrite(PIN_LED1, LOW);    
  delay(250);               
}
```

### Lab 1: Summary

* Compile and download source code to chipKIT board
* Learn basic code template
* Set pin as output
* Set output pin states
* Create a time delay




## Lab 2: Serial Communications
### Lab2: Activity:

Explore serial data output from chipKIT back to PC over USB, using Arduino IDE Serial Monitor

### Lab2: Procedure

* Open up the ‘helloserial.ino’ source file from:  `C:\MASTERs\20003\arduino\helloserial\`

* Put Fubarino Mini in bootloader mode (while holding PRG, press/release RESET)

* Click Upload button in IDE

* After compile/upload complete click Serial Monitor button in IDE

* Notice serial output on PC

### Lab 2: Core Runtime Functions


Builtin Library Hardware Serial

```c
Serial.begin(baud)```

Initialize the UART and set the baud rate  
```c
Serial.print(val)
```
Print the specified value to UART  
```c
Serial.read()
```
Read characters from UART



### Lab 2: Code – Hello World
```c
void setup() {
  Serial.begin(9600);  //init UART
  delay(5000);
}

void loop() {
  Serial.println();
  Serial.println("Hello World!");
  for (int i = 1; i <= 10; i++) {
    Serial.print(" i = ");
    Serial.println(i, DEC);
  }
  delay(5000); //wait five seconds
}
```

### Lab 2: Summary

Print human-readable output to PC over USB using Serial Monitor.


## Lab 3: Sense Analog Read Temperature
### Lab 3: Activity
### Lab 3: Core Runtime Functions*

```c
analogRead(pin)```

Returns A/D converter value for specified pin  

```c
analogWrite(pin, val)```

Pseudo-analog output using PWM. Sets output duty cycle to specified value

* Example (no lab) – Dim an LED

```c
int pinLed = 9;   //assume LED on pin 9
int pinPot = A0;  //assume pot on analog 0

void setup() {   //nothing needed*
}

void loop() {
 int val;
 val = analogRead(pinPot);
 analogWrite(pinLed, val/(1024/256));
 delay(1000);
}
```
This code could as use the map function to scale 10 bit input to 8 bit output:
```c
val = map(val, 0,1023,0,255);
```

## Lab 3: Activity
* Objective

Explore several different example programs that are available within Arduino IDE to manipulate an LED strip and learn about adding external libraries to your sketch

* Simple LED Strip Control

* Adding Libraries

* You can use the built in Library Manager to search for and easily add libraries to your Arduino IDE install

* Some libraries not available in Library Manager yet, need to download zip file from GitHub repo or website

* Adafruit DotStar Library: We will use this one to control our LED strip


### Using Libraries

* Pre-written libraries provide additional functionality

* If you add hardware to your design, there are often libraries already written to help you make use of the hardware

To include a libary:
* `#include <libraryname.h>`
This will add it to your sketch.

* chipKIT core includes highly optimized libraries for PIC32

* Example: Library Manager

* Adafruit DotStar Library

### Note: Concerns with Logic Levels

* Many existing Arduino boards are powered with 5v

chipKIT boards are all 3.3v

chipKIT boards are not all 5v tolerant

* Many new Arduino shields are 3.3v

* 5v boards cannot always be triggered by 3.3v logic

* Sending 5v into a 3.3v input can kill the device


## Lab 3: Procedure

* Open up the ‘hellodotstar.ino’ source file from: `C:\MASTERs\20003\arduino\hellodotstar\`

* Put Fubarino Mini in bootloader mode (while holding PRG, press/release RESET)

* Click Upload button in IDE

* After compile/upload complete, click watch LED strip, and examine code 

Press RESET button on FB Mini to see pattern start over again


## Lab 3: Code
## Lab 3: Summary



# Lab 4: Control LED Strip
## Lab 4: Activity
## Lab 4: Code
## Lab 4: Summary

# Lab 5: Arduino Firmata and Node.js: Read Temperature
## Lab 5: Activity
## Lab 5: Code
## Lab 5: Summary

# Lab 6: Firmata and LED Strip
## Lab 6: Activity
## Lab 6: Code
## Lab 6: Summary
# Lab 7: DEMO




##Lab 3:
**Lab 3 Activity**

**Objective:**


**Lab 3 Procedure**

**Lab 3 Code – Control LED**

**#include &lt;Adafruit_DotStar.h&gt;**

**#include &lt;SPI.h&gt;**

**#define DATAPIN    MISO**

**#define CLOCKPIN   MOSI**

**#define NUMPIXELS    30  // Num LEDs in strip**

**#define RED    0xFF0000**

**#define GREEN  0x00FF00**

**#define BLUE   0x0000FF**

**uint32_t frame[NUMPIXELS];**

**uint32_t colors[3] = {RED, GREEN, BLUE };**

**Adafruit_DotStar strip = Adafruit_DotStar(NUMPIXELS, DATAPIN, CLOCKPIN, DOTSTAR_BGR);**

**void setup() {**

**  strip.begin(); // Initialize pins for output**

**  strip.show();  // Turn all LEDs off ASAP**

**  //initialize strip array**

**  for (int i = 0; i &lt; NUMPIXELS; i++) {**

**    frame[i] = RED; //initialize all to red**

**    strip.setPixelColor(i, frame[i]);**

**  }**

**  strip.show();**

**  delay(1000);**

**  //initialize strip array**

**  for (int i = 0; i &lt; NUMPIXELS; i++) {**

**    frame[i] = GREEN; //initialize all to GREEN**

**    strip.setPixelColor(i, frame[i]);**

**  }**

**  strip.show();**

**  delay(1000);**

**  //initialize strip array**

**  for (int i = 0; i &lt; NUMPIXELS; i++) {**

**    frame[i] = BLUE; //initialize all to BLUE**

**    strip.setPixelColor(i, frame[i]);**

**  }**

**  strip.show();**

**  delay(1000);**

**}**

**Lab 3 Code – Control LED**

**void loop() {**

**  /***

**    for (int ii = 0; ii &lt; NUMPIXELS; ii++) {**

**     frame[ii] = colors[ii % 3]; // all to red**

**     strip.setPixelColor(ii, frame[ii]);**

**    }**

**    strip.show();**

**  */**

**  for (int i = 0; i &lt;= NUMPIXELS; i++) {**

**    if (i == 0) {**

**      frame[i] = 0xFFFFFF;**

**      strip.setPixelColor(i, frame[i]);**

**      strip.show();**

**      delay(100);**

**    }**

**    else {**

**      frame[i - 1] = 0x000000;**

**      frame[i] = 0xFFFFFF;**

**      strip.setPixelColor(i - 1, frame[i - 1]);**

**      strip.setPixelColor(i, frame[i]);**

**      strip.show();**

**      delay(100);**

**    }**

**  
  **

**    for (int i = NUMPIXELS; i &lt; 0; i--) {**

**      if (i == 30) {**

**        frame[i] = 0xFFFFFF;**

**        strip.setPixelColor(i, frame[i]);**

**        strip.show();**

**        delay(100);**

**      }**

**      else {**

**        frame[i + 1] = 0x000000;**

**        frame[i] = 0xFFFFFF;**

**        strip.setPixelColor(i + 1, frame[i + 1]);**

**        strip.setPixelColor(i, frame[i]);**

**        strip.show();**

**        delay(100);**

**      }**

**    }**

**  }**

**}**

**Lab 3 Summary**

**Control LED strip directly with simple commands in sketch**

**Use Adafruit DotStar library in sketch to enable advanced functionality**

**Understand how to load external libraries using Library Manager**

**DEMO: Tying It All Together**

**The demo combines:**

Firmata code to enable remote access and board level features

Web accessible device control for temperature and LED strips