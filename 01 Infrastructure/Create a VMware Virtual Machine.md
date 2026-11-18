## Purpose

Create the virtual machines in VMware. The operating system installation steps are in [[Install Linux Mint]] and [[Install Debian]].

## Linux Mint virtual machine

1. Open VMware to create a new Linux server machine.
2. Press **File**, then **New virtual machine**. A window opens. Click **Next**.

![[create-a-vmware-virtual-machine--fig-01--new-vm-wizard-window.png]]
*the window that opens after File → New virtual machine*

3. Click **Next** again.
4. You do not need to install the image now. It will be done later.
5. Choose **Linux** and the version you want at the bottom. Ubuntu 64-bit was chosen in this lab.
6. Choose a name for the machine, then the location. It is better to create a folder named after the machine for good management. Click **Next**.
7. About 40 GB was needed in this lab; choose what suits your situation. Click **Next**.
8. If you notice an error in the configuration, click **Customize hardware** to change what you need. Otherwise click **Finish**.
9. The machine is ready. Go to **Edit virtual machine settings**, click **CD/DVD (SATA)**, then choose the image of your operating system. You must have downloaded the image beforehand.
10. Click **Add** and add a new network adapter. Click **OK**.
11. Start the machine with **Power on this virtual machine**.

## Debian virtual machine

1. Download the Debian ISO (amd64) from the Debian website.
2. In VMware, click **Create a new virtual machine**.
3. Select **Typical (recommended)** and click **Next**, then **Next** again.
4. Choose the option shown:

![[create-a-vmware-virtual-machine--fig-02--installer-option.png]]
*the option to choose in the new virtual machine wizard*

5. Choose the operating system you want to install. Debian, the most recent version, was chosen in this lab.
6. Give the virtual machine a name and choose a location to save it.
7. Set the maximum disk size, then click **Next** and **Finish**. Reserve more than 20 GB. Depending on your needs, for example 60 GB or 100 GB.
8. Use the image that you already downloaded.

## Next

[[Install Linux Mint]] · [[Install Debian]]
