# Lab Manual

### Table of Contents
 * [Lab 1: Blink LED](#lab-1-blink-led)
 * [Lab 2: Serial Communications](#lab-2-serial-communications)
 * [Lab 3: Sense Analog Read Temperature](#lab-3-sense-analog-read-temperature)
 * [Lab 4: Control LED Strip](#lab-4-control-led-strip)
 * [Lab 5: Arduino Firmata and Node.js: Read Temperature](#lab-5-arduino-firmata-and-nodejs-read-temperature)
 * [Lab 6: Firmata and LED Strip](#lab-6-firmata-and-led-strip)
 * [Lab 7: DEMO: Tying It All Together](#lab-7-demo-tying-it-all-together)

## Lab 1: Blink LED

### Lab 1: Procedure

* Start up Arduino IDE
* Open up the "blink.ino" source file from:
 `C:\MASTERs\20003\arduino\blink\`
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
digitalWrite(pin, val)
```

Sets a digital pin to specified state

```c
delay(ms)
```

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
Serial.begin(baud)
```

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
### Lab 3: Core Runtime Functions

```c
analogRead(pin)
```

Returns A/D converter value for specified pin  

```c
analogWrite(pin, val)
```

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
## Lab 3: Code
```c
float tempValue;

void setup() {
  Serial.begin(9600);
  delay(5000);
  Serial.println("ON");
}

void loop() {
  tempValue = analogRead(A0);
  tempValue = tempValue * 330 / 1024;
  Serial.print("Temp: ");
  Serial.println(tempValue);
  delay(250);
}
```

## Lab 3: Summary
* Analog input from temp sensor
* Numeric conversion
* Printing human-readable output to PC over UBS using Serial Monitor


# Lab 4: Control LED Strip
## Lab 4: Activity

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


## Lab 4: Procedure

* Open up the ‘hellodotstar.ino’ source file from: `C:\MASTERs\20003\arduino\hellodotstar\`

* Put Fubarino Mini in bootloader mode (while holding PRG, press/release RESET)

* Click Upload button in IDE

* After compile/upload complete, click watch LED strip, and examine code 

* Press RESET button on FB Mini to see pattern start over again


## Lab 4: Code
```cpp
#include <Adafruit_DotStar.h>
#include <SPI.h>

#define DATAPIN    MISO
#define CLOCKPIN   MOSI
#define NUMPIXELS    30  // Number of LEDs in strip

#define RED    0xFF0000
#define GREEN  0x00FF00
#define BLUE   0x0000FF

uint32_t frame[NUMPIXELS];
uint32_t colors[3] = {RED, GREEN, BLUE };

Adafruit_DotStar strip = Adafruit_DotStar(NUMPIXELS, DATAPIN, CLOCKPIN, DOTSTAR_BGR);

void setup() {
  strip.begin(); // Initialize pins for output
  strip.show();  // Turn all LEDs off ASAP

  //initialize strip array
  for (int i = 0; i < NUMPIXELS; i++) {
    frame[i] = RED; //initialize all to red
    strip.setPixelColor(i, frame[i]);
  }
  strip.show();
  delay(1000);
  //initialize strip array
  for (int i = 0; i < NUMPIXELS; i++) {
    frame[i] = GREEN; //initialize all to GREEN
    strip.setPixelColor(i, frame[i]);
  }
  strip.show();
  delay(1000);
  //initialize strip array
  for (int i = 0; i < NUMPIXELS; i++) {
    frame[i] = BLUE; //initialize all to BLUE
    strip.setPixelColor(i, frame[i]);
  }
  strip.show();
  delay(1000);
}

void loop() {
  /*
    for (int ii = 0; ii < NUMPIXELS; ii++) {
     frame[ii] = colors[ii % 3]; //initialize all to red
     strip.setPixelColor(ii, frame[ii]);
    }
    strip.show();
  */

  for (int i = 0; i <= NUMPIXELS; i++) {
    if (i == 0) {
      frame[i] = 0xFFFFFF;
      strip.setPixelColor(i, frame[i]);
      strip.show();
      delay(100);
    }
    else {
      frame[i - 1] = 0x000000;
      frame[i] = 0xFFFFFF;
      strip.setPixelColor(i - 1, frame[i - 1]);
      strip.setPixelColor(i, frame[i]);
      strip.show();
      delay(100);
    }

    for (int i = NUMPIXELS; i < 0; i--) {
      if (i == 30) {
        frame[i] = 0xFFFFFF;
        strip.setPixelColor(i, frame[i]);
        strip.show();
        delay(100);
      }
      else {
        frame[i + 1] = 0x000000;
        frame[i] = 0xFFFFFF;
        strip.setPixelColor(i + 1, frame[i + 1]);
        strip.setPixelColor(i, frame[i]);
        strip.show();
        delay(100);
      }
    }
  }
}

```
## Lab 4: Summary

* Control LED strip directly with simple commands in sketch

* Use Adafruit DotStar library in sketch to enable advanced functionality

* Understand how to load external libraries using Library Manager


# Lab 5: Arduino Firmata and Node.js: Read Temperature
## Lab 5: Activity
* Set's the event interval and checks the temperature.
* Can be modified to only show temperature 'on change'

## Lab 5: Code
```c
var config = require('config');
var five = require("johnny-five");
var board = new five.Board({
    port: config.get('port')
});

board.on("ready", function() {
    // Assuming a sensor is attached to pin "A0"
    this.pinMode(0, five.Pin.ANALOG);
    this.analogRead(0, function(value) {
        var celsiusValue = value * 330 / 1024;
        var farenValue = celsiusValue * 9 / 5 + 32;
        console.log("Analog: " + value + " , C: " + celsiusValue.toPrecision(3) + ", F: " + farenValue);
    });
});

```
## Lab 5: Summary
* Create an analog sensor with Johnny Five
* Use firmata to read analog data from an Arduno board

# Lab 6: Firmata and LED Strip
## Lab 6: Activity
* Creates a green led and traverse the led strip.
* Can be modified for color or more pixels added to animation.

## Lab 6: Code
```c
var config = require('config');
config.get('port') = "/dev/cu.usbmodem1411";
var Board = require("firmata");
var board = new Board(config.get('port'));
var repl = require('repl');

//var board = new Board({port: "COM15"});

var CK_COMMAND = 0x40;
var CK_PIXEL_SET = 0x10;
var CK_PIXEL_SHOW = 0x11;
var CK_PIXEL_CLEAR = 0x12;
var CK_PIXEL_BRIGHTNESS = 0x13;
var PIXEL = 1;
var PIXEL_MAX = 30;
var RED = 0;
var GREEN = 0;
var BLUE = 0;

repl.start('firmata>').context.board = board;

var packColor = function(red, green, blue) {
    red &= 0xFF;
    green &= 0xFF;
    blue &= 0xFF;
    var b1 = red >> 1;
    var b2 = ((red & 0x01) << 6) | (green >> 2);
    var b3 = ((green & 0x03) << 5) | (blue >> 3);
    var b4 = (blue & 0x07) << 4;
    var data = [b1, b2, b3, b4];
    console.log(data);
    return data;
};

board.on('ready', function() {
    console.log("START STRIP");
    // board.sysexCommand([CK_COMMAND, CK_PIXEL_CLEAR]);
    // board.sysexCommand([CK_COMMAND, CK_PIXEL_SHOW]);
    // board.sysexCommand([CK_COMMAND]);

    setInterval(function() {
        board.sysexCommand([CK_COMMAND, CK_PIXEL_CLEAR]);
        board.sysexCommand([CK_COMMAND, CK_PIXEL_SHOW]);
        board.sysexCommand([CK_COMMAND, CK_PIXEL_SET, PIXEL++].concat(packColor(RED, GREEN, BLUE)));
        board.sysexCommand([CK_COMMAND, CK_PIXEL_SHOW]);
        console.log(PIXEL);
        if (GREEN < 255) {
            GREEN++;
        }
        if (PIXEL > PIXEL_MAX) {
            PIXEL = 0;
        }
    }, 25);
});

```

## Lab 6: Summary
* Demonstrated the use of custom firmata commands  
* Use custom firmata commands from firmata.js
* These commands control the animation of an led strip from a desktop computer


# Lab 7: DEMO: Tying It All Together

* Firmata code to enable remote access and board level features
* Web accessible device control for temperature and LED strips

## Lab 7: Activity
* Use a full stack development framework to interact with  devices connected to t he server.

## Lab 7: Procedure
From the node.js command line run:
```node server-io.js```

## Lab 7: Summary

* We can send the board meta data to the client.
* Create temperature sensing app on the functionality
* Create a multi user led strip using standard full stack developer resources
