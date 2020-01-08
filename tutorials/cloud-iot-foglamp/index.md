# Using Dianomic FogLamp with Google Cloud IoT Core

FogLamp from Dianomic provides a robust solution for getting data from sensors
into data stores.  The following diagram illustrates the platform architecture
for FogLamp in the context of Google Cloud IoT Core (Cloud Iot Core).



In the context of getting data into Google Cloud from sensors, the FogLamp
architecture consists of a service that bridges sensors that communicate
through FogLamp using the "South" service to Google Cloud Platform as a data
store on the "North" service.

For this quickstart we'll be using a Raspberry Pi to host the FogLamp service.

## Setup your Raspberry Pi with the Raspbian Buster image
This tutorial requires that you use the Debian Buster version of the Raspberry
Pi software because it uses packages built for that verision of Linux. To get
Buster on your Raspberry Pi, first download a Raspbian buster image from the
[Raspbian downloads page](https://www.raspberrypi.org/downloads/raspbian/).

Next, unzip the image you downloaded, which will create a file that ends in
`.img`. For example, if you downloaded the "full" version of Raspbian, the
file would be named `2019-09-26-raspbian-buster-full.img`.

After you have unzipped the image file, create the SD card image for your
Raspberry Pi using a disk image utility. Raspberry Pi recommends that you use
[balenaEtcher](https://www.balena.io/etcher/).

For more information about flashing your Raspberry Pi, see the detailed instructions
on [the Raspberry Pi install documentation](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

At this point, you will need to set up access the Raspberry Pi. If you intend
to access the Raspberry Pi in headless mode, follow the instructions for
[setting up headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)
on the Raspberry Pi site. Otherwise, connect your Raspberry Pi to a keyboard,
mouse, and display for local access.

Finally, you will want to do any final configuration of the Raspberry Pi with
the [raspi-config](https://www.raspberrypi.org/documentation/configuration/raspi-config.md)
utility such as expanding the filesystem and setting the user password.

You will be performing subsequent steps on the Raspberry Pi itself either over
SSH if you're working headless or on the Raspberry Pi hardware if you're using
the Desktop UI.

## Download the FogLamp packages
From your Raspberry Pi, download the FogLamp packages to the file system. For
the purposes of this tutorial, we'll be placing them in a folder named
`foglamp` in your user's home directory.

```
mkdir $HOME/foglamp
cd $HOME/foglamp
wget https://foglamp.s3.amazonaws.com/1.7.0/buster/armv7l/foglamp-1.7.0_armv7l_buster.tgz
tar -xzvf  foglamp-1.7.0_armv7l_buster.tgz
```

The folder will now have the base packages for FogLamp. Let's also download the
packages for the Google Cloud IoT Core North plugin.

```
wget
https://download-directory.github.io/?url=https://github.com/GoogleCloudPlatform/community/tree/master/tutorials/cloud-iot-foglamp/foglamp-north-gcp-1.7.0.arm71.tgz
tar -xzvf foglamp-north-gcp-1.7.0-arm71.tgz
```

At this point, the `foglamp` folder in your user's home directly will contain
all the requirements for getting your Raspberry Pi up and running.

## Install FogLamp and FogLamp GUI on your Raspberry Pi

After setting up your Raspberry Pi with the Raspbian Buster image and getting
the FogLamp packages, it's time to install the FogLamp service and FogLamp GUI.

The FogLamp Graphical User Interface (GUI) makes it easier to configure and
control FogLamp from your Raspberry Pi.

Browse to the folder that you downloaded the FogLamp packages to and install
the FogLamp and FogLamp GUI packages.
```
pushd $HOME/foglamp/foglamp/1.7.0/buster/armv7l/
sudo apt -y install foglamp-1.7.0-armv7l.deb
sudo apt -y install foglamp-gui-1.7.0.deb
```

Now you can start the FogLamp service and check its status.

```
export FOGLAMP_ROOT=/usr/local/foglamp
$FOGLAMP_ROOT/bin/foglamp start
$FOGLAMP_ROOT/bin/foglamp status
```

You can also navigate to the FogLamp GUI by navigating with your web browser
to the web server that started running on the Raspberry Pi when you installed
the FogLamp GUI. To determine the IP address of your server, run the ifconfig
command.

```
ifconfig
```

It will output the IP address of the network interfaces. The following example
shows how the output might look.

```
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.217  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::9f73:3b56:93c2:4ed4  prefixlen 64  scopeid 0x20<link>
        ether dc:a6:32:03:b7:cb  txqueuelen 1000  (Ethernet)
        RX packets 426133  bytes 305815327 (291.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 274458  bytes 172317386 (164.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

In the above example, the ip address is `192.168.1.217` so you would open the
URL http://192.168.1.217.

When you navigate to the web server, you will see a dashboard similar to the
following image.

![FogLamp GUI dashboard](image/foglamp-gui.png)

Now it's time to install the plug-ins for connecting to Google Cloud Platform
and generating data.

## Install a FogLamp South plugin for generating data
Before you set up a North Plugin for publishing data, you need data to publish.
One quick approach to getting data is to install the "random" plugin.

```
cd $HOME/foglamp/foglamp/1.7.0/buster/armv7
sudo apt install ./foglamp-south-random-1.7.0-armv7l.deb
```

Now navigate to the "South" menu on the left side of the FogLamp GUI navigation
and click the `Add +` button as seen in the following image.

![Add button](image/foglamp-south-add.png)

Now, select the Random plugin, give your plugin a name, e.g. `random`, and
click next.

![Add random plugin](image/foglamp-south-add-random.png)

Click `Next` again on the next screen, then select `Done` on the final
form.

Navigating to the Dashboard or Assets & Readings menu on the FogLamp GUI will
show random data getting generated by the newly configured South plugin.

With data getting generated, it's time to start publishing that data to
Cloud IoT Core.

## Install and configure the FogLamp North Plugin for Cloud IoT Core
Now that you have data getting generated by the South Plugin, it's time to
publish that data to Google Cloud through the Iot Core device bridge. The
FogLamp …

## Conclusions and next steps
Now that you have seen how FogLamp works on a Raspberry Pi, it's a great time
to take a look at their other hardware partners such as … to



Feedback:
Raspberry Pi Zero lab
Filters
Events

