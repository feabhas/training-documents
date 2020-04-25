# Testing code using QEMU

The current branch of the QEMU project for the Feabhas target simulation can be found on [github](https://github.com/feabhas/qemu-2).

The project is a work in progress, and the Cortex-M/STM32F407 is not 100% accurate compared to running on the target system. 

## Testing code (no debug) using QEMU
At the project root do:
```
$ scons --qemu
```

## Errors in QEMU
If the qemu process aborts with an error message similar to:
```
qemu: fatal: Trying to execute code outside RAM or ROM at 0x0800018c
```
then you are trying to simulate an image which has been built for the target rather than qemu (i.e. the `--qemu` flag wasn't set when building with `scons`).

### Testing LED simulation with QEMU

To mimic the LEDs on the Feabhas target board (GPIO-D pins 8..11):
1. In one terminal invoke the following script:
```
   $ ./qemu-scripts/run_leds.sh
```
   A monitor window will appear and there will be some debug output, which can be ignore for the moment. Once the programming is running, the state of the LEDs is displayed, e.g.

```
0:D6 On
0:D6 Off
1:D7 On
1:D7 Off
0:D6 On
1:D7 On
0:D6 Off
1:D7 Off
2:D8 On
2:D8 Off
0:D6 On
2:D8 On
```
where D6-D9 map to the board LEDs.

Once `main` exits the `qemu` process will exit and return to the command line.


### Testing the Washing Machine Simulator output using QEMU
To mimic the WMS outputs (GPIO-D pins 8..15):
1. In a terminal invoke the following script:
```
   $ ./qemu-scripts/run_qemu.sh
```
   A monitor window will appear and there will be some debug output, which can be ignore for the moment. Once the programming is running, the state of the WMS outputs is displayed, e.g.
```
   7-Segment: 1
   7-Segment: 3
   7-Segment: 7
   7-Segment: IS OFF
   Motor is ON
   7-Segment: 8
   7-Segment: 0
   Motor is OFF
   Motor is ON
   7-Segment: 1
   Motor is OFF
   Motor is ON
   7-Segment: 0
   7-Segment: 2
   Motor is OFF
   Motor is ON
   7-Segment: 3
   Motor is OFF
   Motor is ON
   7-Segment: 2
   7-Segment: 0
   7-Segment: 4
```

## Simulating USART3 in QEMU
The IO from USART3 of the STM32F407 microcontroller has been mapped on to QEMU Serial Port 0 (`serial_hds[0]`). 
The script `qemu-scripts/run_qemu_serial.sh` maps QEMU Serial Port `0` to `TCP` port `7777` (this can be any port, just a random choice).
Once the shell script has been invoke it is listening on `localhost:7777` for a telnet connect.

To run, in on terminal, invoke the script: 
```
   $ qemu-scripts/run_qemu_serial.sh
```
   The QEMU simulation will now wait until a telnet connection is made on port `7777`.

In a second terminal invoke a telnet session on port `7777`:
```
   $ telnet localhost 7777
   Trying ::1...
   Connected to localhost.
   Escape character is '^]'.

```
The difference between the simulated serial port and the real serial port behaviour is that when using QEMU, input only is passed in when the `return` key is pressed. This means a string will be buffered, and single characters will be appended with `\n`.


## Debugging with QEMU emulation

### To build with the QEMU support
As before, at the project root do:
```
$ scons --qemu
```

### Simple LED simulation with QEMU and GDB

#### Running a simulation
To mimic the LEDs on the Feabhas target board (GPIO-D pins 8..11):
1. In one terminal invoke the following script:
```
   $ ./qemu-scripts/run_leds_gdb.sh
```
   A monitor window will appear and there will be some debug output, which can be ignore for the moment. With the script the qemu simulation will be halted at the first instruction waiting for a GDB connection.

2. In another terminal, run GDB
```
   $ arm-none-eabi-gdb -x gdb-scripts/gdb_command_file
   ...
   ...
   (qemu-arm-gdb)
```
1. All application stdio will appear in the gdb window
2. Type `c` (continue) to run, or `n` for next (step-over), or `s` for step (step-in)
3. If PORT-D pins 8..11 are written to, output will appear in the qemu windows, such as
```
0:D6 On
0:D6 Off
1:D7 On
1:D7 Off
0:D6 On
1:D7 On
0:D6 Off
1:D7 Off
2:D8 On
2:D8 Off
0:D6 On
2:D8 On
```
where D6-D9 map to the board LEDs.

#### Exiting a session
To exit:
1. If the application is running use `<ctrl>+c` in the gdb window to interrupt the executing process
2. Then kill (`k`) the remote qemu process. 
   This will exit the qemu process in the qemu window and return you to the command line
3. Finally `q` will quit gdb

Alternative you can stay in gdb and rerun a session by:
1. reinvoke qemu as before:
```
   $ ./qemu-scripts/run_leds.sh
```
2. rerun the gdb init file
```
    (qemu-arm-gdb)source gdb-scripts/gdb_command_file
```

### Debugging USART3 in QEMU
The IO from USART3 of the STM32F407 microcontroller has been mapped on to QEMU Serial Port 0 (`serial_hds[0]`). 
The script `qemu-scripts/run_gdb_serial.sh` maps QEMU Serial Port `0` to `TCP` port `7777` (this can be any port, just a random choice).
Once the shell script has been invoke it is listening on `localhost:7777` for a telnet connect.

To run:
1. In on terminal, invoke the script: `qemu-scripts/run_gdb_serial.sh` 
2. In another terminal invoke gdb using the script: `arm-none-eabi-gdb -x gdb-scripts/gdb_command_file`
3. In a third terminal invoke a telnet session on port `7777`:
```
$ telnet localhost 7777
Trying ::1...
Connected to localhost.
Escape character is '^]'.

```
The difference between the simulated serial port and the real serial port behaviour is that when using QEMU, input only is passed in when the `return` key is pressed. This means a string will be buffered, and single characters will be appended with `\n`.

## Building exercise soultions

To build any of the exercise solutions run the script:
```
$ ./build-one N [--qemu]
```
where `N` is the exercise number.

**NOTE: this will delete all files in the `/src` directory**