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

First, you'll update the wpa_supplicant.conf file in /etc/wpa_supplicant/ which will store the information about which wireless networks you want your Pi to be able to connect to on startup. We will have to use the default vi text editor, which is difficult to use, so you will build the text files you need in a text editor on your computer and then paste them into vi.

Depending on whether your network requires a password, or is open (like the Columbia University network), you will follow a different set of instructions. For a network that requires a password, do the following:

1.	Open the wpa_supplicant.conf file from this repository in a code friendly text editor on your computer ( Textedit on a Mac, Notepad on a PC, or Sublime Text 2 on either is best; do not use Word).

2.	Back at the Pi command prompt, generate this encoded password by typing the following where ssid is the name of the network (eg. Columbia University) and password is your password in plain text.

		$ wpa_passphrase [ssid] [password]

3.	The output will look something like this:

		network={
			ssid="mynetwork"
			#psk="mypasswordinplaintext"
			psk=19e655769206f1d867e4b338f2f39b06a37c4d2a5265365438fd67d96afea44a
		}

4.	In your local text editor, delete the first network declaration block (everything from network= to the second curly brace), and change the ssid and psk values of the second network declaration block to match those you just generated using the Pi command prompt (note the ssid value is enclosed in quotes and the psk value is not).

5.	Back at the Pi command prompt, open the wpa_supplicant.conf file using vi by typing the following:

		$ cd /etc/wpa_supplicant
		$ sudo vi wpa_supplicant.conf

6.	This will bring up the vi text editor. This editor by default is difficult to navigate, so you will delete everything in the file in order to paste in the text from your local text editor. Vi is controlled by the keyboard, so it has a command mode and an insert mode. It loads in the command mode, so you can delete each line in the file successively by typing dd. Type dd multiple times until you have deleted every line.

7.	Typing the letter i will put you in insert mode, though it can sometimes be sticky, so press the letter i until a letter i is written to the vi text editor window. When it is, hit the backspace key to delete it.

8.	Now you are in insert mode and can paste. Copy all of the text in the wpa_supplicant.conf file you prepared on your local text editor. Back in the vi text editor, press the appropriate ctrl/cmd+v paste command for your computer (Windows or Mac). This should paste the entire text into vi.

9.	Press the ESC button to enter into command mode, and then type :wq and press enter. This will run the write (w) command to save the file, and the quit (q) command to close vi. This should return you to the pi command prompt. If so, your wpa_supplicant.conf file is ready.

If your network does not require a password, follow the above steps skipping #2 and #3, and instead of deleting the first network declaration block in #4, delete the second and alter the first with your ssid.

Next, you'll need to prepare your interfaces file to tell the pi which interfaces to external networks it should support and how.

1.	Open the interfaces file in the vi text editor by typing the following
	
		$ cd /etc/network
		$ sudo vi interfaces

2.	Delete every line as you did with wpa_supplicant.conf by typing dd until they are all removed.

3.	Open the interfaces file from this github repository in a local text editor as you did above with wpa_supplicant.conf and copy all of the text.

4.	As you did above, type the letter i until one appears on the screen, then delete it and paste in the text you just copied using ctrl/cmd+v.

5.	Type ESC followed by :wq to save the file and quit vi.

Now you are ready to reboot. First, install your Wi-Pi USB dongle by inserting it into one of the two USB ports on the pi, then type the following at the command line prompt

	$ sudo reboot

Once your pi reboots, you should see the Wi-Pi dongle begin to flash blue.

##Updating and Upgrading Packages (eg. Applications)

The Raspbian OS image you just loaded onto the SD card is the bare minimum you need to get things going. In this step, you will launch the Pi for the first time, and update and upgrade all of the necessary packages (eg. applications).

	$ sudo apt-get update
	$ sudo apt-get upgrade -y

##Installing Git

The Raspbian linux distribution comes with the apt-get package manager already installed. Since we'll be doing much with open source projects hosted on github, we'll want to install git as well. We'll use apt-get to do this:

	$ sudo apt-get install git-core -y


##Updating Firmware

Now, we want to update the Pi firmware using an open source updater created by github user Hexxeh (note: this may take a few minutes):

	$ sudo apt-get install rpi-update
	$ sudo rpi-update

Now you'll need to reboot for the changes to take:
	
	$ sudo reboot

##Installing and Configuring Vim

##Installing Node.js

	$ cd ~
	$ wget http://nodejs.org/dist/v0.10.13/node-v0.10.13-linux-arm-pi.tar.gz
	$ tar xvzf node-v0.10.13-linux-arm-pi.tar.gz
	$ rm node-v0.10.13-linux-arm-pi.tar.gz
	$ sudo mkdir /opt/node
	$ sudo cp -r node-v0.10.13-linux-arm-pi/* /opt/node
	$ rm -r node-v0.10.13-linux-arm-pi/
	$ vi .bash_profile

Add these two lines to your .bash_profile:

	PATH=$PATH:/opt/node/bin
	export PATH

Source the .bash_profile file to apply the changes

	$ source .bash_profile

Then, test to see if it installed Node.js and NPM:
	
	$ node -v
	$ npm -v

##Install RaspiCam Camera

Update the boot configuration file

	$ cd /boot
	$ vi config.txt

Add the following to the end of the file:

	start_file=start_x.elf
	fixup_file=fixup_x.dat

Then, update the raspi-config to include the camera firmware

	$ sudo raspi-config

Select the Enable Camera option and choose enable. It will then ask you if you would like to reboot, choose yes.

##Useful Linux Commands

	$ top
	$ free -h
	$ ps aux | grep taskname
	$ vncserver -kill :$DISPLAY
