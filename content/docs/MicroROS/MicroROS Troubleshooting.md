---
Title: MicroROS Troubleshooting
tags:
  - microros
  - ros2
  - platformio
description: Answer to all possible troubles that we have witnessed and how we overcame.
# icon: pio
date: '2025-12-04T13:00:00+05:30'
params:
  width: full
authors:
  - name: koustav
    link: /members/koustav
    image: https://github.com/koustavbetal.png

---
## PIO configuration (.ini) for ESP32-S3 
Tested with [ESP32-S3-N16R8](https://github.com/microrobotics/ESP32-S3-N16R8/blob/main/ESP32-S3-N16R8_User_Guide.pdf) board, which has 2 USB ports. (USB & COM)
```ini
[env:esp32-s3-devkitc-1]
platform = espressif32
board = esp32-s3-devkitc-1

build_flags =
    ; -DBOARD_HAS_PSRAM
    -DARDUINO_USB_MODE=1
    -DARDUINO_USB_CDC_ON_BOOT=1 ; This line is important for boards with 2 USBs

framework = arduino
monitor_speed = 115200

board_microros_transport = serial
board_microros_distro = jazzy

lib_deps =
    https://github.com/micro-ROS/micro_ros_platformio
```
##### Important: Use `USB` port to upload code, and `COM` for connecting with ROS2