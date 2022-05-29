# pi

Everything related to setting up my Raspberry Pi for Home Automation, based on the book [Control Your Home with Raspberry Pi by Koen Vervloesem](https://koen.vervloesem.eu/books/control-your-home-with-raspberry-pi/)

# Setup

1. Download Imager to prepare the microSD card: https://www.raspberrypi.com/software
2. Flash Raspberry Pi OS (64-bit) Lite to the SD card
3. Don't insert SD card in Raspberry Pi yet, but reinsert it on your desktop.
4. Connect Raspberry Pi to ethernet (without SD card)
5. Check DHCP leases on Routers web interface and find the IP address of the Raspberry Pi. MAC address of every Raspberry Pi starts with `b8:27:eb` (the older models) or `dc:a6:32` (beginning from the Raspberry Pi 4).
6. Give Raspberry Pi a static IP address in the Routers web interface. The menu item should be called something similar to "DHCP static mappings".
7. Create an empty file called `ssh` in the `boot` folder of the microSD card.
8. Unmount the microSD card and insert it in the Raspberry Pi.
9. Connect the Raspberry Pi to power, it takes a while the first time, because it expands the file system the the microSD cards full capacity.
10. Check DHCP leases again to get Raspberry Pi's IP address.
11. Connect via SSH `ssh pi@IPADDRESS`. `pi` is the default user in Raspberry Pi OS.
12. After accepting SSH fingerprint, use the default password `raspberry`.
13. Change password with the `passwd` command.
14. Update package repositories and upgrade packages:

```
sudo apt update
sudo apt upgrade
```

15. Configure extra settings with `sudo raspi-config`.

- Change hostname if you have multiple Raspberry Pi's
- Change timezone
- In advanced options, change the memory split so the GPU gets the minimal value of 16 MB, since we don't need a graphical interface.

16. If asked to reboot, press Yes.
17. Install Pip ` sudo apt install python3-pip`.

Continue on page 44 of the book.
