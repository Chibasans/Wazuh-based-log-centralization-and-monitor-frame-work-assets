# Import-VM
How to import virtual machine

1.VMware Workstation setup
  1.1 Network adapter setup for all endpoint
    In VMware Workstation there is essential setup we have to make to let we testing the system
  and get result smoothly, which is virtual network setting.
    1. Upon on VMware Workstation window, on top left corner enter ”Edit” > ”Virtual Net-
    work Editor...”
    2. Go to lower right corner then enter ”Change Setting”
    3. Enter ”Add Network”, then choose desire network, For our case we choose VMnet2.
    4. Select VMnet 2, correct option ”Use local DHCP service to distribute IP address to
    VMs”as
    Then change subnet IP to ”192.168.1.0” with subnet mask of ”255.255.255.0” and click
    ”OK”.
  1.2 Import virtual machine to VMware Workstation
    1. Upon VMware Workstation window, on top left corner, enter ”File” > ”Open”
    2. Then choose desire virtual machine and the file need to have file extension of ”OVA”
    or ”OVF”
