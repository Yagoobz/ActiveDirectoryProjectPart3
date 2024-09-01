<h2>Active Directory Project Part 3</h2>

<h3>Objectives</h3>
- Install & Configure Sysmon & Splunk onto Windows Target Machine & Windows Server
<br />
<br />

To start, I ensure that my network settings in VirtualBox are configured for a NAT network so that all virtual machines can be on the same network and still have internet access. This is achieved by navigating to "Tools" then "Network" and creating a new NAT network with the IPv4 address set to 192.168.10.0/24. Next, I assign this NAT network to each virtual machine.

On the Splunk server, I configure a static IP address to match the IPv4 address of the NAT network. To do this, I access the network configuration file by typing "sudo nano /etc/netplan/00-installer-config.yaml." Within this file, I modify the DHCP setting from "true" to "no," add an "addresses" command with the IP address, include another "addresses" command with Google's DNS IP, add "routes" with "default," and specify the gateway using "via." After saving these changes, the configuration is complete and ready to use.

<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/4210e995-f35f-4ba9-93e0-865f2fd6f141" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To ensure everything is configured correctly, I execute the command "ip a" to check the IP address, confirming that it has changed according to the static IP configuration. Additionally, I perform a ping to google.com to verify if I receive a response, and I do, indicating that the network setup is working perfectly.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/a0d977a0-11b9-47ec-b020-bd8488aef227" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To download Splunk, I visit their website, create an account, and download the free trial. Once the file is downloaded onto my host computer, I launch the Splunk virtual machine I previously created. Within the virtual machine, I create a new folder named "share" and proceed to mount the shared folder to the "share" directory using the command "sudo mount -t vboxsf -o uid=1000,gid=100." Next, I add the Splunk download file to VirtualBox and navigate to the "share" directory to initiate the Splunk download process.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/9c3d71f1-3f5b-415c-a948-a08a6c72acd8" height="30%" width="70%" alt="Disk Sanitization Steps"/>

This doesn’t fully download Splunk, so to initiate the full download and installation of Splunk, I switch to the Splunk user by executing the command "sudo -u splunk bash." Next, I navigate to the "bin" directory using "cd bin" as it contains all the Splunk binaries. From there, I run the installer using "./splunk start" to begin the Splunk installation process.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/4698d23d-8d1f-4d92-8d65-0b0dae928ea8" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To ensure that Splunk starts automatically every time the virtual machine reboots, I navigate to the "bin" directory within Splunk and execute the command "sudo ./splunk enable boot-start -user splunk." This command configures Splunk to start up during system boot, using the specified "splunk" user.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/f52e3354-aad5-446f-a288-155203919cfa" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To begin installing Splunk Universal Forwarder and Sysmon on both the target machine and the server, I first need to change the IPv4 address to avoid any potential IP conflicts. I accomplish this by navigating to the Windows settings and selecting "Change adapter options." From there, I enter the appropriate numbers to modify the IPv4 address, ensuring that there are no conflicts. This step is crucial to ensure smooth installation and operation of Splunk Universal Forwarder and Sysmon.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/d939c065-9774-4b7a-b1c3-4a5dfdb54fdd" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To install Splunk Universal Forwarder, I begin by downloading it from Splunk's website and running the installation process. Concurrently, while Splunk is downloading, I also download Sysmon along with a configuration file from GitHub. In PowerShell, I navigate to the directory where I extracted the Sysmon config file and proceed to download Sysmon. Once both installations are completed, I have Splunk Universal Forwarder and Sysmon successfully installed on the target machine.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/9a21cd9b-5093-415b-884f-a50e68775ca1" height="30%" width="70%" alt="Disk Sanitization Steps"/>

This step is crucial in configuring Splunk Universal Forwarder to send specific data to the Splunk server. This is achieved through the configuration of the "inputs.conf" file, which is located in the Program Files directory on the C drive under the Splunk folder. It's important not to edit the file in the "default" folder to avoid causing issues, so a new file must be created in the "local" folder. Using administrative privileges, I open Notepad and paste the configuration contents obtained from a GitHub page. I save the Notepad file into the "local" folder, restart Splunk Forwarder via Windows Services, and everything is set up for data transmission to the Splunk server.
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/179ec5f6-1822-440a-9e18-7ffe18c946e9" height="30%" width="70%" alt="Disk Sanitization Steps"/>

To finalize the Splunk server configuration, I log in to the Splunk web portal using the credentials I created during the Splunk installation on the server. In the inputs.conf file, all the inputs are being sent over to an index named "endpoint." Since this index doesn't exist in Splunk yet, I create a new index named "endpoint" and ensure that the Splunk server is enabled to receive the data.

To enable data reception, I navigate to "Forwarding and receiving" in Splunk and create a new receiving port using port 9997, which is the default Splunk port for receiving data. If everything is set up correctly, I should start seeing data coming in from the target machine.

To verify this, I go to "Search & Reporting" in Splunk and search for "index=endpoint." If configured correctly, I should see over 10,000 events with the "host" field showing "TARGET-PC" and the "source" field displaying application, system, security, and Sysmon from the inputs.conf file.

Next, I repeat the same process for the Windows Server. Upon completion, I confirm that both the target machine and server are up and running and being recognized by Splunk. Everything seems to be working correctly!
<br />
<br />
<img src="https://github.com/Yagoobz/ActiveDirectoryProjectPart3/assets/145611184/48ec3f93-5d96-48e6-828c-907d643358c8" height="30%" width="70%" alt="Disk Sanitization Steps"/>
