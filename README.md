# What is this?
A simple device driver written in python, for old (ca 2005) DeskCNC CNC mills. Works without the original DeskCNC software, can be used to replace it, e.g. on a raspberry pi. It connects to the device on a serial port, and controls the machine. The python script accepts and executes standard G-code commands from a file or standard input. 

<p float="left">
  <img src="https://user-images.githubusercontent.com/44381886/116586671-013dc000-a91a-11eb-895a-786ccd49ba33.jpg" alt="photo5242723118185818911" height="250" />
  <img src="https://user-images.githubusercontent.com/44381886/116586680-0438b080-a91a-11eb-9840-3117a16276d4.jpg" alt="photo5271833315596413812" height="250" /> 
</p>

Objects made with [deskcnc_serial.py](deskcnc_serial.py)

# Features
- Tested on Linux, MacOS and Windows
- Can be used as standalone executable (e.g. run `./deskcnc_serial.py /dev/ttyUSB0 gcode.nc`) or as python module (`import deskcnc_serial`, for example [hofalapp_cncserver](https://github.com/hofa-lab/HoFaLapp/blob/main/hofalapp_cncserver.py) does this)
- Able to execute large G-code files (tested for jobs with more than 40000 commands)
- Supports G-code files for instance generated by [easel.inventables.com](https://easel.inventables.com), Fusion 360, or any other CAD / CNC software
- Automatically reconnects if the connection to the device is lost. So, your job will be continued even if you turn off the machine or disconnect the cable while it is running.
- Supports an interactive mode, i.e. you can type G-code commands directly in your terminal and they are executed instantly.
- Alternatively, accepts G-code commands on stdin with a pipe from any other program. For example, [sensor2gcode.py](sensor2gcode.py) receives JSON data, e.g. from the [SensorStreamer](https://play.google.com/store/apps/details?id=cz.honzamrazek.sensorstreamer) android app and converts it to G-Code commands, so that the machine can be live remote controlled by tilting/rotating the android phone. Execute with e.g. `unbuffer ./sensor2gcode.py [phone-IP] [port] | ./deskcnc_serial.py /dev/ttyUSB0`

# Limitations
- Tested for one device only, for other machines some modifications may be neccessary
- Supports only a basic subset of common standard G-Code commands, many other G-codes are not implemented
- Requires the "firmware.txt" / "firmware_response.txt" hexdump files to initialise communication with the device
- Device settings can not be changed

# Installation and Usage
- Requires Python 3
- Required modules: pip install pyserial numpy pygcode
- You need a serial port or USB-to-serial adapter
- You must know the device name, e.g. `COM7` on Windows or `/dev/ttyUSB1` on Linux
- Usage: `python deskcnc_serial.py serialport [gcode-filename]`

# Usage Hints
- Currently, the starting position (where the tool is when the python script is launched) is always used as origin `(0,0,0)` of the absolute coordinate system and this can not be changed later.
- So, first physically move the tool to whererver you want `(0,0,0)` to be, e.g using G0 commands in interactive mode, then restart deskcnc_serial.py
- This does not turn the milling spindle on automatically, so double-check that there is an M3 command in your G-Code file before executing it.
