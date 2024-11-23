# Cybersecurity-Homelab
Virtualized Network using VMware for the purpose of developing and demonstrating networking skills.  
I started this project by following an article on Cyberacademy (https://cybercademy.org/cybersecurity-homelab-project/).

In this document I will be recording my progress in this project as I work on it.  
(Please note this document is being created after I started the project. I will be including a summary of progress up until now, but it will not be as detailed/accurate as I probably won't remember everything)

# 11/22/24
Created the Project summary document, a summary of the project so far is as follows:

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
