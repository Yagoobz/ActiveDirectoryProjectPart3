<h2>Active Directory Project Part 3</h2>

<h3>Objectives</h3>
- Install & Configure Sysmon & Splunk onto Windows Target Machine & Windows Server
<br />
<br />

To start, I ensure that my network settings in VirtualBox are configured for a NAT network so that all virtual machines can be on the same network and still have internet access. This is achieved by navigating to "Tools" then "Network" and creating a new NAT network with the IPv4 address set to 192.168.10.0/24. Next, I assign this NAT network to each virtual machine.

On the Splunk server, I configure a static IP address to match the IPv4 address of the NAT network. To do this, I access the network configuration file by typing "sudo nano /etc/netplan/00-installer-config.yaml." Within this file, I modify the DHCP setting from "true" to "no," add an "addresses" command with the IP address, include another "addresses" command with Google's DNS IP, add "routes" with "default," and specify the gateway using "via." After saving these changes, the configuration is complete and ready to use.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/8cbbc7ea-d618-4ca0-8227-eea136e56831" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To ensure everything is configured correctly, I execute the command "ip a" to check the IP address, confirming that it has changed according to the static IP configuration. Additionally, I perform a ping to google.com to verify if I receive a response, and I do, indicating that the network setup is working perfectly.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/36572ccd-a1f4-4881-996d-d473ff60268a" height="30%" width="70%" alt="Disk Sanitization Steps"/>
