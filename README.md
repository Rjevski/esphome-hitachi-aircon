# ESPHome for Hitachi HVAC units

This repo contains an ESPHome configuration for some older Hitachi HVAC units. It interfaces with the aircon by emulating the IR remote.

TODO: document this better and add links to service manuals.

# Installation

You'll need the following off the board:

* +5V power feed
* data line from the IR receiver

Optionally:
* line from the "HA" pins - this is a primitive control bus that allows the aircon to be powered on/off (powers on in "auto" move) and outputs a boolean representing the power status of the aircon
* lines from the serial bus towards the compressor (for future reverse engineering)
* desolder the on-board buzzer so the aircon doesn't beep upon every change

Hitachi provides schematics in their service manuals.

# Usage

You should be able to use the aircon as normal; any operations from the official remote should be picked up by ESPHome and reflected on the Climate entity. You can also of course control it from Home Assistant.

## Ambient temperature reporting

The device provides an "ambient temperature reference" Number entity, whose value will be reported in the Climate entity's ambient temperature.

You can use a Home Assistant automation (see `automation.yaml`) to set this.

This is purely for cosmetic effect (so the ambient temp reported by the climate entity looks good) and has no actual effect on the aircon device.

You could also add a real temperature sensor if you wish?


## Compressor serial bus snooping

You'll notice that this config defines UARTs which snoop on both directions of the serial bus going to the compressor. The protocol is documented in the service manual so it can be decoded and read-only entities be created from the decoded values.

# Known issues

* Setting the "swing" mode is broken on some models; need to reverse-engineer the IR codes and PR to ESPHome.
