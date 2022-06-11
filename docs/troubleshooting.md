# Troubleshooting

## My circuit board is completely broken! Nothing is working!

Install `esptool` and use it to erase the flash of the ESP8266 microcontroller. Adjust the port as needed.

```bash
$ esptool --port /dev/ttyUSB0 erase_flash
```

Now, [flash](https://bastelgarage.github.io/led-circuit-board/wled) a new firmware.
