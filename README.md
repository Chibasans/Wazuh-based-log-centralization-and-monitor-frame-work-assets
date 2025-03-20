# Tested Endpoint Virtual Machine
In this section i will included all the link to download OVA and OVF to import into VMware Workstation

*All of this link are from official*

1. **Wazuh Virtual Machine OVA** : https://packages.wazuh.com/4.x/vm/wazuh-4.11.1.ova
   or https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
2. **Fortinet Firewall Virtual Machine** : https://www.forticloud.com/#/
  - You may need to make an registrationa first, if you didn't do one
  - Once finished logged into main page, on top menu > Support > Download : VM Imange
  - ![image](https://github.com/user-attachments/assets/df8dc976-e8ae-4c56-8372-bec412b9477c)
  - Then Choose lasted version or prefer one
  - ![image](https://github.com/user-attachments/assets/c0dce2aa-9db1-404e-a559-2b12c81fe751)
3. **RouterOS Self hosted - Mikrotik** : https://mikrotik.com/download
  - On the main page, scroll down untill see section "Cloud Hosted Router"
  - Enter this section then choose "OVA Template"
  - ![image](https://github.com/user-attachments/assets/ebaa60a6-8aca-47ea-9dfa-b3355ec6d4ba)
4. **Ubuntu Server** : https://releases.ubuntu.com/jammy/
  - In main page choose option "Server Install Image"
  - ![image](https://github.com/user-attachments/assets/910843d6-512f-4831-af9a-0f75f3703c53)
  - This one is not OVA or OVF (imported virtual machine) to install .iso extension, you have to "Create New Virtual Machine" instead of import virtual machine

# VMware Workstation setup

- Network adapter setup for all endpoint
    In VMware Workstation there is essential setup we have to make to let we testing the system
  and get result smoothly, which is virtual network setting.
  
  - Upon on VMware Workstation window, on top left corner enter ”Edit” > ”Virtual Net-
    work Editor...”

  ![image](https://github.com/user-attachments/assets/9c83fe2c-cf10-4925-baa0-f4b3352b0ade)

  - Go to lower right corner then enter ”Change Setting”

  ![image](https://github.com/user-attachments/assets/70ae3ac2-8c05-41ef-8d18-6383af1aa64c)

  - Enter ”Add Network”, then choose desire network, For our case we choose VMnet2.

  ![image](https://github.com/user-attachments/assets/d3e81bcf-2fbb-4f39-9419-1c214869c849)

  - Select VMnet 2, correct option ”Use local DHCP service to distribute IP address to
    VMs”as

  Then change subnet IP to ”192.168.1.0” with subnet mask of ”255.255.255.0” and click
  ”OK”.

  ![image](https://github.com/user-attachments/assets/1bdf8f82-d7a7-4eb0-af54-35dfe2ccf803)


# Import virtual machine to VMware Workstation
  
1. Upon VMware Workstation window, on top left corner, enter ”File” > ”Open”

  ![image](https://github.com/user-attachments/assets/78b5fa9d-adfe-4c58-9900-00a0fa84f424)

2. Then choose desire virtual machine and the file need to have file extension of ”OVA”
    or ”OVF”

  ![image](https://github.com/user-attachments/assets/b85278de-2749-43de-bd19-8724f4871992)

# Network adapter setup for all virtual machine

  - Upon VMware Workstation window, perform right-click on desire virtual station and
  enter ”Setting”

![image](https://github.com/user-attachments/assets/11a82932-1b55-4f32-9d44-6911fbd26e01)

  - In setting window, enter ”Add”, select ”Network Adapter” and then click ”Finish”

![image](https://github.com/user-attachments/assets/8561b148-d99b-400b-a682-039ebed2ccd7)

  - Enter one of the ”Network adapter”,

  • first network adapter will use ”Bridged” option.

  • Second network adapter will use ”Custom” option then choose VMnet2 that create
  earlier.
  
![image](https://github.com/user-attachments/assets/5b47e567-76ec-47cc-a5a2-9bbc536b5628)

# Wazuh server setup and configuration

1. Essential initial setup on start
Uppon finished adding Wazuh into VMware Workstation and start VM, we need further
configuration to be able to access Wazuh web UI

  - Once Wazuh finished start up all services, use following command to update outdated
service

$ sudo yum upgrade

2. Then you can access Wazuh Dashboard by enter URL of Wazuh server IP and use
following Login and Password

URL: https://<wazuh.server.ip>

user: admin

password: admin\textbf{}

You may find <wazuh.server.ip> by using following command

$ ip address

As result

![image](https://github.com/user-attachments/assets/904421ac-e0c6-4e76-9693-6d70d96a9154)

From the result, we will using IP of ”192.168.1.131” to access Wazuh Dashboard
through URL in any browser

# Configuration to receive log from syslog protocol

1. In Wazuh server terminal using following command to enter config file /var/ossec/
etc/ossec.conf

$ sudo nano /var/ossec/etc/ossec.conf

2. Within configure file adding following line to enable Wazuh to receive log from syslog
then save file.
  
 > <remote>
 > <connection>syslog</connection>
 > <port>514</port>
 > <protocol>tcp</protocol>
 > <allowed-ips>192.168.1.0/24</allowed-ips>
 > <local_ip>192.168.1.131</local_ip>
 > </remote>

• <allowed-ips>: will be subent IP of our system
  
• <local.ip>: is IP of Wazuh server

3. Perform following command to restart Wazuh-manager to apply change.

$ systemctl restart wazuh-manager

4.Window 10 VM setup and configuration



