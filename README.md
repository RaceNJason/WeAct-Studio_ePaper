# WeAct Studio ePaper Screens
ESPHome custom component for WeAct Studio ePaper screens.

This covers:

ESPHome pull request #6209 by MrMDavidson for the WeAct Studio 4.2" BW screen.

ESPHome pull request #6226 by jbergler for the WeAct Studio 2.9" BWR screen.

And...my implementation of the WeAct Studio 4.2" BWR screen which is technically a modification of the code jbergler provided. I chose to put all 3 implementations together (as well as the latest ESPHome waveshare_epaper component code at the time of this posting) in order to, of course, support all currently supported screens as well as the WeAct Studio screens I've used and require a custom component to work.

Hopefully at some point ESPHome will introduce a WeActStudio_ePaper component or add these 3 screens to the current waveshare_epaper implemenation. Until then...anyone with these screens is free to use this code (giving credit to to the above authors for their original work of course).

Directions for Use:

1. Copy the custom_components folder into your ESPHome folder (or technically any folder accessible to your ESPHome compiler as long as you properly note the path in your yaml file)
2. Open up the esphome_example.yaml file included here to see what entries are necessary to pull in the custom models/code

That's it...
