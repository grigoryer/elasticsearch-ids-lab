## UTM Configuration

This lab uses local virtual machines. The virtual machine software I am using is **UTM**, which is configured to run on my Mac M1 (aarch64/arm64) architecture. UTM can accommodate other architectures through emulation; however, for this lab, I will set it up using virtualization. This lab can also be repurposed for **amd64** by installing the corresponding **amd64** software.

For the main **elk_server**, I will be using **Ubuntu 24.04.1 Server**.  
First, after installing the virtual machine software of choice (**UTM**), download the specified Ubuntu ISO image.

The Ubuntu ISO image can be found [here](https://ubuntu.com/download/server/).

Next, after selecting **Virtualize** and choosing the Linux preconfiguration, change the **Boot ISO Image** to the ISO file previously downloaded.

<img width="300" alt="iso file change" src="https://github.com/user-attachments/assets/4dec3f35-df6a-4c9e-8fba-c14ba71e95d2" />

<br>

The following setup in UTM will allow you to allocate an appropriate amount of RAM and storage for the virtual machine. After creating the VM, it is important to configure its network settings.

UTM currently offers four different network modes, each serving a unique purpose:

1. **Shared Network**: Connects to the external internet from the host but is not visible to other devices.
2. **Emulated VLAN**: Creates a LAN where VMs can communicate with each other but not with external networks.
3. **Bridged (Advanced)**: Directly connects the VM to the router, acting as a physical device and receiving its own IP.
4. **Host Only**: Only connects to the host and other VMs on the same network, with no external internet access.

This lab will use **Bridged network mode** since we will be adding the **Elasticsearch** repo using `wget`. You can use other modes, such as VLAN or Host Only, if you configure external network routing for the VMs. The selected enumerated network card should be **virtio-net-pci**.

After saving the changes, start the VM and begin the Ubuntu server installation process.

---

## Ubuntu Installation

After starting the VM, select **"Try and Install Ubuntu Server"** using the keyboard.

Select your preferred language and choose **Ubuntu Server**.  
For network configuration, since we are using **Bridged mode**, the DHCP server of the router should automatically assign an IPv4 address. Click **Continue**, and keep the default settings for storage allocation. Set a **server name (hostname)**, along with a **username** and **password** for your user. Remember these credentials, as they will be used for both root and `sudo` access.

Next, select the installation of **OpenSSH**, and import personal keys if needed. You can also connect using just the username and password. Do **not** select any server snaps.

Once the installation process is complete, the system will prompt you to reboot. After rebooting, the screen may appear black. **Close the VM completely**, and—this is important—**clear the ISO boot image file**. If you skip this step, the installation process will restart on the next boot.

After completing this step and restarting the VM, you should see the server login prompt. Enter your **username** and **password**, and the following screen should appear:

![Login Screen](https://github.com/user-attachments/assets/d7f76548-51ed-4b8d-87e0-c1325b03f17a)

[Next: ELK Server Setup](elkserver_setup.md)
