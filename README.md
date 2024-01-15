# Noobs guide how to install/enable Octoprint on old Oneplus One

Octoprint for Android running on Oneplus One (bacon), for Creality Ender3

### Parts needed:

- mini-usb to USB cable - to connect to the Ender 3 printer
- Y-usb reduction with OTG support - USB Micro-B --> USB-A & USB Micro-B - to connect charger from phone (to micro-B) and printer (USB-A) and connect it to phone (micro-B)
- OnePlus One (bacon)

### Setup:

- 1. Connect printer viac min-usb cable to Oneplus using Y-cable, OTG should be recognised automatically & display should lit
- 2. If the printers display lit, put a piece of electrical tape on 5V Pin in USB-A, as this guide suggest (https://community.octoprint.org/t/put-tape-on-the-5v-pin-why-and-how/13574) - it prevets leaking current from phone to printer
- 3. Download latest octoprint4a, install it on the phone (be patient, can take up to 30 minutes)
- 4. Turn the printer on
- 4. In Octoprint4a, serial port should be recognised and "Printer is connected" shoudl be written as status

## OTG support & charge phone simultaneosly:

1. unlock bootloader & install TWRP recovery, copy all files from `./bin` to phone `./Download` folder
2. On TWRP Recovery, flash the ROM cm-13.0-20161205-jgcaap-bacon.zip
3. after that flash the kernel from `nikhil18 - https://androidfilehost.com/?fid=961840155545585586` - LIGHTNING+KERNEL+NEW+57.zip which includes a patch to enabled power over OTG
4. flash extras: gapps 5.1 (optional) and SuperSU to root

when reboot into the system, install one terminal app or enable tyerminal on developer options and write:

```sh
su root
echo Y > /sys/module/dwc3/parameters/aca_enable
```

plug the cable with the device (peinter) and charger connected, and it works properly, both charging and data transfer.

Sources:

- https://github.com/feelfreelinux/octo4a/wiki/Connecting-to-printer-via-OTG,-simultaneous-charging
- https://xdaforums.com/t/kernel-power-over-otg-host-mod.3201100/
- https://xdaforums.com/t/guide-unlock-root-flash-for-oneplus-one.2839471/

## Oneplus One simple flashing guide:

Prerequisites:

You'll need a working adb/fastboot environment on your computer to get through some of these guides.

1. Install adb (`brew install android-platform-tools`)
2. Check if installed properly: `adb version`
   If it returns a version number for Android Debug Bridge then you're good to go.
3. test adb by connecting your device to your computer while booted into Android (making sure that adb/usb debugging is enabled in **Settings>Developer Options**) with the screen unlocked and issuing this command: `adb devices`
   It should return your device serial number, if so, adb is working.
4. You can test fastboot by connecting your device to your PC while booted into fastboot mode (power + volume up) and issuing this command: `fastboot devices`
   It should return your device serial number, if so, fastboot is working.

## How To Unlock Your Bootloader

Power off your phone then boot into fastboot mode (power + volume up).
Connect your phone to your PC via usb cable.
Open a command prompt from within your fastboot folder (navigate to where you have fastboot.exe located on your PC, shift + right click anywhere within that folder, select open command prompt here).
Check your fastboot connection by issuing this command:

`fastboot devices`

It should return your device serial number, if not you need to make sure your drivers are installed correctly.​
Once you've confirmed your fastboot connection issue this command:

`fastboot oem unlock`

The device will now go through the automated unlocking process, just let it do its thing and it'll boot up into Android.​
Go back to fastboot mode and issue this command to ensure that it worked:

`fastboot oem device-info`

It should have a couple of lines there, both with the flag set to true.​
You can now reboot your phone using this command:

`fastboot reboot`
It's now safe to disconnect your usb cable.

Please note: **this will erase all user data from your device, it is best to do this before you really start using the device and installing apps or putting data on the internal storage.**

2. How To Install A Custom Recovery On Your Device

Prerequisites: unlocked bootloader.

You first need to go into Settings/Developer Options and uncheck the "Update recovery with system updates" option (to enable Developer Options go into Settings/About Phone and click 'build number' about seven or eight times).
Download the recovery of your choice (my preferred recovery is TWRP, and we'll be using that for this guide, grab it here).
Make sure you check the md5 to verify its integrity.
Place the file in your fastboot folder (this is where fastboot.exe is located on your PC).
Put the phone in fastboot mode and connect it to your PC via usb cable.
Open a command prompt from within your fastboot folder (shift + right click, select open command prompt here), and enter the following commands:

Code:
fastboot flash recovery openrecovery-twrp-2.8.1.0-bacon.img

(The recovery filename in the command will change depending on which recovery you're flashing)

Code:
fastboot format cache

Code:
fastboot reboot
Let the device reboot, it's now safe to disconnect your usb cable.

Now you can use the advanced power menu (or the power + volume down button combo) to enter your custom recovery.

Please note, if you have taken the stock Lollipop update please read the following:

For some reason CM12S doesn't respect the on/off toggle for the "update CM recovery" option in Developer Options, it just replaces whatever custom recovery you've flashed with the CM recovery regardless of whether that option is unelected.

Flash your custom recovery again, but after flashing don't do a normal reboot. After the flash has succeeded disconnect the usb cable, then power down the phone by holding the power button down, once it's powered off use the power + volume down button combo to boot directly into recovery. This first forced reboot into recovery somehow subverts what was keeping it from staying flashed and all subsequent normal reboots into recovery will boot into the custom recovery of your choice.

3. How To Make A Nandroid Backup With TWRP Recovery

Prerequisites: unlocked bootloader, TWRP recovery.

A nandroid backup is a very important thing to have before installing any custom software on your device. It's basically a backup of your stock system that you can fall back on if anything goes wrong or if you just want your stock ROM back. You can also use the backup tool to create a backup of your favourite ROM set up exactly the way you like it. The backup you create can be easily restored using the restore tool in TWRP recovery.

All you need to do is enter TWRP recovery, select the backup option from the TWRP home screen, check the system/data/boot boxes, and swipe to backup. The process will take a few minutes.

z7e6jQa.jpg

4. How To Root Your Stock Rom

Prerequisites: unlocked bootloader, TWRP recovery.

First you need to download SuperSU.
Enter TWRP recovery via the advanced power menu (or power + volume down).
Select the install option from the TWRP home screen.
Navigate to where you have SuperSU stored on your sd card and select it.
Swipe to install.
Once you've installed SuperSU you'll have an option to wipe cache/dalvik and an option to reboot system. Wipe the cache/dalvik, hit the back button, and hit the reboot system button. That's it.

z7e6jQa.jpg

5. How To Install A ROM with TWRP Recovery

Prerequisites: unlocked bootloader, TWRP recovery.

Installing a ROM is a pretty straight forward and easy process. Before you install anything you should make a nandroid backup (instructions above).

Download a ROM and appropriate Gapps package and place on your device.
Boot into your custom recovery.
Perform a full wipe.
Select the wipe option from the TWRP home screen.
Select advanced wipe.
Check the system, data, cache, and dalvik cache options.
Swipe to wipe.
Install the ROM.
Select the install option from the TWRP home screen.
Navigate to where you have the ROM zip stored on your sd card and select it.
Swipe to install.
Most ROMs will run an installer script at this point but some ROMs have what is called an Aroma Installer which allow you to choose some install options before the script runs.​
You will also need to install the appropriate gapps package directly after installing the ROM.
Once you've installed all necessary zips you'll have an option to wipe cache/dalvik and an option to reboot system. Wipe the cache/dalvik, hit the back button, and hit the reboot system button.

z7e6jQa.jpg

6. How To Install A Custom Kernel With TWRP Recovery

Prerequisites: unlocked bootloader, TWRP recovery.

A custom kernel can open up a new level of control over your device, such as overclocking/underclocking, undervolting, changing governors, changing I/O schedulers, adjusting colour calibrations, adjusting sound calibrations, and many other options.

Download a kernel that is compatible with your current ROM.
Check the md5 to verify its integrity.
Enter TWRP recovery.
Select the install option from the TWRP home screen.
Navigate to the kernel and select it.
Swipe to install.
You'll have an option to wipe cache/dalvik and an option to reboot system. Wipe the cache/dalvik, hit the back button, and hit the reboot system button.

Once your phone has booted up you can use a kernel tuning app to change governors, I/O scheduler, clock speed, and other options. Some of the popular kernel apps are Trickster Mod, No Frills, Kernel Tuner, and many more.

z7e6jQa.jpg

7. How To Flash The Stock Kernel (boot.img) With Fastboot

Prerequisites: unlocked bootloader.

If you're running a custom kernel on Cyanogen OS you'll need to flash the stock kernel back in order to take an OTA update.

Download the appropriate set of stock images from this thread. Extract the zip and grab the "boot.img" file.
Put it in your fastboot folder (where you have fastboot.exe located) on your PC.
Boot into fastboot mode (power + volume up) and connect your phone to your PC via usb cable.
Open a command prompt from within your fastboot folder (shift + right click, select open command prompt here).
Issue this fastboot command:

Code:
fastboot flash boot boot.img
It'll take a few seconds to flash the boot.img, once it's finished you can manually reboot your phone or use the following command to reboot it:

Code:
fastboot reboot
It's now safe to disconnect your usb cable.

Now you have the stock kernel back on your device.
