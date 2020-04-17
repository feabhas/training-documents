# Testing the target connection
A target project is set up on the USB Flash Drive. 

## Connect the target to the laptop

In the lid of the Hardware box you should find three cables:
* a power supply cable
* two USB cables

The should also be a USB mini-hub and possibly a multi-way power extension cable. Any other cables are superfluous. 

1. Connect the power cale to the black PSU on the in the target box
2. Connect one USB cable to the J-Link (the grey box) and the USB-hub
3. Connect the other USB cable to the USB connection on the top-left ob the (green) target board, and to the USB-hub
4. Connect the USB-hub to the laptop

It the J-Link drivers are correctly configured the the LED on the top of the J-Link should turn green.

![target](/images/targetJPG.JPG)


## Testing the compiler

The target project is located at
```
~/course-material/templates/target
```

1. Open a `Terminal` window using the icon on the desktop. 
   
![mint](/images/mint-desktop.png)

2. To navigate to the project directory type:
```
$ cd ~/course-material/templates/target
```
![cd ~/course-material/templates/target](/images/terminal-pwd.png)

3.	Build the project.
From the command line, type
```
$ scons
```
![scons](/images/scons.png)

Once built, the main target image file is created at `build/debug/C++11-501.elf`.

## Testing Debug channel

1.	Launch Ozone.
Ozone is located in the 'Programming' sub-menu of the 'start' menu.
You will be prompted with a simple startup wizard:

![Ozone wizard](/images/Ozone-1.png)

Select 'Open Recent Project'.  This has been set up to launch the target project configuration `C++11-501.jdebug`. If `C++11-501.jdebug` doesn't appear in the list, then use the `Open Existing Project` option and open the file found at:
```
~/course-material/templates/target/C++11-501.jdebug
```

![download](/images/ready-to-download.png)

2.	Download and run the project on the target by clicking on the green `Download and reset program` option (also `F5`). First time you will be presented with a license dialogue:

![download](/images/license-message.png)

Tick the `Do not show again` box and then click `Yes`.

This will re-flash the target (be patient - status is displayed along the bottom of the window). Once downloaded it will break at main and is ready to run. 

![run](/images/ready-to-run.png)

3. Run the application by clicking the `Continue` button (also `F5`). When running the default project will display debug messages in the bottom right-hand window (`Terminal`), e.g.
   
![running](/images/running.png)

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

