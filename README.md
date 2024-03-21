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

To download Splunk, I visit their website, create an account, and download the free trial. Once the file is downloaded onto my host computer, I launch the Splunk virtual machine I previously created. Within the virtual machine, I create a new folder named "share" and proceed to mount the shared folder to the "share" directory using the command "sudo mount -t vboxsf -o uid=1000,gid=100." Next, I add the Splunk download file to VirtualBox and navigate to the "share" directory to initiate the Splunk download process.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/3a82a793-2a8e-470d-9eef-b61d9e7cdeb7" height="30%" width="70%" alt="Disk Sanitization Steps"/>

This doesn’t fully download Splunk, so to initiate the full download and installation of Splunk, I switch to the Splunk user by executing the command "sudo -u splunk bash." Next, I navigate to the "bin" directory using "cd bin" as it contains all the Splunk binaries. From there, I run the installer using "./splunk start" to begin the Splunk installation process.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/d1894414-7365-4533-8f94-1dfca04aad39" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To ensure that Splunk starts automatically every time the virtual machine reboots, I navigate to the "bin" directory within Splunk and execute the command "sudo ./splunk enable boot-start -user splunk." This command configures Splunk to start up during system boot, using the specified "splunk" user.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/7c1c4da3-375d-4ef1-8a15-0a81518103cc" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To begin installing Splunk Universal Forwarder and Sysmon on both the target machine and the server, I first need to change the IPv4 address to avoid any potential IP conflicts. I accomplish this by navigating to the Windows settings and selecting "Change adapter options." From there, I enter the appropriate numbers to modify the IPv4 address, ensuring that there are no conflicts. This step is crucial to ensure smooth installation and operation of Splunk Universal Forwarder and Sysmon.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/c8c492f6-a18b-4a11-b3dc-55180da4ae13" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To install Splunk Universal Forwarder, I begin by downloading it from Splunk's website and running the installation process. Concurrently, while Splunk is downloading, I also download Sysmon along with a configuration file from GitHub. In PowerShell, I navigate to the directory where I extracted the Sysmon config file and proceed to download Sysmon. Once both installations are completed, I have Splunk Universal Forwarder and Sysmon successfully installed on the target machine.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/015e584a-2687-4186-a731-88ea242f4d17" height="30%" width="70%" alt="Disk Sanitization Steps"/>

:)
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/5887f97a-e422-4c4e-a9cb-3803ed62990e" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To finalize the Splunk server configuration, I log in to the Splunk web portal using the credentials I created during the Splunk installation on the server. In the inputs.conf file, all the inputs are being sent over to an index named "endpoint." Since this index doesn't exist in Splunk yet, I create a new index named "endpoint" and ensure that the Splunk server is enabled to receive the data.

To enable data reception, I navigate to "Forwarding and receiving" in Splunk and create a new receiving port using port 9997, which is the default Splunk port for receiving data. If everything is set up correctly, I should start seeing data coming in from the target machine.

To verify this, I go to "Search & Reporting" in Splunk and search for "index=endpoint." If configured correctly, I should see over 10,000 events with the "host" field showing "TARGET-PC" and the "source" field displaying application, system, security, and Sysmon from the inputs.conf file.

Next, I repeat the same process for the Windows Server. Upon completion, I confirm that both the target machine and server are up and running and being recognized by Splunk. Everything seems to be working correctly!
<br />
<br />
<img src="..." height="30%" width="70%" alt="Disk Sanitization Steps"/>
