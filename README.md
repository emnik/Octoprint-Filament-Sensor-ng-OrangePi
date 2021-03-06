# OctoPrint-FilamentSensor-ng-OrangePi

I made this because [Octoprint-Filament-Sensor-ng](https://github.com/Red-M/Octoprint-Filament-Sensor-ng) doesn't have support for OrangePi.

[OctoPrint](http://octoprint.org/) plugin that integrates with a filament sensor hooked up to a Orange Pi GPIO pin and allows the filament spool to be changed during a print if the filament runs out.

Initial work based on the [Octoprint-Filament-Sensor-ng](https://github.com/Red-M/Octoprint-Filament-Sensor-ng) plugin by Red-M.
Initial work based on the [Octoprint-Filament-Reloaded](https://github.com/kontakt/Octoprint-Filament-Reloaded) plugin by kontakt.
Initial work based on the [Octoprint-Filament](https://github.com/MoonshineSG/Octoprint-Filament) plugin by MoonshineSG.

## Required sensor

Using this plugin requires a filament sensor.

## Features

* Configurable GPIO pin.
* Debounce noisy sensors.
* Support norbally open and normally closed sensors.
* Execution of custom GCODE when out of filament detected.
* Optionally pause print when out of filament.

## Installation

* Manually using this URL: https://github.com/emnik/Octoprint-Filament-Sensor-ng-OrangePi/archive/master.zip

## Configuration

After installation, configure the plugin via OctoPrint Settings interface.

The pin being used needs to be entered by name (e.g. PA01, PC07)

## OrangePI OS Configuration

Since we are accessing the GPIO as a non root user we need to configure the OS to allow this. Here's the copy of the library documentation on how to do it:

 If you want to be able to use the library as a non root user, you will need to setup a `UDEV` rule to grant you permissions first. 
 This can be accomplished as follows: 
 ```
 $ sudo usermod -aG gpio <current_user>
 $ sudo nano /etc/udev/rules.d/99-gpio.rules
 ```
 That should add your user to the GPIO group, create a new ``UDEV`` rule, and open it in the Nano text editor. 
 Enter the following into Nano
 ```
   SUBSYSTEM=="gpio", KERNEL=="gpiochip*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport ; chmod 220 /sys/class/gpio/export /sys/class/gpio/unexport'" 
   SUBSYSTEM=="gpio", KERNEL=="gpio*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value ; chmod 660 /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value'"
 ```   
 press ``ctrl-x``, ``Y``, and ``ENTER`` to save and close the file. 
 Finally, reboot and you should be ready to use ``OPi.GPIO`` as a non root user. 


## Issues with OPI GPIO library

https://github.com/rm-hull/OPi.GPIO/pull/28 solves the issues with PI GPIO library and the race condition that was preventing normal operations. It is available in release v0.3.4 and up so make sure you have latest OPI GPIO library. To upgrade to latest OPI GPIO library login to your octoprint server as user ``pi`` and execute:

```
 $ cd OctoPrint/ 
 $ venv/bin/pip install --upgrade OPi.GPIO
```
