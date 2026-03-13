# Levoit Air Purifier Integration for ESPHome

## Supported Devices
* Levoit Core 200s (Confirmed)
* Core 300s (Confirmed)
* Core 400s (Confirmed, thank you @SeveranExp)

## Disassembly
For details on the teardown, please read [this blog post](https://vigue.me/posts/levoit-air-purifier-esphome-conversion).
* Place device upside down and remove base cover and filter.
  * On the Core 300s, 8 screws will be exposed, 4 black screws and 4 with washers attached.
  * On the Core 200s, there will only be 4 screws with washers attached.
* Remove all exposed screws. (*Be careful, these are made out of a soft metal*.)
* Using a pry tool, press into the tabs and gently separate the two pieces.
* Completely separate the base (holds the filter) and the top (holds the fan).
* Slide the entire fan out of the top.
* Unplug logic board from the fan. There's a clip, on the plug, that must be pressed in order for the plug to be removed.

## Flash
* Copy the provided sample configuration for your model to a new ESPHome configuration, while keeping the generated passwords.
* Compile and download the binary (Choose the modern format once compilation has completed).
  * If using esphome on the command line, run `esp compile levoit300s.yaml`. The modern format binary will be located in `.esphome/build/levoit300s/.pioenvs/levoit300s/firmware.factory.bin`.
* Solder wires to pins TXD0, RXD0, IO0, +3V3, and GND near the ESP32 on the logic board, and connect these to a USB UART converter. On some boards, if these are through holes, soldering may not be necessary.
  * The Core 200s has a spot for +5V and no IO0.
* Connect IO0 to ground while applying power to boot into the bootloader.
  * If no IO0 is available, use the GPIO0 pin directly on the ESP32 (right side, very bottom).

### Backup Existing Firmware
```bash
esptool read-flash 0 ALL levoit300s-stock_firmware-$(date +"%Y%m%d").bin
```

### Erase Flash
```bash
esptool erase-flash
```

### Install New Firmware
```bash
esptool write-flash 0x00 esphome-firmware.bin
```

Disconnect and reconnect the USB UART to make sure the ESP32 restarts. Once powered, check to make sure the ESP32 connects to your wireless network.

After confirming, reassemble, and enjoy :)

# Contributing
All contributions are welcome! Please open an issue or a PR.

## Contributors

* Aiden (@acvigue) - Original code & Core300s support
* Ryan Lang (@ryan-lang) - Core400s support
* Havarius (@Havarius) - Core200s support
* Haydon (@haydonryan) - Documentation & testing
