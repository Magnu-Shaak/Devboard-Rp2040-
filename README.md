# Devboard-Rp2040-

This is a devboard following the Kai Pereira tutorial. However, there are 2 major differences:
- 1: Mine breaks out all pins while following the Pi-Pico layout*(mostly). He followed the Pi-Pico layout too*, but I also added 2 1x2 connectors which I used to break out the final GPIO pin, and provide easy access to 3v3 and VBUS from the bottom of the board. This greatly benefits the left side, as it now has an ADC pin and power access from the bottom, instead of needing to run traces to the other side of the board. And despite moving the 3 pin connector (used for debugging) up, some Pi Pico boards have it in the center anyways, so this doesn't make in incompatible as a drop in replacement either(unless its battery operated).
- 2: I also have opted to use the higher power rated LDO for my 3v3 line, whereas he swapped to a lower power rated one for space. Just better overall.

I made these changes for convienience. Having a better LDO is generally just better and the ADC and power pin breakouts are really convienient and help reduce congestion below the chip.

*Replaced some unused i/o pins (VSYS, 3v3_EN, ADC_REF) to breakout some 3 more GPIO pins


<img width="737" height="367" alt="image" src="https://github.com/user-attachments/assets/420c892c-90fb-41c2-9dd8-98ab9919b226" />
