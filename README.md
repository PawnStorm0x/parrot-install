Installing on WSL2
Note: This option only works in Windows 10, where our final set up will include a GUI and CLI for Parrot OS by upgrading a Debian WSL app.

WSL 2
Follow this tutorial 202 via Microsoft Docs

Requirements:
Running Windows 10, updated to version 2004
Build 19041 or higher.
64-bit Machine (for Kernel Update)

Install WSL
Open PowerShell as Admin and run:

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
Restart your machine and complete WSL install

Update to WSL 2
Open PowerShell as Admin

wsl --set-default-version 2
If you see

WSL 2 requires an update to its kernel component. For information please visit aka.ms/wsl2kernel

Download and install the MSI x64 Linux Kernel.

Install Debian
Open the Microsoft Store and select Debian distro
Get and install the application
Create a unix user account (ex. parrot) and password
Ensure that Debian is running on WSL 2
Powershell

wsl --list --verbose
wsl --set-version <distribution name> <versionNumber>
Success! You now have WSL2 and Debian installed :slight_smile:

Debian → Parrot :parrot:
Let’s get to the fun stuff!
In your CLI, copy and paste to get the parrot-install.sh 459
Alternatively, copy the raw text if you don’t trust me :sob:

curl https://raw.githubusercontent.com/PawnStorm0x/parrot-install/master/parrot-install.sh -o parrot-install.sh
chmod a+x parrot-install.sh
sudo ./parrot-install.sh
Select 1. Install Core Only
Why: This will get the correct sources.list for the parrot repositories.
I highly recommend this option!

Update works, but upgrade returns:

404 archives/packages not found
Previous: /etc/apt/sources.list
New: /etc/apt/sources.list.d/*

debian.list (empty)
parrot.list (auto-generated)

My parrot.list
deb https://deb.parrot.sh/parrot/ rolling main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling main contrib non-free
deb https://deb.parrot.sh/parrot/ rolling-security main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling-security main contrib non-free
You may use ANY mirror, but I the rolling release was the only successful endpoint for upgrading.

When you enter root you should see the parrot CLI styling :sparkling_heart:
![image](https://user-images.githubusercontent.com/6363104/186901878-9bd79cae-fb25-4abd-a675-30fb468236a5.png)

To Upgrade
sudo su
parrot-upgrade
This will install all dependencies for the OS, and important packages, but no security tools, or GUI.

Installing a GUI
apt-get install parrot-interface parrot-interface-full parrot-tools-full
Installing Security Tools
apt -y --allow-downgrades install parrot-interface parrot-interface-full parrot-tools-full
To Access the GUI
As of now, we should have a WSL 2 instance with all of the bells and whistles.
So, to access this machine, we will use the onboard application, Remote Desktop Connection.

LAN XRDP Server
This will create an access port on your Local Area Network through the WSL network adapter.
Allows you to connect to an operating system.
Note: You can only remote into a single WSL machine at a time (b/c they share an IP address)

apt-get install xrdp
I created a bash file (rdp.sh) with

#!/bin/bash
/etc/init.d/xrdp start
ip addr
Output
Side Note: I recommend Windows Terminal (Preview) for WSL2
![image](https://user-images.githubusercontent.com/6363104/186901970-1e7d35a2-c71c-472d-bbb0-43b9cbf0ef26.png)

Highlighted is the IP Address you are looking for.
Enter this into the RDC program and click Connect.
![image](https://user-images.githubusercontent.com/6363104/186902088-2bec9f51-0983-44de-a88b-d5fbc36a8805.png)

Once you enter the creds into the XRDP Server, you may now access the Parrot GUI.
And it works :slight_smile:
![image](https://user-images.githubusercontent.com/6363104/186902155-cf2f7db7-92dc-472b-a5a0-12c5a68b3453.png)


