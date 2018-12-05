# touchscreen-focaltech-opi


#update 

since the first time I installed the touchscreen: 
- I compiled a new kernel 4.14.83-sunxi (which is different to 3)
- the touchscreen driver has changed as wel :  edt-ft5x06
- instead of the fex-file, now device tree overlays are used

one can generate :
# armbian-add-overlay i2c-edt-ft5x06.dts (here is described how the screen is connected to the GPIO)

and armbianEnv.txt (under boot) gets adapted automatically

(in armianEnv.txt one needs to enable the i2c0 overlay as well : example included)



I used the touchscreen from an old tablet (odys space : yeah really)

This was a capacitive focaltech touchscreen with an i2c interface (twowire)

The first time I used the touchscreen some areas where not sensitive...
After searching the net, it appeared to be a firmware problem

I had a look at the touchscreen driver, and I'm sure it automatically updates the firmware!
(so even if you have a working touchscreen, it could not be working after loading the module...)

So here is my procedure to install another firmware on a focaltech touchscreen :


wget http://www.freak-tab.de/finless/ft5x_firmware.zip


get the linux kernel sourcecode

copy drivers/input/touchscreen/ft5x/ft_app.h drivers/input/touchscreen/ft5x/ft_app.h-orig

hexdump -v -e '1/1 "0x%.2x,"' ft5406-sc3052-1024X768.bin > /usr/src/h3-wip/drivers/input/touchscreen/ft5x/ft_app.h

then compile and install the module ft5x_ts.ko


for the orange pi : 
adapt fex file (script.bin)

[ctp_para]
ctp_used                 = 1
ctp_name                 = "ft5x_ts"
ctp_twi_id               = 0
ctp_twi_addr             = 0x39
ctp_screen_max_x         = 800
ctp_screen_max_y         = 480
ctp_revert_x_flag        = 0
ctp_revert_y_flag        = 0
ctp_exchange_x_y_flag    = 0
ctp_int_port             = portA20<6><default><default><default>
ctp_wakeup               = portA10<1><default><default><1>
ctp_reset        = portA10<1><default><default><1>
ctp_io_port              = portA20<6><default><default><default>
ctp_power_ldo =
ctp_power_ldo_vol = 3000
ctp_power_io =



(1)
Using the touchscreen under armbian  - xfce was not straightforward :

I had to adapt ft5x_ts.h (comment //#define CONFIG_FT5X0X_MULTITOUCH ) and recompile the module

only then fs5x_ts showed up after executing : xinput --list (and the mousepointer could be controlled by the touchscreen)


(2)
/usr/share/X11/xorg.conf.d contains a file : 10-evdev.conf
I modified it quite a bit, so I'm not sure if this was not already there :

Section "InputClass"
        Identifier "evdev touchscreen catchall"
	        MatchIsTouchscreen "on"
		        MatchDevicePath "/dev/input/event*"
			        Driver "evdev"
				EndSection



