# Cybersecurity-Homelab
Virtualized Network using VMware for the purpose of developing and demonstrating networking skills.  
I started this project by following an article on Cyberacademy (https://cybercademy.org/cybersecurity-homelab-project/).

In this document I will be recording my progress in this project as I work on it.  

# 11/9/2024 - 11/22/24
Created the Project summary document, a summary of the project so far is as follows:
(Please note this document is being created after I started the project. I will be including a summary of progress up until now, but it will not be as detailed/accurate as I probably won't remember everything)

I first implemented a Windows Server 2022 to host Active Directory services.  This server also hosts DNS for the local domain and DHCP.  The domain hosted on this server is called CYBER.local
I then implemented a Windows 10 enterprise workstation and an Ubuntu 24.04.1 workstation.  I then joined these 2 workstations to the CYBER.local domain.  
I created two user accounts on the active directory server for each workstation, dmartinez and pgriffin.  Since these user accounts are located on the active directory server, both user
accounts can be used to log onto either workstation, although I am using one for each workstation, dmartinez on Ubuntu and pgriffin on Windows.
I also implemented a Pfsense router/firewall to act as the default gateway and to segment the virtual network from my home network.  I also had to reconfigure the way VMware was routing
traffic on the network.  Before I had the router setup, all the VMs were using NAT to have the same IP as my PC.  I setup a vnet and added all the VMs to it, which allowed them to 
communicate as if they were all linked to the same switch.  I also had to disable VMware's DHCP service to prevent it from conflicting with my local DHCP service.
I then implemented VPN server using OpenVPN on an Ubuntu 24.04.1 server.  I configured this server to access user information from the Active Directory server usind LDAP.  
I plan on enabling LDAPS in the future, but my goal at the time was to simply implement an functioning VPN server.  I then installed the OpenVPN client on the Windows workstation to
test the functionality of the VPN Server.  I ran into an issue where DNS requests were timing out while connected to the VPN server.  I fixed by configuring the VPN server to have 
the clients send DNS requests to the google DNS server (8.8.8.8) and then configured the VPN server to not allow client traffic to be routed within the local network to prevent any
security risks.  I plan to investigate whether this solution is actually secure, but for now my goal of implementing a working VPN server is complete.

# 1/20/2025
After implementing the VPN Server, I was starting to hit the limits of what my desktop could support while also running my native OS. Therefore, I decided to invest a bit of money to buy an old PC off Ebay. The specs are as follows:
* 32 GB RAM
* i7-4790 Processor
* 128 GB SSD
* 1 Gbps Ethernet

Since this is my first server, I decided to follow the project spec and install Ubuntu, although I did install a newer version (24.04). I downloaded the OS from the Ubuntu website and flashed it to a USB drive using balenaEtcher as recommended by the Ubuntu website. Installation went off without a hitch. In order to enable remote management, I followed the project guide to enable SSH and RDP on the new server. The only problem I ran into enabling SSH was forgetting to allow it past the local firewall, which was fixed with a simple command. RDP was a bit more of a challenge though. The initial configuration of RDP was fairly simple as Ubuntu comes with gnome-remote-desktop by default, I just needed to enable remote control in the settings. This worked initially, although the next day when I tried to RDP into the server after restarting it, I found it no longer worked. I tried SSH, which was still working fine, which led me to realize that you cannot RDP into the server until after the user has logged in. This presented a signifigant problem, as the goal of my configuration was for the server to be headless. I first tried to enable auto-login to bypass this issue, but this didn't work as a password is still required to unlock the login keychain. Through research, I learned that you can bypass the password requirement by setting the keychain password to be empty. I decided to go with this as a solution temporarily, although this poses significant securtiy risks as it means my passwords will be stored in plaintext. I plan on creating a script using the python-gnomekeyring package that I can run with SSH in order to unlock the keychain. I also ran into another trivial issue with RDP. Since my configuration doesn't have a monitor connected to the server, the video drivers don't run. This means that RDP will have no visual information to display. I plan on solving this buy using a dummy plug to trick the video drivers into running even though there will still be no monitor.
