**THIS IS NOT A GUIDE, FOLLOW AT YOUR OWN RISK**

This is an orientative text made to help people recover their Nothing Phone 2a from a bootloop in a certain case. This is not for every case, this is specific.

What I did before:

1. Flashed a custom ROM.

2. When finished flashing, in fastboot did `fastboot reboot recovery`.

3. Entered bootloop, the bootloader was trying to boot into recovery, as the instruction got written in the *misc* partition.

How to know if your device can be unbricked via this method?

1. On the bottom-left corner, it says `RECOVERY MODE =>` when in the bootloop, then orange state, then it reboots.

2. You are certain that the bootloop is caused due to the *misc* partition.

## Steps ahead

*My system is CachyOS Linux, if you are on Windows, follow this guide to unbrick your Nothing device: https://xdaforums.com/t/nothing-2a-unbrick-tool-official-tool.4687839/*

1. Make sure to have installed `mtkclient-git` and `android-tools`.

2. Make sure to have the `DA_MT6886.bin`, `auth_sv6.auth` and the `preloader_raw.img` files in your computer.

   *Optionally, kill ModemManager with `sudo systemctl stop ModemManager`. AFAIK this process hooks the device in BROM as a modem, and the phone might kill the connection.*

3. After that, you might proceed deleting the misc partition:

Run `sudo mtk e misc --preloader files/preloader_raw.img --auth files/auth_sv6.auth --loader files/DA_MT6886.bin` (or whatever your filenames are, also, run the command from inside the repo folder, and adjust the paths if you moved the files out of files/) and let it run in the background, it will be waiting for the device to connect in BROM mode.

*Make sure your device is not connected.*

How to enter BROM mode?

1. In the bootloop, press and hold all three buttons *Volume +, Volume - and Power*. When the device turns off, **release only Power** while still holding *Volume + and Volume -*, and immediately connect the USB cable to your device *(it must be connected to your PC, lol)*. Keep holding both volume buttons until it detects the phone. If the connection is successful, something like this should show in the terminal:

```
DAXML - Uploading xflash stage 1 from DA_MT6886.bin
DAXML - Successfully uploaded stage 1, jumping ..
Preloader - Jumping to 0x2000000
Preloader - Jumping to 0x2000000: ok.
DAXML - DA Stage 1 successfully loaded.
Exploitation - V6 Device is patched against carbonara :(
DAXML - Uploading stage 2...
Progress: |██████████| 100.0% Upload: (0x59830/0x59830), 567.66 KB/s  
Exploitation - Device vulnerable to heapbait ... trying to exploit...
DAXML - Uploading allinone signature...
DAXML - Successfully uploaded allinone signature.
Exploitation - Heapbait Stage 2 loaded.
Exploitation - Heapbait Stage 3 loaded.
Done |██████████| 100.0% Upload: (0x8/0x8),0.00 MB/s
Done |██████████| 100.0% Upload: (0x8/0x8),0.00 MB/s
Done |██████████| 100.0% Upload: (0x8/0x8),0.00 MB/s
Done |██████████| 100.0% Upload: (0x8/0x8),0.00 MB/s
Exploitation - Heapbait succeeded.
DAXML - Successfully booted to stage 2
DAXML - DA XML SLA is disabled
CUSTOM command detected.
DAXML - DA XML Extensions successfully loaded.
Main - Handling da commands ...
DAXML - Formatting addr 0x308000 with length 0x80000, please standby....
Done |██████████| 100.0% Erasing: (0x80000/0x80000),0.00 MB/s

```

If so, congrats! You may proceed to go into fastboot by unplugging the cable and pressing *Volume -* plus *Power*.
