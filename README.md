# Network-Monitoring-System-with-Raspberry-Pi

**INTRODUCTION**
Snort is an open source-based tool. It is a Network Monitoring system. It uses rule-based language where we can write our own rules and design the system. Rules such as alert, log, drop the connection, block etc. It has three different modes packet sniffer, packet logger and NIPDS. In this project, we will cover the custom rules about SSH, FTP, Ping traffic, Telnet and block or reject Ip traffic. It can behave as IDS and IPS. 

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/c0d5e063-760e-44a4-bbfc-798bf2092f0e)

As shown in figure 1, the snort is a open source tool used on raspberry pi which is capable of monitoring networks. If there is any suspicious activity going on, then it is going to alert and display the alert in console. In this way it protects the trusted network from malicious attackers. 

**PROEJCT WORK**

**Aim:** To build NIDS with raspberry pi using snort. 

**Apparatus:** 
•	Raspberry Pi
•	HDMI to HDMI cable
•	Monitor

**Software tools:** 
•	Snort 

**Network Diagram:**


![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/778c1246-e179-4946-aa05-92a656534359)


**INSTALLATION**
Installation of Raspbian OS
•	Download the Raspbian image from the https://www.raspberrypi.com/software/operating-systems/

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/0d60cb90-9019-41a4-a323-3e7cbe07e5c4)

•	Use the Etcher to flash/write the image into the SD card. (Download Etcher https://www.balena.io/etcher#download-etcher).

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/0dccdbd8-b657-4b59-bce2-d9a59a8c72fb)

•	Insert the SD card into Raspberry Pi and the Raspbian OS will boot up and prompt to enter the username and password. 
•	Now the Raspberry Pi and the SD card are ready to work.

Installation of Snort 
•Firstly, Update the raspberry pi to install Snort. By using the below command.
**_sudo apt update _**

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/e0aac24f-6a86-4ea0-8cfb-a548339854df)

•	Installing snort by using the below command 
**_Sudo apt install snort_**

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/5e4922f5-d7c0-4782-9276-d57fb812e49b)

•	Now enter the IP address of the network.
•	To Check the version of snort
**_snort --version_**_

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/618fd045-0b47-4187-8b0e-c4f31641cc78)

**RULES**
_Customize Rules in Snort_
•	There are community rules for snort, we can customize our own rules and make it work. The more powerful rules we implement the more powerful IDS. 
•	Firstly, we need to write rules in local rules and comment on all the other rules to see the working of the rule. (disable the other rules).

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/40cc1bcc-248f-4ca7-b0b2-961ab2bf713a)

•	Now, using the below command to view the local rules write our own rules.
**Sudo nano /etc/snort/rules/local.rules**__
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/711b8b57-8d03-49ea-941b-8c6ec43360b1)

•	Writing our first rule about ping. If we receive any ping from another IP that should be detected and saved in the log file also display the alert in the console.
**alert icmp any any -> $HOME_NET any (msg:”ICMP Ping Detected” ; sid:1001; rev:1)**

 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/b9c4a840-3199-4c58-ae9e-093aac2e459b)

•	Writing the second rule about SSH. When another computer tries to log in with the SSH the logs need to be displayed in the console as well as saved in the log file. By using this command, we can alert the system if someone login. 
**alert tcp any any  -> $HOME_NET 22 (msg:”SSH Authentication attempt” ; sid:1001 ; rev:1)**


![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/9dad8e3c-df4b-4b09-a1f6-d8905c99f272)

•	The third rule is about FTP. It is similar rule to SSH.Whenever some tries to login we need to get the alert in our console and save the logs. 
**alert tcp any any -> $HOME_NET 21 (msg:"FTP Authentication Attemp";sid:1003;rev:1;)**
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/a2bef1e9-1778-49ce-8feb-95416ef41853)

•	Similarly, we can write our own rules for Telnet, UDP. Also, make it even more complex as it can alert us when a user enters the password correctly or not. 
•	Writing HTTP rules. For this, we will firstly install Apache server and host a website. We need to host a website in the following process. 
•	Install the apache2._ **sudo apt install apache2**_
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/637d05d5-6a4c-427b-ba5f-12690ec4e9af)


•	After the installation is done create a directory in /var/www/html/ for the website and start the apache server.
**Sudo mkdir /var/www/html/mywebsite**
**Sudo mkdir /var/www/html/mywebsite/index.html**
**Sudo service apache2 start**
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/84414066-a1de-40a1-9762-b4d84a5ba6ef)

•	Access the website with the  **https://IPaddress/directory/index.html **
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/73f1d0f1-0c3b-4931-96ae-596a146d0e53)

•	For , the HTTP rule we will write both inbound rule and outbound rule. 
**alert tcp any any -> $HOME_NET 80 (msg:”HTTP traffic INBOUND” ; sid:1004;rev:1)**
**alert tcp $HOME_NET 80 -> any any (msg:”HTTP traffic OUTBOUND” ; sid:1005;rev:1) **


**WORKING**


For starting the snort and monitoring the network traffic we use below command where, -l is for logs , -i is for interface -A is for showing it in console – c is for checking the rules from that configure file.
 Command used: **sudo snort -q -l /var/log/snort/ -i wlan0 -A console -c /etc/snort/snort.conf**__
•	Working on the ping rule. 
 

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/1ff8c53e-086b-4351-85c4-6aaf8a8fe5e8)


![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/5c0eeb42-1977-4682-8cd4-91e3dfd5a5aa)


•	Working SSH rule.
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/cae52a8a-ee58-4c4b-8a10-ed6b2ad5f830)

 

•	Working FTP rule. 
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/75018f28-1aa4-4032-a23a-3515a2bfd513)

![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/9da6e5e0-c1a8-4bb2-9842-b66bb3a381b7)


•	Working of HTTP
 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/347ecb10-e57b-4e45-a562-cd83896d576e)

 
![image](https://github.com/srisowmya2000/Network-Monitoring-System-with-Raspberry-Pi/assets/59259117/712d6d3d-f7e6-420e-83e1-a19160319c63)


**CONCLUSION**
Snort monitors network traffic for suspicious activity and generates alerts when potential security threats are detected. Snort is open-source, highly customizable, and widely used in both small and large-scale network environments for enhancing network security. By regularly monitoring network traffic and responding to alerts generated by Snort, network administrators can quickly identify and respond to potential security threats before they cause significant damage to the network.




 
**REFERENCE**
1.	https://untanglingtheworld.medium.com/snort-an-intrusion-detection-system-bc53fa761767
2.	https://www.snort.org/
3.	https://koayyongcett.medium.com/snort-installation-in-kali-linux-from-the-source-9a005558a2ea
4.	https://bin3xish477.medium.com/installing-snort-on-kali-linux-9c96f3ab2910
5.	https://jarayax.medium.com/snort-installation-steps-in-kali-linux-8a6dcfc3eedf 
6.	https://www.youtube.com/watch?v=U6xMp-MIEfA 











