# Custom RC transmitter with touchscreen - Arduino/ESP32
![Alt text](/images/rc_pcb_1.jpeg)
![Alt text](/images/rc_pcb_perspectives.png)
![Alt text](/images/rc_pcb_2.jpg)
![Alt text](/images/rc_pcb_3.jpg)

## Project overview
Custom remote controller created from scratch using a manufactured double-layer PCB, some 3D printed parts, and acrylic plates. The RC has an ESP32S3 board that manages the touchscreen, and an Arduino Nano board that is responsible for reading the RC channel values, sending/receiving the radio signal and measuring battery levels.

This RC was developed as part of the [OpenRover project](https://github.com/pol-valero/openrover-robotic-platform), where I built a 3D printed Mars rover replica robot. Since the rover had many functionalities and status data available, I wanted to create a custom RC with a touchscreen that could be easily expanded with new control and monitoring features without requiring additional hardware (e.g., switches, push buttons, LEDs...). 

The remote controller’s software and UI screens can be easily adapted to control and monitor other kinds of robots, making this project a good starting point for building your own RC tailored to your needs. The RC uses a 2.4GHz NRF24L01 transceiver module for communications, which is very versatile. 

The [OpenRover project](https://github.com/pol-valero/openrover-robotic-platform) repository already contains all the information, software, and design files related to the RC. However, this dedicated ```radio-controller-pcb``` repository focuses exclusively on the RC, and puts all relevant resources in one centralized place.

### Features
- Able to continuously transmit 8 channels: 2 channels (X, Y axis) for each joystick, 3 AUX channels for the physical toggle switches and the push button, 1 free AUX channel
- Bidirectional communication with the robot: status data and commands can be sent to the robot, or received from the robot 
- Three screens available: monitor, control (home), configuration. To change between screens, you can swipe right/left or click on the bottom navigation bar
- Control screen allows 4 main operation modes: conventional driving control, 360º turn control, robotic arm control, head control. Digital toggle switches activate secondary functionalities
- Monitor screen displays current RC channel values, environmental data, and status information. A line graph shows the evolution of environmental data over the last minute
- Touchscreen plugs into the PCB using a JST connector, making it easily detachable. The RC transmitter can work without the touchscreen
- Displays the battery voltage levels of the RC and the robot. Makes a buzzer sound if RC battery level is low. Around 1h 30min of RC battery life
- About 100 meters range outdoors (clear line of sight). A signal bar icon on the screens will start flashing if the connection to the robot is lost

### Tech stack
To program Arduino and ESP32 microcontrollers, the PlatformIO IDE with the Arduino Framework was used. To create the embedded UI design of the touchscreen, the SquareLine Studio platform and LVGL library were used.

![Alt text](/images/tech_stack_icons.png)

### Remarks
This repository contains the code and documentation of all the parts of the remote controller. However, the codebase is actually distributed across different repositories:

//TODO

"Git subtrees" were used to link these independent repositories to folders inside this central ```radio-controller-pcb``` repository.


## Technical implementation details

### Code setup guide
The remote control is formed by two blocks, the "Arduino Nano" and the "ESP32S3 touchscreen" blocks. To develop the code for these blocks, the PlatformIO IDE was used. This IDE is integrated into the versatile Visual Studio Code editor, and can be installed simply by searching "PlatformIO IDE" in the "Extensions" tab of Visual Studio Code. 

Thanks to PlatformIO, all the code, configuration files, and dependencies are in a single package. This makes it very easy to share the complete project, enabling other people to execute it right after they download it. 

Each of the two blocks is a standalone PlatformIO project. Therefore, just by opening the [Arduino Nano](arduino_nano_code/) and [ESP32S3](esp32s3_code/) projects with PlatformIO, connecting the ESP32S3 or Arduino Nano development board via USB, and clicking the "upload" button, the code will be uploaded to the ESP32S3 or Arduino Nano and will start executing. 

### Embedded UI design
To create the embedded UI design of the touchscreen, the SquareLine Studio platform and LVGL library were used. To edit the UI, these steps have to be followed:

- Step 1: Open the [SquareLine Studio project](touchscreen_ui_design_squareline/) and make the desired visual changes

- Step 2: Click on Export -> Export UI Files. The UI files export path must be previously specified in the "Project Settings"

- Step 3: Copy all the exported UI Files into the "squareLineFiles" folder of the [ESP32S3 PlatformIO project](esp32s3_code/), making sure to overwrite the previous files that were in this folder

- Step 4: Click the "upload" button in PlatformIO to upload the code to the ESP32S3. The modified UI should now appear on the touchscreen. 

Three screens have been implemented in the embedded UI: Control, Monitor and Configuration.
![Alt text](/images/embedded_UI_designs.png)

### Hardware and schematics
The RC has been built around a custom double-layer PCB. The [PCB design](remote_control_pcb_design_easyeda.json) has been created using the EasyEDA app, and the PCB has been manufactured by JLCPCB. 

The RC hardware includes an ESP32S3 development board that is responsible for managing the screen and an Arduino Nano board that reads the analog or digital signals from the joysticks/switches/buttons, reads the battery voltage, and controls the radio transceiver. 

The touchscreen model is the ESP32S3_8048S043, with a resolution of 800 x 480 px and 4.3" screen size. This model has an ESP32S3 and I/O connectors built in.

This is the [schematic](esp32s3_touchscreen_schematic-pcb_remote.pdf) of the "ESP32S3 touchscreen" block. The [EasyEDA PCB design](remote_control_pcb_design_easyeda.json) can be seen as the schematic of the "Arduino Nano" block. 

Here are a couple images of the top and bottom layers of the EasyEDA PCB design:
![Alt text](/images/rc_pcb_design_front.png)
![Alt text](/images/rc_pcb_design_back.png)

#### PCB manufacturing
To manufacture the PCB, a [Gerber fabrication file](remote_control_pcb_gerber.zip) has to be generated from the design and then has to be uploaded to the manufacturer's website (e.g., JLCPCB, PCBWAY...). If you want to make any modifications to the EasyEDA PCB design, you will have to regenerate the Gerber by going into the top menu and selecting "Fabrication -> PCB Fabrication File (Gerber) -> Generate Gerber".


### 2D and 3D designs
This [Fusion360 design file](2d-3d_designs/rc-pcb-spacers-and-touchscreen-case.f3d) contains the 3D printable parts: the spacers that hold the acrylic plates and PCB together, the structure that holds the touchscreen, and the case for the ESP32S3-8048 touchscreen. This case is a modified version of a [case design](https://www.thingiverse.com/thing:6517481) found on Thingiverse.

![Alt text](/images/rc_pcb_spacers_and_case.png)

Other parts of the remote control that have to be manufactured are the top and bottom acrylic plates, which should be cut using a laser or CNC machine. In my case, I used a workshop near my home that had a laser machine, but there are also online websites where you can order laser cut designs in acrylic. These are the 2D designs for the [top](2d-3d_designs/rc_top_plate.dxf) and [bottom](2d-3d_designs/rc_bottom_plate.dxf) acrylic plates. I would recommend using 3mm clear acrylic. 

## Getting started steps

//TODO: Order from components_list.txt. Follow Technical implementation details...
//Maybe put getting started just over "technical implementation details" section 

## Demos

//Put RC PCB GIF. Crop existing touchscreen gifs (so only touchscreen is seen?)