**Build GPS tracking system using Raspberry Pi Zero W**

We will be able to build a GPS tracking system using Raspberry Pi Zero W. We will be designing data streaming and visualisation tool to view the detailed tracking information of a moving object.  

**Hardware requirements**

- GPS with UART terminals
- [Raspberry Pi Zero W module](https://www.raspberrypi.com/products/raspberry-pi-zero/)
- [Micro SD Card](https://www.amazon.in/Sandisk-Ultra-UHS-I-Class-Memory/dp/B00812K4V4)
- [Power Supply](https://www.electronicscomp.com/raspberry-pi-zero-w-power-supply-5v-2amp-india)
- USB Card Reader
- Micro USB Cable
- Jumper Wires
- Portable Wi-Fi Dongle
- Computer/Laptop

**Software requirements**

- Python
- [Advanced IP Scanner](https://www.advanced-ip-scanner.com/)
- [BalenaEtcher](https://www.balena.io/etcher/)
- [Putty](https://www.putty.org/)
- [RealVNC](https://www.realvnc.com/en/raspberrypi/)
- [u-center](https://www.u-blox.com/en/product/u-center)

**Installing OS** 

- Refer to [this](https://www.raspberrypi.com/documentation/computers/getting-started.html) link to install latest desktop version. 

**Additional Configuration**

- Add a blank ssh file and wpa\_supplicant.conf file after adding SSID and Password.
- Use [Advanced IP Scanner](https://www.advanced-ip-scanner.com/) to find the IP address.
- If you face issues in loading OS to the Raspberry Pi then use [BalenaEtcher](https://www.balena.io/etcher/) for the same. 
- Download in install [putty](https://www.putty.org/) to login command line access.
- Download and install [RealVNC](https://www.realvnc.com/en/raspberrypi/) to login with GUI access.
  - Enable VNC on the Raspberry Pi in raspi-config
- Download [u-center](https://www.u-blox.com/en/product/u-center) / [VisualGPS](https://www.visualgps.net/) software to test the GPS module.

**Set static IP address**

- Launch putty and login to the Raspberry Pi
- Open dhcpcd.conf file 
  - $ sudo nano /etc/dhcpcd.conf
- Uncomment and modify the lines which has static ip\_address, routers and domain\_name to as per the required value
- Save, exit and reboot.
- Note: You can use the same IP address to login that you have entered in dhcpcd.conf file.

**Testing GPS Module on Computer**

- Connect GPS module to PC either with direct USB connection or with USB to UART converter module.
- Ensure the required drivers are installed and a com port is visible in Device manager.
- Install [u-center](https://www.u-blox.com/en/product/u-center) and launch.
- Go to Reciver -> port and choose the port number.
- If the [u-center](https://www.u-blox.com/en/product/u-center) module didn’t work the download and install [VisualGPS](https://www.visualgps.net/).
- Launch [VisualGPS](https://www.visualgps.net/) and set the com port in settings.


**Testing the GPS using Raspberry Pi**

- Connect GPS module to Raspberry Pi either using Serial pins or via USB
  - If you are using USB make sure that required driver is installed on the Raspberry Pi. You can confirm that by executing ‘$ lsusb’ on Raspberry Pi terminal.
- ![](Aspose.Words.5a581c54-9086-4f53-9b84-eb6c2e788dc4.001.png)
- Find out the required serial port or USB port that GPS is connected to. You can get that info by executing ‘$ ls /dev/tty\*’ on Raspberry Pi terminal.
- To see the incoming raw data from the serial or USB port then execute
  - $ cat /dev/tty<portnumber>

**Understanding GPSD Daemon**

GPSD is service daemon that monitors one or more GPS or AIS receivers attached to a host computer through serial or USB ports making the data of the sensors available to be queries on the TCP port 2947 of the host computer. To know more refer to [this](https://gpsd.gitlab.io/gpsd/installation.html) link.  

**Installation of GPSD**

- To install GPSD library execute following command on Raspberry Pi terminal
  - $ sudo apt-get install gpsd gpsd-clients python-gps -y
- To launch the application
  - $ sudo gpsd /dev/tty<portnumber> -F /var/run/gpsd.sock
  - $ cgps -s
- If you see blank/no data then execute 
  - $ sudo systemctl stop gpsd.socket
  - $ sudo systemctl disable gpsd.socket
  - $ sudo gpsd /dev/tty<portnumber> -F /var/run/gpsd.sock
  - $ cgps -s
- If you are still not able to see any data then make sure GPS is placed at an open space
- XGPS is a simple test client for gpsd with an X interface. To view this tool execute following on Raspberry Pi terminal with GUI interface
  - $ sudo gpsd /dev/tty<portnumber> -F /var/run/gpsd.sock
  - $ xgps

**Steps to make sure GPSD runs on boot**

- Open gpsd file
  - $ sudo nano /etc/default/gpsd
- Make sure following options are added to the files
  - START\_DAEMON=”true”
  - USBAUTO=”true”
  - GPSD\_OPTIONS=”/dev/tty<portnumber>”
  - # At end of the file add
  - GPSD\_SOCKET=”/var/run/gpsd.sock”
- Enable and start gpsd.socket
  - $ sudo systemctl enable gpsd.socket
  - $ sudo systemctl start gpsd.socket
- Reboot the Pi
  - $ sudo reboot
- Execute following command you should be able to see GPS data without any further commands
  - $ cgps -s

**Python code to get data from** 

- To view the raw data, execute the gps\_raw.py on the Raspberry Pi
  - $ sudo python gps\_raw.py
- To fetch the required data from the GPS execute gps\_data.py
  - $ sudo python gps\_data.py

**Ubidots**

We will be using [Ubidots](https://ubidots.com/) as an IoT dashboard for real-time tracking of the GPS location. This is an IoT application builder with data analytics and visualisation, converts sensors data into information that matters for business decisions, machine to machine interactions and many more.

**Dashboard installation and configuration**

- Let’s create a sample random number visualisation in Ubidots.
- Login to [Ubidots](https://ubidots.com/) first and add a device and open.
- Add a new variable – this will be the one which you will be sending from the Raspberry Pi.
- Click on the Dashboard and create a widget of your choice.
- Select the Device name and selected variable.
- On Raspberry Pi run random\_num.py file
  - $ sudo apt-get install python-setuptools
  - $ sudo apt-get-install python-pip
  - $ sudo pip install ubidots
  - $ sudo pyhton random\_num.py
- Make sure the token key and variable key are copied to the Python program before you execute
- Let’s run the ubigps.py to get the required visualisation in the dashboard, before that set the dash board with GPS trace and GPS Map widget. 
  - $ sudo python ubigps.py
- Make ubigps.py to run on boot
  - $ sudo crontab -e
  - # Add
  - @reboot sudo python /home/pi/ubigps.py


**Additional Info**

- You can also use initialState IoT platform to visualise the same data.
- To use initalState then run
  - $ sudo pip install ISStreamer
  - $ sudo pip install geopy
  - $ sudo python gps\_dashboard.py
