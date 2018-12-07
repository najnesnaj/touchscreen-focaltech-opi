# touchscreen-focaltech-opi
I used the capacitive touchscreen from an old tablet (odys space )


#update 

since the first time I installed the touchscreen: 
- I compiled a new kernel 4.14.83-sunxi (specify drivers/input/touchscreen) 
- the touchscreen driver has changed as wel :  edt-ft5x06
- instead of the fex-file, now device tree overlays are used

one can generate :
 armbian-add-overlay i2c-edt-ft5x06.dts (here is described how the screen is connected to the GPIO)

and armbianEnv.txt (under boot) gets adapted automatically

(in armianEnv.txt one needs to enable the i2c0 overlay as well : example included)


in /etc/modules add edt-ft5x06


apt-get install xserver-xorg-input-evdev
adapt 40-libinput.conf




this was a procedure for kernel 3:
procedure to install another firmware on a focaltech touchscreen :


wget http://www.freak-tab.de/finless/ft5x_firmware.zip


get the linux kernel sourcecode

copy drivers/input/touchscreen/ft5x/ft_app.h drivers/input/touchscreen/ft5x/ft_app.h-orig

hexdump -v -e '1/1 "0x%.2x,"' ft5406-sc3052-1024X768.bin > /usr/src/h3-wip/drivers/input/touchscreen/ft5x/ft_app.h

then compile and install the module ft5x_ts.ko





