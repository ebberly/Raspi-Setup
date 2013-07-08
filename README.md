#Raspi-Setup

Setup steps for Raspberry Pi

##Steps

###Getting Online

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
  

  
