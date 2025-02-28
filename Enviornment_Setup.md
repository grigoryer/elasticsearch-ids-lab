## UTM Configuration
This lab uses local Virtual Machines. The Virtual Machine software I am using is UTM which is configured with my Mac M1 (aarch64/arm64) architexture. UTM can accomidate for other archexture thorugh the Emulation, however for this lab I will set-up through virtualization. This lab can be repurposed to amd64 with instalaltion of the amd64 architexture software. 

For the main **elk_server** I will be using Ubuntu 24.04.1 server.
First, after installing the Virtual Machine software of choice (UTM), find the specified Ubuntu ISO image.

The Ubuntu iso image can be found [here](https://ubuntu.com/download/server/).  

Next, after selecting virtualize, and selecting the linux preconfiguration. Change the Boot ISO Image to the ISO file previosuly downloaded.

<img width="440" alt="iso file change" src="https://github.com/user-attachments/assets/6f32ee8e-1e88-47b7-b38e-d6e9287d2201" />


The following set-up in UTM will have you allocate appropriate amount of RAM and Storage for the Virutal Machine. After creating the VM it is important to configure its network settings. 

UTM currently offers 4 seperate network modes which have there own purpose: 

1. Shared Network: Connects to external internet from the host, however will not be seen by other devices.
2. Emulated VLAN: Creates a LAN where VMs can communicate to each other but not outside of the LAN.
3. Bridged (Advanced): Directly connects VM to the router, acting like a phyiscal device and receving its own IP. 
4. Host Only: Only connects to host and other VMs on the network, no external internet access.


This lab will use the Bridged network mode since we will be installing elasticsearch with wget. You can use other modes such as the VLAN or Host Only if you route the eterneal connection to the VMs. The selected enumerated network card should be virtio-net-pci. 

After saving the changes, start the VM and start the ubuntu server installation process.

## Ubuntu Installation

After starting the VM, select "Try and Install Ubuntu Server" with keys ofcourse :).  

Select preferred language, and ubuntu server. For network configuruation since we are on the Bridged mode the DHCP of the router should auotoimically assign you an IPv4 address. Click continue and select the default settings and storage allocation. Select the server name (hostname), and a username and password for your user. Remember these as this will be the root and sudo password aswell.

Next, select select the installation of openSSH, and import personal keys if you want to. You can also connect with just the username and password aswell. Dont select any server snaps. 

After the installation process is complete the machine should ask you to reboot. After rebooting, the screen should should black. Close the VM  compleltly and important you need to clear the ISO boot image file. If you do not do this the installation process will restart on the next boot. After complteting this step and starting the VM. You should see the prompted login for the server, after entering the username and password. The following screen will appear.

<img width="329" alt="Screenshot 2025-02-02 at 17 48 45" src="https://github.com/user-attachments/assets/d7f76548-51ed-4b8d-87e0-c1325b03f17a" />

***



