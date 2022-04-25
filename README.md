# kobo-libra2-uhid-module

My Logitech R500 can't work on Kobo Libra 2 so I try to check the problem. Finally, I rebuild the uhid.ko and it works with R500.

# How to use it

For version 4.31.19086

1. Install [NickelMenu](https://github.com/pgaskin/NickelMenu), I use it to launch the application.
2. Insert the device into your PC over USB.
3. Put kobo-libra2-uhid-module folder in ".adds" folder of device.
4. Copy "kobo-libra2-uhid-module/config" to ".adds/nm" or you could add the text below to your own config if you already use NickelMenu.
```
menu_item :main    :Bluetooth Patch (toggle) :cmd_output         :500:quiet :rmmod -w /mnt/onboard/.adds/kobo-libra2-uhid-module/uhid.ko
  chain_success:skip:3
  chain_failure                              :cmd_spawn          :quiet :exec /mnt/onboard/.adds/kobo-libra2-uhid-module/run.sh
  chain_success                              :dbg_toast          :insmod uhid.ko successfully
  chain_always:skip:-1
  chain_success                              :dbg_toast          :rmod -w uhid.ko successfully
```

# How to build kernel

I follow the concept of [this site](https://blukat.me/2017/12/cross-compile-arm-kernel-module/). However, I use WSL(Widows Subsystem for Linux) to build the uhid.ko. Here is my steps.

01. Use [NickelMenu](https://github.com/pgaskin/NickelMenu) to telnet into the device to get the /proc/config.gz
02. cp /proc/config.gz /mnt/onboard, then use PC to get the file.
03. Download kernel from [here](https://mirrors.edge.kernel.org/pub/linux/kernel/). I download linux-4.1.15.tar.gz.
04. Download cross compiler from [here](https://releases.linaro.org/components/toolchain/binaries/). I download gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz.
05. Suppose your work dir is /mnt/d. copy relevant files to this folder and cd to this folder.
06. sudo gunzip config.gz.
07. sudo tar zxvf linux-4.1.15.tar.gz.
08. sudo tar Jxvf gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf.tar.xz.
09. cd linux-4.1.15
10. cp ../config .config.
11. modify .config, add CONFIG_UHID=m to it.
12. sudo make ARCH=arm CROSS_COMPILE=/mnt/d/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf- oldconfig
13. sudo make ARCH=arm CROSS_COMPILE=/mnt/d/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf- drivers/hid/uhid.ko
14. After completing building, the uhid.ko is in drivers/hid.
