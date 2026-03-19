---
Title: MicroROS Setup
tags:
  - microros
  - esp32
  - platformio
description: Complete Setup guide for micro-ros using ***micro-ros for arduino*** using ***PlatformIO***
icon: u-ros
date: '2025-12-06T15:30:00+05:30'
params:
  width: full
authors:
  - name: koustav
    link: /members/koustav
    image: https://github.com/koustavbetal.png
---

## Prerequisites
1. Install ROS2
2. Install VSCode   

## Install PlatformIO for VSCode:
- Install VSCode, if you haven't already
```sh
sudo snap install code --classic # Assuming you are on Ubuntu
```

- Install PlatformIO extension for VSCode by navigating to:
	extensions (`ctrl+shift+x`) > search "**platformio ide**" > Install 
	OR, 
```sh
code --install-extension platformio.platformio-ide
```

- After Installing the extension,
	Click on the PlatformIO button or clicking the home button on status bar or by searching "**platfomio home**" on command palette (`ctrl+shift+p`)

## Setting up PlatformIO Project
### Resolve Dependencies
- Install the requirement dependencies mentioned [here](https://github.com/micro-ROS/micro_ros_platformio?tab=readme-ov-file#requirements). 
```sh
sudo apt install -y git cmake python3-pip
```

### Create a New Project
- **Open VSCode**
- **Initialise PIO for VSCode (Only for the first time)**
	Click on the PlatformIO button present in the sidebar. That's It!!  
<span style="color:#999;">_For the first time PIO will take its time to download binaries and initialise everything._</span>

- **Navigate to PlatformIo Home** by,
	Clicking on the PlatformIO button or,
	clicking the home button on status bar or,
	by searching "**platfomio home**" on command palette (`ctrl+shift+p`)
- **Click on `Create Project`** 
	- Give a proper name 
	- Select board according to your model (i.e. *DOIT ESP Devkit V1*) 
	- Keep the framework to "*Arduino*"
	- Save it in a recognisable place (uncheck the `locaton` option to turn off default location).
> Note: To open the project later you only have to open this specific older where the `.ini` file is stored. 

### Configure `.ini` file
The {project}.ini file should already be populated with some values:
```ini
; PlatformIO Project Configuration File
; Build options: build flags, source filter
; Upload options: custom upload port, speed and extra flags
; Library options: dependencies, extra library storages
; Advanced options: extra scripting

; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:esp32doit-devkit-v1]
platform = espressif32
board = esp32doit-devkit-v1
framework = arduino
```   
***Please Note:*** This is the configuration for basic ESP32 Wroom Modeule, and most likely will not work for any other models. If you are using ESP32-S3, navigate to [Troubleshooting](/docs/microros/microros-troubleshooting/) section to check if anything helps!   

**Add these lines onto this file:**
```ini
board_microros_transport = serial
board_microros_distro = jazzy
lib_deps =
	https://github.com/micro-ROS/micro_ros_platformio
```
*`board_microros_*`* are optional and should be changed based on your preferred value (all possible values can be found [here](https://github.com/micro-ROS/micro_ros_platformio?tab=readme-ov-file#library-configuration).)
> Upon saving this file pltformIO will download all the pkgs and will build everything in the background, Don't panic and Do Not close the Application.


**Basic setup for the project is done,** Now do the code in main.cpp under /src folder.  


## Resources
<div style="display:flex; align-items:center; gap:30px;">

  <!-- Video (fixed 30% width of screen) -->
  <div style="width:30%;  margin-top:12px; border-radius:15px; overflow:hidden;">
    <iframe
      src="https://www.youtube.com/embed/Nf7HP9y6Ovo?feature=oembed"
      style="aspect-ratio:1.77; width:100%; height:auto; border:0;"
      allowfullscreen
    ></iframe>
  </div>

  <!-- Text (flexible remaining width) -->
  <div style="flex:1; font-size:16px; line-height:1.5;">
    This video covers all I have mentioned in this blog including,</br>  
    <em>1. Configuring PIO </br>
    2. MicroROS Agent setup in main machine. </br>
    3. And basic understanding of microROS codes.</em>

  </div>

</div>

