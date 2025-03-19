# Import-VM
How to import virtual machine

1.VMware Workstation setup

  1.1 Network adapter setup for all endpoint
    In VMware Workstation there is essential setup we have to make to let we testing the system
  and get result smoothly, which is virtual network setting.
  
  1. Upon on VMware Workstation window, on top left corner enter ”Edit” > ”Virtual Net-
    work Editor...”

  ![image](https://github.com/user-attachments/assets/9c83fe2c-cf10-4925-baa0-f4b3352b0ade)

  3. Go to lower right corner then enter ”Change Setting”

  ![image](https://github.com/user-attachments/assets/70ae3ac2-8c05-41ef-8d18-6383af1aa64c)

  4. Enter ”Add Network”, then choose desire network, For our case we choose VMnet2.
    ![image](https://github.com/user-attachments/assets/d3e81bcf-2fbb-4f39-9419-1c214869c849)

  5. Select VMnet 2, correct option ”Use local DHCP service to distribute IP address to
    VMs”as

  Then change subnet IP to ”192.168.1.0” with subnet mask of ”255.255.255.0” and click
  ”OK”.

  ![image](https://github.com/user-attachments/assets/1bdf8f82-d7a7-4eb0-af54-35dfe2ccf803)

  
  1.2 Import virtual machine to VMware Workstation
  
  1. Upon VMware Workstation window, on top left corner, enter ”File” > ”Open”
  ![image](https://github.com/user-attachments/assets/78b5fa9d-adfe-4c58-9900-00a0fa84f424)

  3. Then choose desire virtual machine and the file need to have file extension of ”OVA”
    or ”OVF”
  ![image](https://github.com/user-attachments/assets/b85278de-2749-43de-bd19-8724f4871992)

2.Network adapter setup for all virtual machine

  1. Upon VMware Workstation window, perform right-click on desire virtual station and
  enter ”Setting”
![image](https://github.com/user-attachments/assets/11a82932-1b55-4f32-9d44-6911fbd26e01)

  2. In setting window, enter ”Add”, select ”Network Adapter” and then click ”Finish”
  ![image](https://github.com/user-attachments/assets/8561b148-d99b-400b-a682-039ebed2ccd7)

  3.Enter one of the ”Network adapter”,

  • first network adapter will use ”Bridged” option.

  • Second network adapter will use ”Custom” option then choose VMnet2 that create
  earlier.
  ![image](https://github.com/user-attachments/assets/5b47e567-76ec-47cc-a5a2-9bbc536b5628)

3 Wazuh server setup and configuration

  3.1 Install Wazuh server using OVA
  1. Enter website Wazuh - Virtual Machine OVA
  2. In website, On ”Packet List”table, click on ”wazuh-4.11.1.ova (sha512)” on the right
    of table, then download begin. Wait until finish downloading
    ![image](https://github.com/user-attachments/assets/d871858d-472e-450e-bee1-6d76b5d0baa1)

  3. Open VMware Workstation window, then add new virtual machine into VMware Work-
    station, choosing file ”wazuh-4.11.1.ova”
  ![image](https://github.com/user-attachments/assets/ff6c2088-c25a-4496-9650-dc6014e20de0)

3.2 Essential initial setup on start
Uppon finished adding Wazuh into VMware Workstation and start VM, we need further
configuration to be able to access Wazuh web UI

  1. Once Wazuh finished start up all services, use following command to update outdated
service

$ sudo yum upgrade

3. Then you can access Wazuh Dashboard by enter URL of Wazuh server IP and use
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

3.3 Configuration to receive log from syslog protocol

1. In Wazuh server terminal using following command to enter config file /var/ossec/
etc/ossec.conf

$ sudo nano /var/ossec/etc/ossec.conf

3. Within configure file adding following line to enable Wazuh to receive log from syslog
then save file.

<remote>
  
<connection>syslog</connection>

<port>514</port>

<protocol>tcp</protocol>

<allowed-ips>192.168.1.0/24</allowed-ips>

<local_ip>192.168.1.131</local_ip>

</remote>

• <allowed-ips>: will be subent IP of our system
  
• <local.ip>: is IP of Wazuh server

5. Perform following command to restart Wazuh-manager to apply change.

$ systemctl restart wazuh-manager

4 Window 10 VM setup and configuration

4.1 Install Wazuh agent onto system

1. First entering Wazuh Dashboard, using following specific URL and using Wazuh IP
https://<wazuh-server-ip>/app/endpoints-summary/agents-preview/deploy
![image](https://github.com/user-attachments/assets/d824b470-3edc-4a39-8f41-8c90e3e307c9)
• Section 1: Then choosing package Windows ”MSI 32/64 bits”

• Section 2 on Server addresses section: enter Wazuh server IP

• Section 3 Optional setting section: you may enter ”Agent name” and choosing
”Group” as you desired

• Section 4 : Copy commands in grey box then run it in PowerShell with adminis-
trator privilege.

• Section 5 : Running following command in PowerShell to start Wazuh agent ser-
vice

$ NET START WazuhSvc

4.2 Configuration to enable file integrity features

1. Configure file located in C:/Program Files (x86)/ossec-agent/ossec.conf when
enter the file added following line in section of ”File Integrity Monitoring” to constantly
monitoring directories like Download, Documents and Desktop

<directories realtime="yes">C:\Users\*\Downloads</directories>

<directories realtime="yes">C:\Users\*\Documents</directories>

<directories realtime="yes">C:\Users\*\Desktop</directories>

Or you may check in the following image
![image](https://github.com/user-attachments/assets/ce9ae702-e48d-4861-8fdc-39cb615d05fb)

4.3 Configuration to allow collect log from Window Defender

1. Within configuration file ossec.conf in ”Log Analysis” section, add following line to
allow agent to capture log from Window Defender

<localfile>
  
<location>Microsoft-Windows-Windows Defender/Operational</location>

<log_format>eventchannel</log_format>

</localfile>

Or may check in the following image
![image](https://github.com/user-attachments/assets/1423e5d3-d6ed-48c6-84a5-68c91e6809a6)

5.Ubuntu server setup and configuration

5.1 Install Wazuh agent onto system

1. First entering Wazuh Dashboard, using following specific URL and using Wazuh IP
https://<wazuh-server-ip>/app/endpoints-summary/agents-preview/deploy

• Section 1: Then choosing package Linux operating system of test endpoint

• Section 2 on Server addresses section: enter Wazuh server IP

• Section 3 Optional setting section: you may enter ”Agent name” and choosing
”Group” as you desired

• Section 4 : Copy commands in grey box then run it i privilege.

• Section 5 : Running following command in PowerShell to start Wazuh agent ser-
vice

$ sudo systemctl daemon-reload

$ sudo systemctl enable wazuh-agent

$ sudo systemctl start wazuh-agent

B.5.2 Configuration to allow syslog to forwarding log to Wazuh server

B.5.3 Option: Configuration on Wazuh agent to allow forwarding log from others log
file

B.6 Fortinet firewall setup

B.6.1 Setup logging system to send log to Wazuh server via syslog

B.7 RouterOS/MikroTik setup and configuration

B.7.1 Setup logging system to send log to Wazuh server via syslog

B.7.2 Import essential decoder and rule for MikroTik

B.7.3 Customize rule for MikroTi
