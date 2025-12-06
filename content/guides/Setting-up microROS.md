---
Title: MicroROS Setup
tags:
  - microros
  - esp32
  - platformio
description: Complete Setup guide for micro-ros using ***micro-ros for arduino*** using ***PlatformIO***
icon: u-ros
date: '2025-12-06T15:30:00+05:30'

authors:
  - name: koustav
    link: /members/koustav
    image: https://github.com/koustavbetal.png
---
<svg xmlns="http://www.w3.org/2000/svg" version="1.0" viewBox="-120 0 1280 300">
<g transform="scale(0.6) translate(100,0 )" fill="#FFFFFF" stroke-width="0">
<path d="m133.9 51.9-2.9 2.9V95h30V76c0-26.1-.5-27-15.5-27-8.2 0-8.8.1-11.6 2.9m68.5-1.1c-4 3.2-4.4 5.4-4.4 25.1V95h30V54.8l-2.9-2.9c-2.8-2.8-3.3-2.9-11.8-2.9-6.8 0-9.3.4-10.9 1.8m66.5 1.1-2.9 2.9V95h30V76c0-26.1-.5-27-15.5-27-8.2 0-8.8.1-11.6 2.9M827 68.6c-35.7 4.1-61.1 17.8-81.5 43.9-15.6 20-25.1 42.9-31 75-2.3 12-3.1 47.7-1.6 62.7 8.4 80.3 56 130.9 123.1 130.8 58-.1 102.2-37.9 118.4-101.1 5.4-21 6.9-36.4 6.3-61.9-.6-23.1-1.9-33.6-6.7-51.5-13.7-51.9-46.9-86.3-91.8-95.5-9.5-1.9-28.3-3.2-35.2-2.4m22.8 46c27 5.4 49 27.6 59.5 59.9 11.7 36 9.5 83.7-5.3 115.2-14.1 30.2-37.4 46.3-67 46.3-50.1 0-83.8-49.9-80.7-119.5 1.5-32.5 10.4-59.8 25.3-77.9 16.5-20.1 42.5-29.2 68.2-24M1099.7 69c-33.3 3.8-60.9 21.3-73.2 46.3-6.2 12.7-7.6 18.8-8.2 35.2-1.1 31.4 8 51.3 30.3 66.1 13.1 8.7 27.8 14.7 66.9 27.4 25.1 8.1 41.4 16 49.2 23.7 3.3 3.2 6.7 7.5 7.7 9.6 2.7 5.9 3.7 14.1 2.6 22-2.7 18.7-13.7 29.9-34.3 34.7-34.5 8.1-68.5-1.6-96.3-27.3l-6-5.6-15.8 15.8-15.7 15.7 10.3 10c25.6 24.8 53.8 36.7 89.8 38.1 50.6 1.8 87.1-14.9 103.4-47.4 6.9-13.7 10.6-36.9 8.7-54.2-2.2-20-7.8-32-21-45.1-14.7-14.5-29.6-21.9-72.6-36.2-40.2-13.3-54.3-20.9-60.7-33-2-3.7-2.3-5.8-2.3-14.3 0-8.9.3-10.6 2.8-15.6 4.8-10 15-17.3 27.9-20.4 8.5-2 28.7-1.9 38.5.1 15.8 3.2 29 10 41.2 21l6.4 5.8 15.3-15.9 15.3-16-3.2-3.4c-3.8-4-17.4-14.7-23.6-18.6-12.9-8.1-29.6-14.5-43.8-17-10.8-1.9-29.8-2.6-39.6-1.5M435 224.5V376h43V256h33.3l33.2.1 12.7 26.2c6.9 14.4 20 41.4 28.9 60l16.3 33.7H653l-3.6-7.1c-2-4-5.5-11.3-7.9-16.3-2.3-5-14-29.3-25.9-53.9C603.7 274 594 253.5 594 253s3-1.8 6.8-3.1c28.9-9.5 48.9-32.3 54.5-62.4 7.6-40-3.3-75.7-28.8-95.2-12.8-9.7-29-16-47-18.2-5.7-.7-33.5-1.1-76.7-1.1H435zm144.3-106.9c16.3 4.2 27.8 15.1 32.2 30.6 2.1 7.4 2.4 23.6.6 31.2-3.9 16.4-15.1 27.6-32.2 32.3-5.7 1.6-11.6 1.8-53.9 1.8h-47.5l-.3-48.8-.2-48.7h47.6c40.1 0 48.6.2 53.7 1.6m-451.5-6c-6.3 1.9-14.8 9.8-17.9 16.6l-2.4 5.3V216c0 68 .2 83.2 1.4 86.5 2 5.8 6.7 12.2 11.4 15.6 8.4 6.1 6.4 6 96.3 5.7l81.9-.3 5-2.7c6-3.2 11.4-8.7 14.6-14.8l2.4-4.5v-169l-2.2-4.7c-3.1-6.8-9.9-13.1-16.7-15.7-5.6-2.1-6.4-2.1-87.4-2-68.1 0-82.5.3-86.4 1.5M272 153.2c1.8 1.3 4.3 3.6 5.4 5.1 2.1 2.8 2.1 3.8 2.4 57.2.3 60.5.4 58.9-6.4 64.1l-3.7 2.9-53.6.3c-60.9.3-59.9.4-64.9-7.1l-2.7-4.1v-54.5c0-53.3 0-54.7 2.1-58.1 1.1-1.9 3.5-4.5 5.4-5.7l3.3-2.3h109.4z"/><path d="M183 221.5V265h12v-12c0-6.6.4-12 .8-12 .5 0 1.7 1 2.8 2.1 2.7 3.1 9.2 5.9 13.7 5.9 4.9 0 10.4-2.3 14.4-6.2l3.2-3.1.9 2.4c2 5.2 9.1 8.2 15.6 6.5 2.3-.6 2.6-1.1 2.6-5.1 0-4.1-.2-4.5-2.4-4.5-5.5 0-5.6-.6-5.6-32.1V178h-11.9l-.3 24.7c-.3 22.7-.5 25.1-2.3 28.2-3.5 5.9-6.7 7.6-14.5 7.6-8.2 0-11.3-1.8-14.7-8.5-2.2-4.2-2.3-5.5-2.3-28.2V178h-12zM53.3 135.7c-1.2.2-3.4 1.8-4.8 3.4-2.2 2.7-2.5 3.9-2.5 11.5 0 7.8.2 8.6 2.9 11.6l2.9 3.3 20.1.3 20.1.3V135l-18.2.1c-10.1.1-19.3.4-20.5.6m283.4 0c-.4.3-.7 7.3-.7 15.5V166h18.5c26.4 0 27.5-.6 27.5-15.7 0-10.3-1.1-12.7-6.8-14.3-4-1.1-37.5-1.4-38.5-.3M51.1 204.4c-3.9 2.1-5 5.4-5.1 14.4 0 7.9.2 8.5 2.9 11.3l2.9 2.9H92v-30H72.8c-14.1.1-19.9.4-21.7 1.4M336 218v15h39.8l3.1-2.6c3-2.5 3.1-2.8 3.1-11.6 0-15.3-.8-15.8-27-15.8h-19zM49.3 273.4c-2.5 2.2-2.8 3.1-3.1 10.5-.8 16.1.7 17.1 26.8 17.1h19v-30H72.1c-19.7 0-20 0-22.8 2.4M336 286v15h40.2l2.9-2.9c2.8-2.8 2.9-3.3 2.9-12.4 0-9.4 0-9.6-3.1-12.1l-3.1-2.6H336zm-205 72.9c0 19.7 0 20 2.4 22.8 2.3 2.6 3 2.8 11.4 3.1 8.8.4 9 .3 12.3-2.6l3.4-3 .3-20.1.3-20.1H131zm67.2.2.3 20.1 3.3 2.9c3 2.7 3.7 2.9 11.8 2.9s8.7-.1 11.5-2.9l2.9-2.9V339h-30.1zm67.8-.2c0 19.7 0 20 2.4 22.8 2.2 2.5 3.1 2.8 10.5 3.1 16.1.8 17.1-.7 17.1-26.8v-19h-30z"/><g></svg>
In this guide we will setup complete micro-ros setup from installing PlatformIO on VSCode and then installing the agent onto the main machine installed with ROS2 Jazzy.

## Why PlatformIO? 
1. Easy to setup
2. Mostly automated and takes care all the compiling and uploading part all by itself.

## Prerequisites
- Basic idea of micro-controllers and ROS(2).
- Some hardware related experience for connecting IO pins properly.
- Basics of electronics, voltage and current requirements for used components.

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
(To Understand complete file structure of a PIO project look for the MicroROS 101 in guides.[TODO])

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

