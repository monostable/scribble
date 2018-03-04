Over the past few years I have been using the Arduino ecosystem more and more for prototyping in a professional setting.

I have been working on software for the Braille e-book reader prototypes for [Bristol Braille Technology](http://bristolbraille.co.uk) and I have finalised the firmware of the inital Kickstarter batch of the [VRGO Chair](http://vrgochair.com) virtual reality controller.

![](/images/entry4/vrgo-braille-3.png)

There was a time when I would dismiss Arduino code and as unprofessional and "just for hobby projects" but it has some strong points in its favour that made me abandon these preconceptions long ago:

- A packaged development environment that even a mechanical engineer can install and use.
- A vast selection of re-usable code: any particular module that you think may be suitable for your prototype likely has an open source library or example project for Arduino.
- Abstractions that save time on looking up documentation and writing boilerplate code.
- The abstractions also help portability. Code that makes use of the Arduino libraries can be used over a wide range of processors (from 8-bit low clock-speed [MSP430](http://energia.nu/) and AVR to powerful ARM Cortex M4 in the form of the Arduino Due and [Teensy](https://www.pjrc.com/teensy/index.html) boards).

There are downsides of course:

- A packaged development environment that anyone that cares the least bit about code formatting will perceive as torture.
- You lose some control and performance and the abstraction layer can become a source of confusion (e.g. PIN2 on DDRD on your AVR, as it is in the schematic becomes pin number 0 in the Arduino world, but it depends on your processor)
- While your code is potentially portable it isn't guaranteed. The devil is in the details of what hardware and processor specific things your code, or your libraries code, makes use of.

There are ways to mitigate these issues though and I want to outline some of the coping mechanisms I have been using that have let me use Arduino in a professional setting.

These techniques have, so far, let me not abandon my code as we move from prototypes to products but let me extend it and let me keep using the flexibility and development speed of the "Arduino way" to my advantage.

## A sensible environment

If you spend a lot of time writing code you should spend a lot of time getting to know your text-editor. The one embedded inside the cute Arduino IDE is not a good one and you should abandon it with haste. [Vim](http://www.vim.org/) and [Emacs](https://www.gnu.org/software/emacs/) are the grumpy old men, [Atom](https://atom.io/) and [VSCode](https://code.visualstudio.com/) are the new kids on the block, but there are millions of good ones, why would you use the Arduino IDE to edit code?

There is a little preference that I always tick when I install the Arduino IDE. It's called "Use external editor" and it makes the area with the code go grey and un-editable and refresh whenever the file is saved by something else. I simply use the IDE to compile and upload the code and edit the code itself with Vim.

![](/images/entry4/use_external_editor.png)

As you code grows even this limited use of the IDE may become annoying in which case I thoroughly recommend switching the project over to [PlatformIO](http://platformio.org/). Ejecting your Arduino project from the IDE to PlatformIO should be straightforward. If you are using the Uno for instance running `pio init --board uno` and moving all the source code into the `src/` directory should get you there.

## Let's make it modular

Often people seem to be unaware that Arduino is just C++. More accurately you are using C++ and the Arduino C++ framework.

C++ was designed with huge collaborative software projects in mind and some of its features can seem cumbersome when working on small embedded systems and daunting if you have been learning to code by playing with Arduino.

C++ also has a `class` hammer and too often I come across code that has `class`ed all the things for no particular reason other than that seems to be what you are supposed to do in C++. In my experience code like this easily looses sight of the relationships it's trying to express.

Classes are a language feature for expressing something that exists more than once. A class is really good [when you want two thousand of something](https://youtu.be/J-zFQ9fOTSU?t=35s) and many consider a singleton class, a class with a single instance, to be something to avoid.

In embedded systems you are often building abstractions around resources, say the internal EEPROM or a motor controller or LED attached to particular pins, where it is _dictated by the real world_ that there only ever is one thing and there could never possibly be a possibility to instantiate another.

I don't quite think singleton classes should be avoided at all costs. Classes provide other benefits such as bound methods and encapsulation but I don't introduce a class to my code unless there is a really good reason to. What I use instead are namespaced modules.

Most higher level languages have adopted module systems where you can split your code into files and `import` or `require` your files as desired. C++ has inherited a two file system from C.  `.cpp` is the main source file and `.h` is the header file.

You can declare prototypes, re-confirming the types from your function declarations, in your header files so that you can use them in your `.cpp` file in any order you like. I find that the repetition in `.h` and `.cpp` become cumbersome and slows me down so I opt for single `.h` file modules and namespaces for encapsulation within the modules.

The one module name that has become common in all my projects is pins.h where all the pins used on my Arduino compatible board are mapped out.

```cpp
// pins.h
namespace Pins {
    enum {
        LED = 13,
        BATTERY_LEVEL = A0,
        // ...
    }
}
```

As things are added to you main `.ino` file often simply moving things out of `setup` and `loop` can be a natural start to a new module.

A contrived example:

```cpp
// example.ino
#include "pins.h"
#include "led.h"
#include "some_other_thing.h"

void setup() {
   Led::setup();
   SomeOtherThing::setup();
   // ...
}

void loop() {
   Led::loop();
   SomeOtherThing::loop();
   // ...
}
```

```cpp
// led.h
namespace Led {
     void setup() {
        pinMode(Pins::LED, OUTPUT);
     }

    void loop() {
       digitalWrite(Pins::LED, HIGH);
       delay(500);
       digitalWrite(Pins::LED, LOW);
       delay(500);
    }
}
```


The caveat here is you need to pay attention to your imports and the order your functions are declared in within your `.h` files. It's not quite like module systems of more higher level languages but it's pretty damn close.

## Conclusion

Too often do I see Arduino being disparaged as "not professional" when it is just a convenient way to compile your C++ code and upload it to a variety of development boards. Often people use it as an excuse to re-write everything from scratch, and more often than not [that's a really bad idea](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/).

You can adapt you development environment to be more pleasant and adopt development practices that helps you write clean and modular code. This lets you grow your Arduino prototypes into production ready products rather than throwing everything away and starting over.
