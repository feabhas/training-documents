# Testing the target connection
A target project is set up using either VmWare or VirtualBox

This assumes you have already installed a Virtual Machine image and it is running.


## Testing the compiler

The target project is located at
```
~/course-material/templates/target
```

1. Open a `Terminal` window using the icon on the toolbar. 

2. To navigate to the project directory type:
```
$ cd ~/course-material/templates/target
```
![cd ~/course-material/templates/target](/images/c501-cd-dir.png)

3.	Build the project.
From the command line, type
```
$ scons
```
![scons](/images/c501-scons.png)

Once built, the main target image file is created at `build/debug/Application.elf`.

## Connect the target to the laptop

In the lid of the Hardware box you should find three cables:
* a power supply cable
* two USB cables

The should also be a USB mini-hub and possibly a multi-way power extension cable. Any other cables are superfluous. 

1. Connect the power cale to the black PSU on the in the target box
2. Connect one USB cable to the J-Link (the grey box) and the USB-hub
3. Connect the other USB cable to the USB connection on the top-left ob the (green) target board, and to the USB-hub
4. Connect the USB-hub to the laptop

![target](/images/targetJPG.JPG)

## Connect USB to Virtual Machine - VmWare Workstation Player

1. When VmWare is running and the USB-hub is connected a dialog will popup `New USB Device Detected`
2. The first dialog will be for the Segger J-Link. Go ahead and select **connect to a virtual machine** and selcet the appropriate VM, e.g.
   
![target](/images/c501-vmware-usb.png)

3. A second dialog will then popup for the serial port _Future Devices FT232R USB UART_. Again, go ahead and connect to the virtaul machine, e.g.
   
![target](/images/c501-vmware-ftdi.png)


3. Test the connection by typing 
```
$ lsusb
```
in a terminal window, e.g.

![target](/images/c501-lsusb.png)

If the J-Link drivers are correctly configured the the LED on the top of the J-Link should turn green.


## Connect USB to Virtual Machine - VirtualBox

1. Unlike VmWare, VirtualBox doesn't present a dialog when the USB-hub is attached to the laptop. Instead, use the menu **Devices->USB** to show the various USB devices for you laptop (_note: the list will depend on your laptop_)

![target](/images/c501-usb.png)

2. Enable both **Segger J-Link** and **FTDI FT232R USB UART**, e.g.
   
![target](/images/c501-usb-connected.png)

3. Test the connection by typing 
```
$ lsusb
```
in a terminal window, e.g.

![target](/images/c501-lsusb.png)

If the J-Link drivers are correctly configured the the LED on the top of the J-Link should turn green.


## Testing Debug channel

1.	Launch Ozone.
Ozone is located in the 'Programming' sub-menu of the 'start' menu, this will load the project

![download](/images/c501-ozone-ready.png)

2.	Download and run the project on the target by clicking on the green `Download and reset program` option (also `F5`). First time you will be presented with a license dialogue:

![download](/images/license-message.png)

Tick the `Do not show again` box and then click `Yes`.

This will re-flash the target (be patient - status is displayed along the bottom of the window). Once downloaded it will break at main and is ready to run. **There is currently a bug/feature where Ozone doesn't run to main but breaks before that**

![run](/images/ozone-semihosting-bug.png)

Click the `Continue` button (also `F5`) until you break at main (3 to 4 time).

![main](/images/c501-ozone-main.png)

1. Run the application by clicking the `Continue` button (also `F5`). When running the default project will display debug messages in the bottom right-hand window (`Terminal`), e.g.
   
![running](/images/c501-ozone-running.png)

This indicates that:
* The compiler is fully functional
* The j-tag channel is correctly configured
* The debugger (Ozone) is correctly configured

## Serial check

To check the serial port is correctly configured, in a terminal window, type:
```
$ ls -l /dev/ttyUSB0
```
If this successfully displays a `dialout` entry, then the Serial port is ready to use.

