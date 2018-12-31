# touchscreen-focaltech-opi
I used the capacitive touchscreen from an old tablet (odys space )



since the first time I installed the touchscreen: 
- I compiled a new kernel 4.14.84-sunxi (specify drivers/input/touchscreen) 
- the touchscreen driver has changed as wel :  edt-ft5x06
- instead of the fex-file, now device tree overlays are used

one can generate :
 armbian-add-overlay i2c-edt-ft5x06.dts (here is described how the screen is connected to the GPIO)


and armbianEnv.txt (under boot) gets adapted automatically



(in armianEnv.txt one needs to enable the i2c0 overlay as well : example included)




adapt 40-libinput.conf
(see example)


#howto use armbian to compile kernel modules

it only works on ubuntu bionic 18.04 x64
(I installed it on google cloud)

git clone --depth 1 https://github.com/armbian/build
cd build
./compile.sh

this process generates a kernel, with updates, modules etc ....

it packs it into deb packages in the output/debs directory
you can install this on your armbian-device

------------------------
in order to avoid a recompilation of everything :

under the build/cache/sources/linux-mainline-linux-4xxxy
is the kernel source

make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules SUBDIRS=drivers/input/touchscreen
(I only want the focaltech touchscreen driver)


eventually I copied the driver from the linus torvalts directory (updated jan 2018)






----this was applicable on the previous kernel module (it flashed new firmware to touchscreen)

I installed firmware on the touchscreen : (using version 3 kernel module) 

wget http://www.freak-tab.de/finless/ft5x_firmware.zip


get the linux kernel sourcecode

copy drivers/input/touchscreen/ft5x/ft_app.h drivers/input/touchscreen/ft5x/ft_app.h-orig

hexdump -v -e '1/1 "0x%.2x,"' ft5406-sc3052-1024X768.bin > /usr/src/h3-wip/drivers/input/touchscreen/ft5x/ft_app.h

then compile and install the module ft5x_ts.ko





