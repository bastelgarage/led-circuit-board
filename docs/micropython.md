# MicroPython

The first step is to download the MicroPython port for the ESP8266: https://micropython.org/download/?port=esp8266

The "[Deploying the firmware](https://docs.micropython.org/en/latest/esp8266/tutorial/intro.html#deploying-the-firmware)" section in the MicroPython documentation will provide you all the information you need.

Connect your LED circuit board with the serial adapter to your computer. Here we are using a linux-based system. Thus, the serial port is called `/dev/ttyUSB0`. If you are using Windows or MacOS you have to adjust this.

Usally it's simple to flash the firmware. Make sure that you have `esptool` available on your system. It's a good idea to erase the previously used firmware with `sudo esptool.py --port /dev/ttyUSB0 erase_flash` before flashing a new one.

```
$ sudo esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 esp8266-1m-20220117-v1.18.bin 
```

Reboot your LED circuit board and connect with your serial terminal tool of your choosing, e.g., `minicom`:

```bash
$ minicom -D /dev/ttyUSB0
```

This will give you access to the REPL/serial prompt of MicroPython.

```bash
Welcome to minicom 2.7.1

OPTIONS: I18n 
Compiled on Jan 20 2022, 00:00:00.
Port /dev/ttyUSB0, 14:42:48

Press CTRL-A Z for help on special keys                                  
                                                                         
#5 ets_task(4020f598, 29, 3fff8e80, 10)                                  
                                                                         
MPY: soft reboot
MicroPython v1.18 on 2022-01-17; ESP module (1M) with ESP8266
Type "help()" for more information.
>>> 
```

Another step you want to take perhaps is to set up [WebREPL](https://docs.micropython.org/en/latest/esp8266/tutorial/repl.html#webrepl-a-prompt-over-wifi).

## Work with the LED strip

MiciroPython supports WS2812 LEDs or also know as NeoPixels out of the box. For a first test, we are goining to set the first LED (position/index 0) to full red. Remember the LEDs are connected to `GPIO2`.

```bash
>>> import machine, neopixel
>>> np = neopixel.NeoPixel(machine.Pin(2), 7)
>>> np[0] = (255, 0, 0)
>>> np.write()
>>> 
```

Further details about using NeoPixels can be found in the "[Controlling NeoPixels](https://docs.micropython.org/en/latest/esp8266/tutorial/neopixel.html)" section in the MicroPython documentation.

## Using the buttons

The buttons (`GPIO0` and `GPIO5`) can be used in two ways. 

```bash
>>> pin = machine.Pin(0, machine.Pin.IN, machine.Pin.PULL_UP)
>>> pin.value()
1
>>> pin.value()
0
```

Or if you prefer working with interrupts.

```bash
>>> def callback(pin):
...     print("Button state changed", pin)
>>>
>>> from machine import Pin
>>> p5 = Pin(5, Pin.IN)
>>> p5.irq(trigger=Pin.IRQ_FALLING, handler=callback)
<IRQ>
>>>
pin change Pin(5)
```
