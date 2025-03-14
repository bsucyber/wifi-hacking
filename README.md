# wifi-hacking
Wifi hacking activity with ESP32-WROOM-ESP boards.

## Assumptions
This activity assumes you are already acquainted with both the hacking code of ethics and basic linux/windows administration (installing drivers and programs, manipulating text files). All instructions are for Windows 11 (and assumed to work on Windows 10). If you have a Mac, use bootcamp or borrow a windows laptop from someone for this session.

![ethics](https://www.hackthebox.com/storage/blog/NtpLUlwRjbuvPc5vzMR3UQ9bFHp2GKBz.jpg)

## Objectives
Learning how to break a WPA2 wifi hash using `ESP32-WROOM-32~` platform boards and Hashcat.

## Prerequisites
Install Putty with an admin PowerShell window:
```
winget install PuTTY.PuTTY
```
(as a failsafe, you can use the [web serial](https://serial.huhn.me/) interface to run commands. this is less reliable)  
Install Drivers from here:  
https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads  
*(Don't forget to restart your computer after installing the drivers)*

## Firmware Install
- Go to the [ESPWebTool Web Updater](https://esp.huhn.me/)
- With your device plugged in, click connect (only works on Chromium/Chrome)
- Use the following table to select the appropriate files and place them at the corresponding address
- Enter the address shown as the blue text in the appropriate space and add the file linked to that blue text

|ESP32 Marauder v4|WiFi Dev Board Pro/LDDB/NodeMCU-32S/ESP32 Wemos D1 Mini/BFFB|
|--|--|
|Bootloader|[`0x1000`](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/MarauderV4/esp32_marauder.ino.bootloader.bin)|
|Partitions|[`0x8000`](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/MarauderV4/esp32_marauder.ino.partitions.bin)|
|Boot App|[`0xE000`](https://github.com/justcallmekoko/ESP32Marauder/raw/master/FlashFiles/FlipperZeroMultiBoardS3/boot_app0.bin)|
|Firmware|[`0x10000`](https://github.com/justcallmekoko/ESP32Marauder/releases/latest)|

- Use the following table to select the proper Marauder binary for your hardware. Please refer to version marking on your Marauder hardware is present.

|Hardware|Binary Version|
|--|--|
|v4 (OG)|`_old_hardware.bin`|

- Click program and wait. Once it says "To run the new firmware please reset your device", hit the reset button on the board (there is no dip switch).

## Configuration
Putty has bad default settings for what we are doing. Let's change that.

On the putty initial screen, make sure the "Session" categoty is selected. 

As pictured below, make sure to select COM3 port and change the speed to `115200`.  
![Alt Text](https://github.com/bsucyber/wifi-hacking/blob/main/putty-settings-2.png)

As pictured below, enable "Forced" for both local echo and local line editing. This will allow the terminal interface be usable and safe to operate on campus without misfires (as long as you carefully consider each command before sending).  
![Alt Text](https://github.com/bsucyber/wifi-hacking/blob/main/putty-settings.png)

## Next Steps
Once you've connected you are good to go! 

There are a ton of different features that you can check out.

Here is the offical wiki where you can find all the commands:
[ESP32 Marauder Wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki)

## Note about Wifi scans
Because we don't have sd cards, captured packets from wifi scans must be output to the terminal.  
to do this use the -serial option on the wifi scan you want to perform.  
For example:
```
sniffap
```  
```
select -a "AP"
```  
Give the SSID of the access point to target  
```
sniffpmkid -serial // Sniffs and displays captured pmkid/eapol frames sent during WiFi authentication sessions
```

It may be a good idea to enable logging in Putty so you can access the pcap data afterwards. You'll have to parse it back into a pcap file if you want to analyze it.  
