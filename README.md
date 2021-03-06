
## pi-lcd-line-display-plugin
Plugin for the HD44780 LCD for Volumio & Moode

Package should install, i2c is confirmed to work on any Pi, GPIO only on Pi1 models (driver is recompiled, just needs testing).

Roadmap:

1. ~~Write the configuration~~
2. ~~Write the index.js~~
3. ~~Update the installscript to final~~
4. ~~Update dependencies for NodeJS (abandon use of LCDproc/mpdlcd to support overall functionality)~~
5. ~~Create final package~~
6. Support 4-line display properly

## LCD daemon and proc

I did not write nor contribute to lcdproc, so all credits go to them for enabling me to use it.
You can start/stop the service by calling for example:

$ service LCDd start

You can test your LCD by calling lcdproc to print CPU info, note that it is a capital C.

$ lcdproc C

For some reason a Pi2/Pi3 in combination with GPIO connections will not work:

Apr 15 19:03:55 volumio LCDd[1643]: check_board_rev: This board is not recognized as a Raspberry Pi!

Apr 15 19:03:55 volumio LCDd[1643]: Driver [hd44780] init failed, return code -1

Apr 15 19:03:55 volumio LCDd[1643]: Could not load driver hd44780

Apr 15 19:03:55 volumio LCDd[1643]: There is no output driver

Apr 15 19:03:55 volumio LCDd[1643]: Critical error while initializing, abort.

I recommend the use of an i2c piggy back.

## MPDLCD

Same as lcdproc, all credits go to the mpdlcd development team. You can start mpdlcd manually after installation to test its functionality.

$ mpdlcd

The configuration can be found in /etc/mpdlcd.conf

### Update 26-06-2018

Removed the node_modules from the package, this will increase installation time, but installation will succeed in pre- and post 2.411 versions of Volumio.

## Enable i2c
Open SSH connection to Pi

Enable i2c:
$ sudo raspi-config

Select 5 Interfacing Options
Select P5 I2C
Select "Yes" when asked "Would you like the ARM I2C interface to be enabled?"

## Install
Open SSH connection to Pi


$ git pull https://github.com/duongdoan/pi-hd44780-plugin.git
$ cd pi-hd44780-plugin

$ chmod u+x install.sh
# sudo ./install.sh

## Autostart
$ sudo nano /etc/rc.local

Add lines before Exit 0:

service LCDd start
mpdlcd ––no-syslog &

Restart pi:
$ sudo shutdown -r now
