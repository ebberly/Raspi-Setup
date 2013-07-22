#Configuring your Raspberry Pi as a Public Web Server

http://alexdberg.blogspot.com/2012/11/creating-public-web-server-on-raspberry.html

May need for github

	sudo apt-get install ca-certificates


##Install and Setup Firewall with iptables

The iptables package that is installed with the rpi-update firmware upgrader by Hexxeh that you previously installed will allow you to set up rules for a firewall that will protect your pi from malicious attacks once you open it up to the public internet. Install it as below:

1.	Check current iptables config

		$ sudo iptables -L

2.	Setup firewall in /etc/iptables.firewall.rules

		$ sudo vi /etc/iptables.firewall.rules

	Copy iptables.firewall.rules from this Github repository into this new file, save and close.

3.	Load the firewall

		$ sudo iptables-restore < /etc/iptables.firewall.rules

4.	Verify the rules were loaded

		$ sudo iptables -L

5.	Setup auto load of firewall pre-network connection on system restarts with a shell script

		$ sudo vi /etc/network/if-pre-up.d/firewall

	Copy firewall.sh from this Github repository into this new file, save and close.

6.	Make the file executable by changing the permissions

		$ sudo chmod +x /etc/network/if-pre-up.d/firewall

##Secure Against Dictionary Password Attacks with Fail2Ban

Fail2Ban will protect your pi from malicious servers that try to crack your password by using a dictionary to hammer your server with tons of login attempts.

1.	Install the fail2ban package with apt-get

		$ sudo apt-get install fail2ban


##Setup Router

http://portforward.com/english/routers/port_forwarding/Netgear/WNR2000/SSH.htm






