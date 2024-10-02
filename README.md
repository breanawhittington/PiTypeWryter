# PiTypeWryter
distraction free writing tool - use raspberry pi &amp; a kindle (or anything that is able to display a browser)

Direct inspiration from the following:
PiWrite
TypeWryter
ZeroWriter

This project consists of less hands-on hardware work. You can use a Kindle (and possible any tablet that has a browser) and a keyboard to use a simple distraction free text editor. 

I will attempt to explain steps as detailed as possible. I am not incredibly talented with this stuff (hence why I am using other's codes and chatgbt to figure it all out.)


# Install Raspian OS to your micro sd card for your Raspberry PI
https://github.com/zerowriter/zerowriter1/blob/main/how-to-setup-your-pi has amazing directions on how to do this step by step - including some of the ssh information. It's great for those who need a little more guidance. The final program doesn't use the official clone of the git for zerowriter - however shout out to zerowriter for being one of the main reasons that typewryter exists, and then this. 

I just used the Raspberry Pi Imager. I did the debian bookworm lite 32bit with the desktop just in case. I am using a Raspberry PI Zero W for this set up. The first round is a test set up for my husband. I have no idea if it is enough or not, but it does have wifi & bluetooth - which I need.

I set up the name for my pi, user info, and wifi account to try to make my life easier.

# SSH into your Pi
I connected my pi to my computer via usb. https://github.com/zerowriter/zerowriter1/blob/main/how-to-setup-your-pi has amazing directions on how to do this step by step. If you haven't done this before - when the pi is powered on it can take a few minutes to boot up. I'm not sure how fast other non zero w pis are, but the zero w can take like 3+ minutes at times.

```
ssh pi@[hostname].local
```
OR 

use the pi ip address. I found mine by logging into my router, and looked at devices attached to my router.  
```
ssh pi@[pi IP address]
```

# Making pi a wireless access point
^ NOTE: If you are using a pi that doesn't have an ethernet port, or not planning to use ethernet, you likely will want to install the program before making the pi a wireless access point. I had issues because when I made my pi finally be an access point I couldn't ssh in it via wifi - which make sense but also is a pain. 

first - always want to do the upgrade stuff to your pi. 

```
sudo apt update
```
```
sudo sudo apt-get update
```

Get the access point and DNS services needed
```
sudo apt install hostapd dnsmasq
```

Start them
```
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
```

Edit `/etc/dhcpcd.conf`and add 
```
sudo nano /etc/dhcpcd.conf
```
add to file
```
interface wlan0
    static ip_address=192.168.11.1/24
    nohook wpa_supplicant
```
next step

```
sudo nano /etc/dnsmasq.conf
```

add to file - I just added it to the end of the file since everything was commented out.
```
interface=wlan0 # Listening interface
dhcp-range=192.168.11.2,192.168.11.20,255.255.255.0,24h
                # Pool of IP addresses served via DHCP
domain=write     # Local wireless DNS domain
address=/pi/192.168.11.1
                # Alias for this router
```
Config the next file:
```
sudo nano /etc/hostapd/hostapd.conf
```
add to fie
```
country_code=US
interface=wlan0
ssid=PiTypeWryter
hw_mode=g
channel=7
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=CHOOSE SOMETHING
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

After this set up - reboot the pi.
```
sudo shutdown -r now or sudo systemctl reboot
````
In theory your pi access point should be set up and ready to go. You are supoose to be able to see it in your networks (wifi networks).
If you have issues with the set up - I just check with ChatGPT cause I ain't got time for that. I had to use the correct usb cord to propery ssh into my pi without internet. 

