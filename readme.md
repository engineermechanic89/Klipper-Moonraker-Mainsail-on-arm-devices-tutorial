Basic tutorial on how to install Klipper/Moonraker/Mainsaill on non RPi arm based devices like Orangepi, Nanopi etc.
Example is based on Orangepi Zero LTS (the one with shitty WiFi module) and Armbian as it can be installed on most of Bananapi, Orangepi,Nanopi, Rock etc.

**What do You need:**
- Some kind of board with Pi in its name
- micro sdcard and card reader
- Balena Etcher https://www.balena.io/etcher/
- putty https://www.putty.org/
- Acces to Your router
- lan cable, usb cable
- patience

# Armbian
1. Go to Armbians website and download image correct to Your board https://www.armbian.com/download/?device_support=Supported
Stable branch recommended.
2. Put Your card into reader, open Balena Etcher, select image file, sdcard and flash it.
3. Put the card into Your PI, connect it to router with lan cable, power up the board, wait a while.
4. Acces Your routers management interface via http, find clients table and find Your boards IP. In my case it's called orangepi.
5. Open putty, type in boards IP and connect, the terminal widow should show up
<img width="479" alt="image" src="https://user-images.githubusercontent.com/77267254/175824871-e606b4c9-c244-44ba-9dac-08d8657fe41a.png">

6. Log in as user: **root** pass: **1234** 
7. You will be asked to change root passwod so type in Your password twice.
9. Next You will be asked to create ner user, to avoid problems later, name it **pi** and continue to set up passwod, locale etc. When done You should see something like this:
<img width="523" alt="image" src="https://user-images.githubusercontent.com/77267254/175825174-d7ea8d02-f496-4bb2-9a00-8abb6639a33a.png">

10. Next step is setting up WiFi connection. First way is to type `sudo armbian-config` 
<img width="676" alt="image" src="https://user-images.githubusercontent.com/77267254/175825283-c5ec8c1a-cc92-47d3-87d4-a962a062d604.png">

Navigate using arrows, go to "network" > "Wifi", find Your network and connect. Check the connection via routers management interface, disconnect lan cable and try to connect using putty and wireless card ip.

If it not works, things are getting more serious. Recconect lan cable, connect via putty and log in as pi. 
You will have to make changes in two files:
type `sudo nano /etc/network/interfaces`
and replace whats inside with:

`auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf`


