# PI - HOLE
## Using Raspberry Pi As DNS Server

---

> *Note: This is **NOT** a guide, but this shows how I install and implement Pi-Hole on my network as a learning project, I'm pretty sure there's something that I miss or write incorrectly.*

---

## Table Of Content
- [What Is Pi-Hole?](#what-is-pi-hole-)
- [Equipment](#equipment)
- [Raspberry Pi OS Installation](#raspberry-pi-os-installation)
- [Pi-Hole Installation](#pi-hole-installation)
- [Pi-Hole Implementation](#pi-hole-implementation)
- [Pi-Hole Addition](#pi-hole-addition)
- [Conclusion](#conclusion)

---

## What Is Pi-Hole
Pi-Hole is a DNS sinkhole or I would call it as something like a DNS Server that can protect devices that is connected to the Pi-Hole DNS server. see more in depth information of how pi-hole work [Pi-Hole Documentation](https://docs.pi-hole.net/)

### Reason To Do This Project
The reason that I try to do this project is to understand how DNS work, so i can make my own DNS server, and hopefully protect my network.

---

## Equipment
Equipment that I use:
>1. Raspberry Pi 3B+ (*Will run the Pi-Hole*)
>2. Laptop (*To do SSH to Raspberry Pi*)
>3. Router (*TP-Link | TL-WR940N*)
>4. [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/) (*To provide SSH connection to RaspberryPi*)
>5. Memory Card (MicroSD)

---

## Raspberry Pi OS Installation
Download the raspberry pi imager,  i got mine from [Raspberry Pi Imager](https://www.raspberrypi.com/software/) or you can install it from the terminal using this command
```
sudo apt install rpi-imager
```
After the installation of imager is finished, run it, and the imager should look like this:

![Raspberry Pi Imager](/Images/Capture1.PNG)

My microSD is brand new, so I need to format it to be FAT32, but thankfully, the imager already got that function, **CHOOSE OS > ERASE > CHOOSE STORAGE > WRITE**

![Raspberry Pi Format FAT32](/Projects/Pi-Hole/Images/Capture2.PNG)

(In this Picture im using USB as an example)

With the Formatted microSD, **Choose Storage > MicroSD**, and pick the operating system, I'll be using the **Raspberry Pi OS (32 Bit)**

![Raspberry Pi OS](/Images/Capture3.PNG)

After Choosing the OS, a **Gear** logo should appear, with this gear logo, you can enable some setting that could help set up Headless Raspberry Pi

![Settings](/Images/Capture4.PNG)

>1. Enable the "Set Hostname" if the hostname wanted to be changed
>2. Enable the "Enable SSH", this is needed for headless connection with the Raspberry Pi
>3. Enable the "Set Username and Password", this will be used when we connect to Raspberry through SSH
>4. Enable the "Configure Wireless LAN", this is needed so that the Raspberry can connect to the Wi-Fi, **SSID** is the name of Wi-Fi. and the "Password", is the Password for the Wi-Fi.
>5. **SAVE**

After all that is done, press **WRITE** and the imager should start installing to microSD. With the Installed OS on the microSD, insert it to the Raspberry Pi and power it on.

---

## Pi-Hole Installation
*Find a detailed installation of Pi-Hole* [here](https://github.com/pi-hole/pi-hole)

With the finished OS Installation, Turn on the Raspberry Pi and go to the router admin view or use an application called [Angry IP Scanner](https://angryip.org/) to scan the IP of the Raspberry to do headless configuration. headless means that the raspberry is not directly connected using keyboard or monitor.

In this case im using a TP-Link Router, and to access the router admin setting, usually theres a default IP for the router behind the router, or you can connect it via ethernet LAN and do this command:
```
ifconfig
```
or
```
ipconfig
```
this will shows the default gateway, in my case, the default gateway is **192.168.0.1**

![Default Gateway](/Images/Capture5.PNG)

After knowing the default gateway, go to Chrome or any other search engine, and type in the default gateway, after that the admin setting should pop up.

![Router Admin Settings](/Images/Capture6.PNG)

In my case, the TP-Link admin looks like the picture, if this is the first time of accessing this setting, the password should be the default that can be found from the back of the router or searched online. but usually the password is `admin` or even nothing.

If the password is correctly entered, the router can now be worked on. with TP-Link TL-WR940N, the UI looks like this:

![TP-Link UI](/Images/Capture7.PNG)

Click on the `DHCP` and more lists of setting would pop up, the IP and MAC of the devices that connected to the router can be seen here. 

![DHCP Settings](/Images/Capture8.PNG)

If the previous "Angryscanner" can't find the Raspberry Pi IP, The IP of the Raspberry Pi should be here. Go to the `DHCP Client List` and search a Client name with something like "raspberry", Copy the IP and MAC of the Raspberry PI and go to the `Address Reservation` and paste it the IP and MAC of the Raspberry PI, this is done to make sure that the IP of the Raspberry Doesn't change if the Raspberry PI is turned off. After all of that is finished, it should look like this:

![Address Reseration](/Images/Capture9.PNG)

Ok. Now access the Raspberry PI with Putty by entering the IP address of the reserved IP:

![Putty](/Images/Capture10.PNG)

After the Hostname or IP address of the Pi, Click "Open" and a window will pop up, asking `login as` and followed by the password that has been setup at the imager, and it should look like this:

![Putty Login](/Images/Capture11.PNG)

the line of the CLI will be green indicating that we've been connected to the Pi, after that we can install the Pi-Hole by using this command:
```
curl -sSL https://install.pi-hole.net | bash
```
or
```
wget -O basic-install.sh https://install.pi-hole.net
```
after the download finished do this:
```
sudo bash basic-install.sh
```

and the PI should start downloading and installing the Pi-Hole.
when the Pi-Hole is finish, it will show a configuration setting for the Pi-Hole, I just press Yes to all of it and use the `Wlan0` as the connection.

---

## Pi-Hole Implementation
With the installation of Pi-Hole finished, we can access the web interface by searching this address in the search engine:
```
http://ipadress/admin

example:

http://192.168.0.100/admin
```
the interface will look like this, as of Pi-Hole version **5.11.3** and web interface version **5.13**

![WEB Interface](/Images/Capture12.PNG)

to access the information, Click on the **Login** using the provided password when installing the Pi-Hole or using custom password by doing this command:
```
pihole -a -p
```
use this command to know more about Pi-Hole
```
man pihole
```

Well basicly the DNS server using Pi-Hole is finish!!, but to make sure if the devices on the network is using the Pi-Hole as the DNS server or no, we can use this command to check:
```
nslookup
```

with `nslookup` and providing the domain name like `google.com`, if the IP address shown the same as the IP address of the Pi-Hole, then the devices on the network is using the Pi-Hole as the DNS Server.

---

## Pi-Hole Addition
To make the Blocking capabiliy of the Pi-Hole better, we can add more "**adlist**" by using site [Pi-Hole Adlist](https://firebog.net/), I recommend using the **green** link to avoid the wrong blocked domain.

---

## Conclusion
In conclusion, Pi-Hole as a DNS Server are really good learning project, from how to set up the DHCP IP address reservation, accessing router admin configuration, configurating the installation to SSH communication.

![Finished Project](/Images/Capture13.PNG)

