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
9. Next You will be asked to create new user, to avoid problems later, name it **pi** and continue to set up passwod, locale etc. When done You should see something like this:
<img width="523" alt="image" src="https://user-images.githubusercontent.com/77267254/175825174-d7ea8d02-f496-4bb2-9a00-8abb6639a33a.png">

10. Next step is setting up WiFi connection. First way is to type `sudo armbian-config` 
<img width="676" alt="image" src="https://user-images.githubusercontent.com/77267254/175825283-c5ec8c1a-cc92-47d3-87d4-a962a062d604.png">

Navigate using arrows, go to "network" > "Wifi", find Your network and connect. Check the connection via routers management interface, disconnect lan cable and try to connect using putty and wireless module ip.

If it not works, things are getting more serious. Recconect lan cable, connect via putty and log in as pi. 
You will have to make changes in two files:
type `sudo nano /etc/network/interfaces`
and replace whats inside with:

```
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```
hit `Ctrl+O` to save and `Ctrl+X` to exit

next type:
`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

put inside:
```
network={
   ssid="YOUR_NETWORK_NAME"
   psk="YOUR_NETWORK_PASS"
}
```
do not remove quote marks.
hit `Ctrl+O` to save and `Ctrl+X` to exit.

Last step is to remove native network-manager, type:
`sudo apt remove network-manager`

after that, reboot by typing `reboot` in console.
When rebooted, You should be connected to Your Wifi.

# Klipper/Moonraker/Mainsail
1. Log in as **pi**
2. Update and install required packages by typing:
`sudo apt update` and next `sudo apt upgrade`
Install git, dfu-util and unzip by typing:
`sudo apt install git dfu-util unzip`
3. You need to install some packages required for klipper and since Armbian is not Debian based, there are some differences in packages names. Just copy and paste to Your concole:

`sudo apt install virtualenv python-dev libffi-dev build-essential libncurses-dev libusb-1.0-0-dev avrdude gcc-avr binutils-avr avr-libc stm32flash dfu-util libnewlib-arm-none-eabi gcc-arm-none-eabi binutils-arm-none-eabi libusb-1.0-0`

4. When finished You are ready to install the rest. There are two ways:

**Easy way**
Install Kiauh https://github.com/th33xitus/kiauh and proceed.


**Still easy but harder way**
Go to Mainsail website and proceed with manual setup
https://docs.mainsail.xyz/setup/manual-setup/klipper

5. When done, You have to make changes in moonraker config. Type in console:

`nano klipper_config/moonraker.conf'

navigate to "authorization" and "trusted clients" section and edit or add ip range for Your network. For example, if ips in Your network starts with 192.168.1.xxx, edit or add 192.168.1.0/24

<img width="127" alt="image" src="https://user-images.githubusercontent.com/77267254/175826982-9e787b47-e978-4c78-88db-3f18949362b4.png">

it will allow You to acces the web interface via http.

# END

