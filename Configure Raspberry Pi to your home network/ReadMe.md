# Configure Raspberry Pi to your home network

In this section, let us see how to configure a Raspberry Pi to your home network.

Here is the list of hardware that you need,

- Any Raspberry Pi Board
- 8 GB Micro SD Card
- Power supply for Raspberry Pi (Note: Please use the official power supply from Raspberry Pi)
- Laptop/PC

## Steps

- First, let's download the latest Raspberry Pi OS from the web. Google "Raspberry Pi OS download" and download the image.
- Then download and install the Balena Ether tool for loading OS to the Raspberry Pi memory card.
- Insert 8 GB Micro SD card into USB Memory Card Reader and connect to PC.
- Open the Balena etcher tool and select the Raspberry Pi OS that you downloaded in the previous step. Select the USB Memory Card Reader drive and click on flash. Wait till flash completes.
- Download the following configuration files on your computer
  - [ssh](https://github.com/IEEEProjectsBengaluru/RaspberryPiPythonProjects/blob/main/Configure%20Raspberry%20Pi%20to%20your%20home%20network/conf/ssh)
  - [userconf](https://github.com/IEEEProjectsBengaluru/RaspberryPiPythonProjects/blob/main/Configure%20Raspberry%20Pi%20to%20your%20home%20network/conf/userconf)
  - [wpa_supplicant.conf](https://github.com/IEEEProjectsBengaluru/RaspberryPiPythonProjects/blob/main/Configure%20Raspberry%20Pi%20to%20your%20home%20network/conf/wpa_supplicant.conf)
- Open wpa_supplicant.conf using any editor and replace <SSIDHERE> with your WiFi SSID name and replace <PASSWDHERE> with your WiFi password. Save and close the file.
  - Note: Add SSID and Password inside " ".
- Copy ssh,userconf and wpa_supplicant.conf to USB Memory Card Reader drive (boot) and saftely eject USB Memory Card Reader.
- Insert Micro SD card to Raspberry Pi slot and power on the Raspberry Pi board.
- Download and install Advance IP scanner. Launch Advance IP scanner tool after installation is done.
- Click on scan to find the Raspberry Pi IP address.
  - Note: Please select the IP address range as per the IP address range of your router. To find the IP address range execute ipconfig (on windows) / ifconfig (on unix) in command terminal.
- Download and Install Putty from the web. Launch Putty for the IP address that is discovered in Advance IP scanner and use 'pi' as user name and 'raspberry' as password.

## Software links

- Raspberry Pi OS: <https://www.raspberrypi.com/software/operating-systems/>  
- Balena Etcher: <https://www.balena.io/etcher/>  
- Advance IP Scanner: <https://www.advanced-ip-scanner.com/>  
- Putty: <https://www.putty.org/>
