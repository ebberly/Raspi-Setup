#Raspberry Pi Setup

Raspberry Pi setup for the Site to Site course taught at the Columbia University Graduate School of Architecture, Planning and Preservation. We will be using the Raspberry Pi with a version of the Linux open source OS called Raspbian in order to connect remote physical spaces through the web.

##Formatting the SD Card with the Raspbian OS

_Note: Much of this step and the following are also documented [here](http://lifehacker.com/5976912/a-beginners-guide-to-diying-with-the-raspberry-pi), though I had trouble with the SD card formatter, and the config options were different as that tutorial might have been for an earlier release of the Raspbian OS._

The Raspberry Pi uses an SD card (4GB minimum, class 4, 6 or 10; 10 is fastest) as its hard drive, the only source of persistent data storage. You can either buy a blank SD card and install the OS yourself using your own computer, or you can buy a SD card with the OS already loaded (I recommend formatting the SD card yourself, both to have a better understanding of how it's working, and to have more control over the version).

To begin, you'll need to download the latest raw image of the Raspberry Pi hard-float Raspbian "wheezy" OS from the Raspberry Pi site [downloads](http://www.raspberrypi.org/downloads) section.

You'll then need to format your SD card. I suggest using a program to do this for you. If you're running a Mac, [PiWriter](http://sourceforge.net/projects/piwriter/) is easy and reliable. For a PC or Linux, you can choose from the list [here](http://elinux.org/RPi_Easy_SD_Card_Setup#Create_your_own). Note: you will need an SD writer for your computer. Macs often come with one, but you can also buy one that connects to USB for fairly inexpensively.

The SD formatter program will guide you through the process. Be patient, it may take some time to write to the card, and some programs will not give you a status update, so you'll likely have to wait for them to prompt you to take the card out. It may also ask you to unmount the card without taking it out of the slot.

Once this is done, the card should be ready to power your Raspberry Pi.

##Initializing the Raspberry Pi

Now you'll begin to interact with the Pi. The Pi is a mini-computer, but it doesn't come with all of the external 

####Option 1: External Display and Keyboard


####Option 2: Using a Laptop with an Ethernet Connection

You'll need to make a change to the SD card on your computer before you load it into the Pi. Swap your cmdline.txt file with the one here, and also add cmdline.direct and cmdline.normal to your SD card, then eject it. This will add a static IP address to your Pi, so that when it has booted, you can ssh into the command line through a physical ethernet cable connection in order to set up wi-fi on the Pi.

Insert the SD card and an ethernet cable connected to a laptop and plug in the power supply. Give it a few minutes to boot up, and then open a terminal window and type

	$ ssh pi@169.254.0.2

It will likely prompt you to see if you will accept the RSA key, you should respond yes and press enter. You will then be prompted to type your password which will be

	raspberry

##Configing the Pi

Now you should have the $ command prompt showing. The first thing you want to do is configure your Pi. Do this by typing

	$ sudo raspi-config

First select Expand Filesystem by selecting the option using the arrow keys and pressing enter, then confirm your choice. 

Next, change the user password, which is the password you will use from now on when logging into your Pi with the username pi.

Next, choose Internationalisation Options and set your timezone to America > New_York.

Next, go into advanced options and enable SSH.

Next, go back into advanced options and choose Memory Split, and set the GPU memory to 128.

Finally, add your Pi to Rastrack if you want to be part of the global registry of Raspberry Pis.

Select finish, and choose to reboot the pi. Wait a few moments to try to ssh in again as above. Continue to try the ssh command as long as you get connection timeout errors, this just indicates that the pi is in the process of rebooting and is not ready.


##Getting Online

Before you can install the necessary packages to work with your pi, you'll need to establish an internet connection. This is achieved through the pre-installed wpa_supplicant package, which you will now configure to work with whichever wireless network you have available to your pi.

First, you'll update the wpa_supplicant.conf file in /etc/wpa_supplicant/ by providing an encoded password for your wireless network. Generate this encoded password by doing the following:

	$ wpa_passphrase [ssid] [password]

where ssid is the name of the network (eg. Columbia University) and password is your password in plain text.

This will output something that looks like

	network={
		ssid="mynetwork"
		#psk="mypasswordinplaintext"
		psk=19e655769206f1d867e4b338f2f39b06a37c4d2a5265365438fd67d96afea44a
	}

Copy this entire block of text, and then you will use the native linux/unix text editor, vi, to alter the wpa_supplicant.conf file. In it's default state, it is somewhat difficult to use, so be sure to follow the below steps precisely:

	$ cd /etc/wpa_supplicant
	$ sudo vi wpa_supplicant.conf

This will bring up the vi text editor. Here, tap the down arrow key twice to move the cursor to the beginning of the second (and last) line. The vi editor has two modes, control and insert. When it launches, it is in control mode, so you will need to get into insert mode before you can paste in the network id and passphrase. To do this, as well as create a new blank line below the current last line, type *o and press enter (that's asterix and a lowercase letter o). Then, hit return to free up another line, and finally press ctrl/command+v (ie. do a paste using the appropriate keyboard combination for a Mac or PC). Then, press the esc key to get back into control mode, and then type :wq and hit enter. This will execute the write (w) command, followed by the quit (q) command. This should return you to the command line $ prompt.

Now you are ready to reboot. First, install your Wi-Pi USB dongle by inserting it into one of the two USB ports on the pi, then type the following at the command line prompt

	$ sudo reboot

Once your pi reboots, you should see the Wi-Pi dongle begin to flash blue.

##Updating and Upgrading Packages (eg. Applications)

The Raspbian OS image you just loaded onto the SD card is the bare minimum you need to get things going. In this step, you will launch the Pi for the first time, and update and upgrade all of the necessary packages (eg. applications).


##Updating Firmware

Following the video [here](https://www.youtube.com/watch?v=Vwrxep7oB24).

##Useful Linux Commands

$ top
$ free -h
$ ps aux | grep taskname
$ vncserver -kill :$DISPLAY
