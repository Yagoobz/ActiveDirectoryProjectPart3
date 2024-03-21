<h2>Active Directory Project Part 3</h2>

<h3>Objectives</h3>
- Install & Configure Sysmon & Splunk onto Windows Target Machine & Windows Server
<br />
<br />

To start, I ensure that my network settings in VirtualBox are configured for a NAT network so that all virtual machines can be on the same network and still have internet access. This is achieved by navigating to "Tools" then "Network" and creating a new NAT network with the IPv4 address set to 192.168.10.0/24. Next, I assign this NAT network to each virtual machine.

On the Splunk server, I configure a static IP address to match the IPv4 address of the NAT network. To do this, I access the network configuration file by typing "sudo nano /etc/netplan/00-installer-config.yaml." Within this file, I modify the DHCP setting from "true" to "no," add an "addresses" command with the IP address, include another "addresses" command with Google's DNS IP, add "routes" with "default," and specify the gateway using "via." After saving these changes, the configuration is complete and ready to use.
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>
