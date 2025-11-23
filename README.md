# WeAct Studio ePaper Screens
ESPHome custom component for WeAct Studio ePaper screens.

From RaceNJason (https://github.com/RaceNJason/WeAct-Studio_ePaper)

This covers:

ESPHome pull request #6209 by MrMDavidson for the WeAct Studio 4.2" BW screen.

ESPHome pull request #6226 by jbergler for the WeAct Studio 2.9" BWR screen.

And...my implementation of the WeAct Studio 4.2" BWR screen which is technically a modification of the code jbergler provided. I chose to put all 3 implementations together (as well as the latest ESPHome waveshare_epaper component code at the time of this posting) in order to, of course, support all currently supported screens as well as the WeAct Studio screens I've used and require a custom component to work.

Hopefully at some point ESPHome will introduce a WeActStudio_ePaper component or add these 3 screens to the current waveshare_epaper implemenation. Until then...anyone with these screens is free to use this code (giving credit to to the above authors for their original work of course).

Directions for Use:

1. Copy the custom_components folder into your ESPHome folder (or technically any folder accessible to your ESPHome compiler as long as you properly note the path in your yaml file)
2. Open up the esphome_example.yaml file included here to see what entries are necessary to pull in the custom models/code

That's it...

I have further modified the code to seperate out the names of the WeAct Studio 2.9 inch displays and to merge in changes so that the 3 colour 2.9 inch display can be used in black and white model. Black and white mode enables partial updates and hence fast refreshes without the flickering that is seen in the 3 colour mode.

Here's what I (666djb) did:

* I specified two new "models" in display.py:
  * wa2.90in operating only in black and white mode - this uses mods from PR#6217. It adds a new class WeActEPaper2P9In to the end of waveshare_epaper.h, and creates a new file weact_2p9in.cpp based on code from that PR.
  * wa2.90in3c operating in three colour mode - this uses mods from RaceNJason's work (https://github.com/RaceNJason/WeAct-Studio_ePaper)

How to use this code as a custom components remains as above, but you'll see in the Esphome IDE (Home Assistant ESPHome Builder) that you can select the following in the display section of of your YAML:
* model: wa2.90in
* model: wa2.90in3c