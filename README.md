
# GRBL control station
An exploration of ways of interacting with hobbyist CNC machines, focusing on reducing beginner intimidation. 

### Some terms before we begin
* CNC - ([wiki](https://en.wikipedia.org/wiki/Numerical_control)) machines are computer controlled machines used to make things.
* [Grbl](https://github.com/gnea/grbl/wiki) is a lightweight CNC motion planning firmware that runs on a microcontroller.
* [Arduino](arduino.cc) is a super popular hobbyist electronics platform that is programmable.
* [Fabricatable machines](https://github.com/fellesverkstedet/fabricatable-machines/wiki) an open source project developing user friendly CNC milling machines.
* DRO ([wiki](https://en.wikipedia.org/wiki/Digital_read_out)) is short for Digital Read Out, basically that you have a display on the machine showing the state and the current position coordinates. 
* SD, a memory card used to hold gcode files.
* Gcode, machine coordinates that the CNC machine goes through to perform a job.

## Background and motivation
I want to lower the intimidation factor of using a hobbyist CNC milling machine.  
  
Compare the user experience when faced with learning to use a typical grbl CNC milling machine with learning to use a RAMPS 3D printer.

### A generic RAMPS 3D printer with a Smart controller
* Generates gcode in cura (*this can be expanded upon*)
* Saves the gcode on an SD card
* Inserts the SD card into the printer
* Interacts with the ONLY button
* Chooses the file and presses to run it
* The printer automatically homes and 

It's common for users to use SD cards to load gcode into 3D printers and users are used to simple, few-button interfaces to start and stop jobs.

My starting point for doing this research is that I previously did a [Smart Controller SD+LCD conversion of old and unused 3D printers.](https://github.com/Jaknil/TAMS12013#type-a-machines-series-1-2013-wood-edition-with-full-graphic-smart-controller) The conversion seems to have lovered the threshold to start using the printers. Previously the users would have to download UGS or similar software and physically connect their laptop to the printer, this wasn't happening and the printers stood unused.

I have also made a crude jogging controller for grbl during Fab Academy 

Notable ways of controlling grbl:
* [The official list](https://github.com/gnea/grbl/wiki/Using-Grbl#how-to-stream-g-code-programs-to-grbl)
* [Universal Gcode sender - platform](https://winder.github.io/ugs_website/) A Java, cross platform Gcode streaming and jogging program used to connect and **control grbl from a computer over a USB to serial connection**. Popular among fabricatable machines users. (*Note that UGS also has a built in local webserver that makes you smartphone into a wireless jogging-pendant.*)
* [Octooprint](https://octoprint.org/) uses a raspberry Pi to create an local wifi webserver that then is used to **control grbl from your browser**. Made for 3D printing.
* [LCD ob grbl](https://wiki.shapeoko.com/index.php/LCD_on_GRBL#Full_version_GRBL_1.1) using a Arduino Leonardo and an LCD to make a simple DRO.

Forum logs
* [LCD + SD](https://github.com/grbl/grbl/issues/717) Headless by modding smart controller
* [LCD+SD+knob](https://github.com/gnea/grbl-Mega/issues/77)
* [XLCD serial proxy](https://wiki.shapeoko.com/index.php/XLCD)

LCD mega
* http://www.bartvenneker.nl/Arduino/index.php?art=0025

There are many more variations but these should be representative of the mayor ways one can control grbl.

To be investigated further:
* Features needed, minimal and nice to have
* buy and adapt the $5 ramps smart controller directly? https://reprap.org/wiki/RepRapDiscount_Full_Graphic_Smart_Controller
* Does a limited interface force the user to best practices and reduces the stress of choise?

## Adapting the Smartcontroller to ESP8266

* [Pinout from the smartcontroller](https://docs.google.com/spreadsheets/d/19lpjkqaOqPkHmnNl6rEzy1P8AtqGDscS7eaFzm6XpZ4/edit?usp=sharing
)

# Ideas

## ESP8266 controller -> Grbl motion planner on an arduino
* ESP for controls and file handling, arduino for motion control
* Physical serial connection Tx Rx, not USB
* Streams gcode to a standard separate grbl microcontroller
* LCD display
* Encoder knob
* SD card memory (integrated is just 4 MB)
* Wifi webserver on local network allows gcode files to be sent to SD card wirelessly
* Simple menu system to run files, allows for homeing sequences, firmware config, jogging using small relative motion gcode files.
* Wifi credentials can be stored on SD card


## ESP32, all in one solution
* One core does motion control  (not how below has implemneted it)
* One core does wifi, handle displays and files etc (seems hard to do and mod, interupts etc)
* http://www.buildlog.net/blog/2018/07/grbl-cnc-firmware-on-esp32/
* https://github.com/bdring/Grbl_Esp32
Grbl maker also recommend dedicated microcontroller to motion control
