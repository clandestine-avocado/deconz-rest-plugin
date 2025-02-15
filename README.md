Introduction
============
- [x] this is a complete item
- [ ] this is an incomplete item
- [x] @mentions, #refs, [links](),
**formatting**, and <del>tags</del>
supported
- [x] list syntax required (any
unordered or ordered list
supported)


The deCONZ REST plugin provides a REST-API to access Zigbee 3.0 (Z30), Zigbee Home Automation (ZHA) and Zigbee Light Link (ZLL) lights, switches and sensors from Xiaomi Aqara, IKEA TRÅDFRI, Philips Hue, innr, Samsung and many more vendors.

A list of supported Zigbee devices can be found on the [Supported Devices](https://github.com/dresden-elektronik/deconz-rest-plugin/wiki/Supported-Devices) page.

To communicate with Zigbee devices the [RaspBee](https://phoscon.de/raspbee?ref=gh) / [RaspBee&nbsp;II](https://phoscon.de/raspbee2?ref=gh) Zigbee shield for Raspberry Pi, or a [ConBee](https://phoscon.de/conbee?ref=gh) / [ConBee&nbsp;II](https://phoscon.de/conbee2?ref=gh) USB dongle is required.

To learn more about the REST-API itself please visit the [REST-API Documentation](http://dresden-elektronik.github.io/deconz-rest-doc/) page.<br>

---
### Interacting with the API - Getting Started
>Unless you already know the IP and port of your gateway, you need to do a GET request to ```https://phoscon.de/discover``` to get the IP and other internal gateway >data



Example Response:
```json
[
  {
    "id": "00212EFFFF04F20A",
    "internalipaddress": "192.168.1.215",
    "macaddress": "00212EFFFF04F20A",
    "internalport": 80,
    "name": "Phoscon-GW",
    "publicipaddress": "108.28.9.36"
  }
]
```


Next, you need an API key. With the ```IP:PORT``` data, you can request an API key. Do a POST to ```http://192.168.1.215:80/api``` with the JSON payload of ```{"devicetype":""my application}```. Before running this POST, you MUST unlock the gateway as follows:

<li>Open the Phoscon App
<li>Click on Menu → Settings → Gateway
<li>Click on “Advanced” button
<li>Click on the “Authenticate app” button
<li>Run the POST request

Example Response:
```
[
    {
      "success": {
        "username": "B2F077CD18"
      }
    }
  ]
```

Now you can build and REST commands you want with ```http://IP:PORT/<API KEY>/```

Examples include:
    
  List all lights via ```GET``` post:
   
    http://192.168.1.215:80/api/B2F077CD18/lights 
    
  List all sensors via ```GET``` post:
      
    http://192.168.1.215:80/api/B2F077CD18/sensors

  Get the details of a device by its ID via ```GET``` post

    http://192.168.1.215:80/api/B2F077CD18/lights/5    
    
  Turn on a light with a JSON payload of ```{"on": true}``` via ```PUT``` post
    
    http://192.168.1.215:80/api/B2F077CD18/lights/5/state    
  
  Turn off a light with a JSON payload of ```{"on": false}``` via ```PUT``` post
    
    http://192.168.1.215:80/api/B2F077CD18/lights/5/state    
    
    
  
  
  
  
The REST-API plugin is implemented in C++ using the [deCONZ C++ API Documentation](https://phoscon.de/deconz-cpp).

For community based support with deCONZ or Phoscon, please visit the [deCONZ Discord server](https://discord.gg/QFhTxqN). 

### Phoscon App
The Phoscon App is a browser based web application and supports lights, sensors and switches. For more information and screenshots visit the [Phoscon App Documentation](https://phoscon.de/app/doc?ref=gh).


### Release Schedule

deCONZ beta releases are scheduled roughly once per week. After 2–3 betas a stable version is released and a new beta cycle begins. The stable release is usually published between 1st — 15th of the month.

Current Beta: **v2.12.0-beta**  
Current Stable: **v2.11.5**

Next Beta: [v2.12.1-beta](https://github.com/dresden-elektronik/deconz-rest-plugin/milestone/14).
Next Stable: **v2.12.x** expected in June.

Installation
============

##### Supported platforms
* Raspbian ~~Jessie~~, Stretch and Buster
* Ubuntu Xenial, Bionic and Focal Fossa (AMD64)
* Windows 7 and 10

### Install deCONZ
You find the instructions for your platform and device on the Phoscon website:

* [RaspBee](https://phoscon.de/raspbee/install?ref=gh)
* [RaspBee&nbsp;II](https://phoscon.de/raspbee2/install?ref=gh)
* [ConBee](https://phoscon.de/conbee/install?ref=gh)
* [ConBee&nbsp;II](https://phoscon.de/conbee2/install?ref=gh)

**Important:** If you're updating from a previous version **always make sure to create an backup** in the Phoscon App and read the changelog first.

https://github.com/dresden-elektronik/deconz-rest-plugin/releases

### Install deCONZ development package (optional, Linux only)

**Important:** The deCONZ package already contains the REST-API plugin, the development package is **only** needed if you wan't to modify the plugin or try the latest commits from master branch.

    sudo apt install deconz-dev

#### Get and compile the plugin

1. Checkout the repository

        git clone https://github.com/dresden-elektronik/deconz-rest-plugin.git

2. Checkout the latest version

        cd deconz-rest-plugin
        git checkout -b mybranch HEAD

3. Compile the plugin

        qmake && make -j2

**Note** On Raspberry Pi 1 use `qmake && make`

4. Replace original plugin

        sudo cp ../libde_rest_plugin.so /usr/share/deCONZ/plugins

Precompiled deCONZ packages for manual installation
===================================================

The deCONZ application packages are available for the following platforms and contain the main application and the pre-compiled REST-API plugin.

* Windows  http://deconz.dresden-elektronik.de/win/
* Raspbian http://deconz.dresden-elektronik.de/raspbian/beta/
* Ubuntu and Debian 64-bit http://deconz.dresden-elektronik.de/ubuntu/beta/

To manually install a Linux .deb package enter these commands:

    sudo dpkg -i <package name>.deb
    sudo apt-get install -f

Headless support for Linux
--------------------------

The deCONZ package contains a systemd script, which allows deCONZ to run without a X11 server.

1. Enable the service at boot time

```bash
$ sudo systemctl enable deconz
```

2. Disable deCONZ GUI autostart service

The dresden elektronik sd-card image and default installation method autostarts deCONZ GUI.
The following commands disable the deCONZ GUI service:

```bash
$ sudo systemctl disable deconz-gui
$ sudo systemctl stop deconz-gui
```

Hardware requirements
---------------------

* Raspberry Pi 1, 2B, 3B, 3B+ or 4B
* [RaspBee](https://phoscon.de/raspbee?ref=gh) Zigbee shield for Raspberry Pi
* [RaspBee&nbsp;II](https://phoscon.de/raspbee2?ref=gh) Zigbee shield for Raspberry Pi
* [ConBee](https://phoscon.de/conbee?ref=gh) USB dongle for Raspberry Pi and PC
* [ConBee&nbsp;II](https://phoscon.de/conbee2?ref=gh) USB dongle for Raspberry Pi and PC

3rd party libraries
-------------------
The following libraries are used by the plugin:

* [SQLite](http://www.sqlite.org)
* [qt-json](https://github.com/lawand/droper/tree/master/qt-json)
* [colorspace](http://www.getreuer.info/home/colorspace)

License
=======
The plugin is available as open source and licensed under the BSD (3-Clause) license.


