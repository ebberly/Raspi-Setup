#Raspi-Setup

Raspberry Pi setup for the Site to Site course taught at the Columbia University Graduate School of Architecture, Planning and Preservation. We will be using the Raspberry Pi with a version of the Linux open source OS called Raspbian in order to connect remote physical spaces through the web.

##Formatting the SD Card with the Raspbian OS

_Note: Much of this step and the following are also documented [here](http://lifehacker.com/5976912/a-beginners-guide-to-diying-with-the-raspberry-pi), though I had trouble with the SD card formatter, and the config options were different as that tutorial might have been for an earlier release of the Raspbian OS._

The Raspberry Pi uses an SD card (4GB minimum, class 4 or 6) as its harddrive, the only source of persistent data storage. It is on the SD card that you will install the operating system (OS) first with your computer, or you can buy an SD card with the OS already loaded (I recommend formatting the SD card your self, both to have a better understanding of how it's working, and to have more control over the version).

To begin, you'll need to download the latest raw image of the Raspberry Pi hard-float Raspbian "wheezy" OS from the Raspberry Pi site [downloads](http://www.raspberrypi.org/downloads) section.

You'll then need to format your SD card. I suggest using a program to do this for you. If you're running a Mac, [PiWriter](http://sourceforge.net/projects/piwriter/) is easy and reliable. For a PC or Linux, you can choose from the list [here](http://elinux.org/RPi_Easy_SD_Card_Setup#Create_your_own). Note: you will need an SD writer for your computer. Macs often come with one, but you can also buy one that connects to USB for fairly inexpensively.

The SD formatter program will guide you through the process. Be patient, it may take some time to write to the card, and some programs will not give you a status update, so you'll likely have to wait for them to prompt you to take the card out. It may also ask you to unmount the card without taking it out of the slot.

Once this is done, the card should be ready to power your Raspberry Pi.

##Initializing the Raspberry Pi

Now you'll begin to interact with the Pi. 

####Option 1: External Display and Keyboard


##Updating and Upgrading Packages (eg. Applications)

The Raspbian OS image you just loaded onto the SD card is the bare minimum you need to get things going. In this step, you will launch the Pi for the first time, and update and upgrade all of the necessary packages (eg. applications).





##Getting Online

You'll need to update the wpa_supplicant information to get on a wireless connection that uses the WPA security key.

First, figure out the psk (an encoded version of your wireless password) by using the wpa_passphrase command

	$ wpa_passphrase [ssid] [password]

ssid is the name of the network (eg. Columbia University) and password is your password in plain text.

This will output something that looks like

	network={
		ssid="mynetwork"
		#psk="mypasswordinplaintext"
		psk=19e655769206f1d867e4b338f2f39b06a37c4d2a5265365438fd67d96afea44a
	}
  

  
##Updating Firmware

Following the video [here](https://www.youtube.com/watch?v=Vwrxep7oB24).