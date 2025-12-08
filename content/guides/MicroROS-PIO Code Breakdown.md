---
Title: MicroROS (PIO) Code Breakdown
tags:
  - microros
  - ros2
  - platformio
description: Easiest Boiler Plate code for Creating fullfledged Node Structure for Seamless Communication.
icon: pio
date: '2025-12-08T13:00:00+05:30'

authors:
  - name: koustav
    link: /members/koustav
    image: https://github.com/koustavbetal.png
---
Micro-ros has several ways to program them and build them, easiest way being the platformIO compilation with its standard CMake file structure way of handling projects.

## Prerequisites
Follow [**How to Setup for Micro-ROS on PlatformIO**](https://koustavbetal.github.io/guides/setting-up-microros/) Guide First.

## Basics Components of any Micro-ROS code
> **Micro-ROS code is nothing but a simple Arduino code sprinkled with micro-ros codes, 
> The following sections describes all the required and optional code we have to add to work it properly.**
### Initialise Transportation 
```c++
set_microros_transports(); // Serial/Wifi/*
```
*Or do your custom transport config (not recommended for beginners)*

### Allocator Setup
Allocator manages memory. It assigns and frees memory according to the needs.
```c++
rcl_allocator_t allocator = rcl_get_default_allocator();
```

### Initialise Micro-ROS Support
It prepares the micro-ROS layer on ESP32.
```c++
rclc_support_t support;
rclc_support_init(&support, 0, NULL, &allocator);
```

### Create Nodes
The micro-ROS node that hosts pubs/subs/services.
```c++
rcl_node_t node;
rclc_node_init_default(&node, "esp32_ultrasonic_node", "", &support);
```

### Create Topics/Services
This is where you define your interface.
#### Create Publisher
```c++
rcl_publisher_t publisher;
rclc_publisher_init_default(
    &publisher,
    &node,
    ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Int32), // Or Your custom msgs
    "distance_data"
);
```

#### Create Subscriber
```c++
rcl_subscription_t subscriber;
rclc_subscription_init_default(
    &subscriber,
    &node,
    ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Bool), // Or Your custom msgs
    "enable_sensors"
);
```

#### Create Timer (Optional)
```c++
rcl_timer_t timer;
rclc_timer_init_default(
    &timer,
    &support,
    RCL_MS_TO_NS(100), // See the code example below for possible values
    timer_callback
);
```

## Let's Construct the code!!
This code is inspired from [rasheeddo](https://github.com/rasheeddo/pubsub_many/tree/master) on GitHub.
***Anything is written under TODO: can and should be updated! 
Else that function is required for the ros-agent***
### Let's Include all Necessary Headers/libraries
```c++
/////////////////////
/// For Micro ROS ///
/////////////////////
#include <Arduino.h>
#include <micro_ros_platformio.h>
#include <stdio.h>
#include <rcl/rcl.h>
#include <rcl/error_handling.h>
#include <rclc/rclc.h>
#include <rclc/executor.h>
#include <rmw_microros/rmw_microros.h>

/*
 * TODO : include your desired msg header file
*/
#include <std_msgs/msg/bool.h>
#include <std_msgs/msg/float32.h>
```

### Declare all the Necessary Objects
#### Declare rcl objects
```cpp
/*
 * Declare rcl object 
*/
rclc_support_t support;
rcl_init_options_t init_options;
rcl_node_t micros_node;
rcl_timer_t timer;
rclc_executor_t executor;
rcl_allocator_t allocator;
```

#### Declare all the publishers, subscribers & Services object
```cpp
/*
 * TODO : Declare your 
 * publisher & subscription objects below
*/
rcl_publisher_t act_pub;

rcl_subscription_t act_sub;
```

#### Create msg objects
```cpp
/*
 * TODO : Define your necessary Msg
 * that you want to work with below.
*/
std_msgs__msg__Bool act_msg;
std_msgs__msg__Int32 int32_msg;
```

### Create Entities
#### Init Configurations
```cpp
/*
	 TODO : Define your
	 - ROS node name
	 - namespace
	 - ROS_DOMAIN_ID
*/
const char * node_name = "esp32_uros_node";
const char * ns = "";
const int domain_id = 0;
```

#### Initialise rcl objects
```cpp
/*
 * Initialize node
 */
allocator = rcl_get_default_allocator();
init_options = rcl_get_zero_initialized_init_options();
rcl_ret_t ret = RCL_RET_OK;
ret = rcl_init_options_init(&init_options, allocator);
ret = rcl_init_options_set_domain_id(&init_options, domain_id);
rclc_support_init_with_options(&support, 0, NULL, &init_options, &allocator);
rclc_node_init_default(&micros_node, node_name, ns, &support);
```

#### Initialise Topics
```cpp
/*
 * TODO : Init your publisher and subscriber 
 */
/* ----------------- */
/* --- Publisher --- */ 
/* ----------------- */
rclc_publisher_init_best_effort(
	&act_pub,
	&micros_node,
	ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Bool),
	"/act_state"); // TODO: Publishing Topic Name

/* -------------------- */
/* --- Subscription --- */ 
/* -------------------- */
rclc_subscription_init_best_effort(
	&act_sub,
	&micros_node,
	ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Bool),
	"/move_act"); // TODO: Subscribing Topic Name
```
### Start the Executor
```cpp
/*
 * Init Executor
 * TODO : make sure the num_handles is correct
 * num_handles = total_of_subscriber + timer
 * publisher is not counted
 * 
 * TODO : make sure the name of sub msg and callback are correct
 */
unsigned int num_handles = 2; //As We Only have 1 Sub + Timer
executor = rclc_executor_get_zero_initialized_executor();
rclc_executor_init(&executor, &support.context, num_handles, &allocator);
rclc_executor_add_subscription(&executor, &act_sub, &act_msg, &act_sub_callback, ON_NEW_DATA);
rclc_executor_add_timer(&executor, &timer);
```
***But as we know all the Subscribers needs a callback func(); 
We need to create those too!***

#### Create Subscription Callback
```cpp
/*
 * TODO : Define your subscription callbacks here
*/
void act_sub_callback(const void *msgin) {
	std_msgs__msg__Bool * act1_msg = (std_msgs__msg__Bool *)msgin; // (required)

	act_state = act1_msg->data; // points to bool.data (required)
	
	if (act1_msg->data == true) {
        extendActuator();  
    } else {
        retractActuator();  
    }
}
```

#### Create Timer for Publisher
**This is tied to hardware timer, which will call the publishers in fixed time interval**
##### Initialise Timer
```cpp
/*
 * Init timer_callback
 * TODO : change timer_timeout
 * 50ms : 20Hz
 * 20ms : 50Hz
 * 10ms : 100Hz
 */
const unsigned int timer_timeout = 50;
rclc_timer_init_default2(&timer,&support, RCL_MS_TO_NS(timer_timeout), timer_callback, true); // timer_callback is a function that will be called in every defined interval, so we have to put all the publisher inside this function.  
```
##### Create timer_callback()
```cpp
void timer_callback(rcl_timer_t * timer, int64_t last_call_time)
{
	(void) last_call_time;
	if (timer != NULL) {

		/*
			 TODO : Publish anything inside here
			 
			 For example, we are going to echo back
			 the int16array_sub data to int16array_pub data,
			 so we could see the data reflect each other.

			 And also keep incrementing the int16_pub
		*/

		rcl_ret_t ret = RCL_RET_OK;

		/// Switch state ///
		act_msg.data  = act_state;
		ret = rcl_publish(&act_pub, &act_msg, NULL);	 // calling the publisher/publishing from here
	}
}
```
***Our bare bone Micro-ROS code is ready to deploy, although there are some precautions we have to take for reusing the hardware next time without any issues.***

### Destroy All Created Entities / Destructor
```cpp
	rmw_context_t * rmw_context = rcl_context_get_rmw_context(&support.context);
	(void) rmw_uros_set_context_entity_destroy_session_timeout(rmw_context, 0);

	rcl_ret_t ret = RCL_RET_OK;

	ret = rcl_timer_fini(&timer);
	ret = rclc_executor_fini(&executor);
	ret = rcl_init_options_fini(&init_options);
	ret = rcl_node_fini(&micros_node);
	rclc_support_fini(&support);
	/*
	 * TODO : Make sue the name of publisher and subscriber are correct
	 */
	ret = rcl_publisher_fini(&act_pub, &micros_node);

	ret = rcl_subscription_fini(&act_sub, &micros_node);
```

### Optional Helper Functions
**rcl() has some status codes, based on that we can essentially manage the ros-agent as well as the ESP32**
#### Defining the Status States
```cpp
/*
 * Helper functions to help reconnect
*/
#define EXECUTE_EVERY_N_MS(MS, X)  do { \
		static volatile int64_t init = -1; \
		if (init == -1) { init = uxr_millis();} \
		if (uxr_millis() - init > MS) { X; init = uxr_millis();} \
	} while (0)

enum states {
	WAITING_AGENT,
	AGENT_AVAILABLE,
	AGENT_CONNECTED,
	AGENT_DISCONNECTED
} state;
```
#### Managing the Agent
**As we constantly have to monitor to manage the agent and the board we can add this to Arduino's builtin `loop()` function**
```cpp
void loop() {
	/*
	 * Try ping the micro-ros-agent (HOST PC), then switch the state 
	 * from the example
	 * https://github.com/micro-ROS/micro_ros_arduino/blob/galactic/examples/micro-ros_reconnection_example/micro-ros_reconnection_example.ino
	 * 
	 */
	switch (state) {
		case WAITING_AGENT:
			EXECUTE_EVERY_N_MS(200, state = (RMW_RET_OK == rmw_uros_ping_agent(100, 1)) ? AGENT_AVAILABLE : WAITING_AGENT;);
			break;
		case AGENT_AVAILABLE:
			state = (true == create_entities()) ? AGENT_CONNECTED : WAITING_AGENT;
			if (state == WAITING_AGENT) {
				destroy_entities();
			};
			break;
		case AGENT_CONNECTED:
			// if number of MS is too low less than 1000, it will ping agent too many times
			// and caused reconnect too many times
			EXECUTE_EVERY_N_MS(1000, state = (RMW_RET_OK == rmw_uros_ping_agent(100, 5)) ? AGENT_CONNECTED : AGENT_DISCONNECTED;);
			if (state == AGENT_CONNECTED) {
				rclc_executor_spin_some(&executor, RCL_MS_TO_NS(100));
			}
			break;
		case AGENT_DISCONNECTED:
			destroy_entities();
			state = WAITING_AGENT;
			break;
		default:
			break;
	}

	/*
	 * TODO : 
	 * Do anything else you want to do here,
	 * like read sensor data,  
	 * calculate something, etc.
	 */
```
## Stitching Everything Together!!
```c++
/*
	* This is a simple template to use microros with ESP32-Arduino
	* This sketch has sample of publisher to publish from timer_callback
	* and subscrption to listen of new data from other ROS node 
	* to control 2x linear actuator conneted with ESP32 through a Motor-Driver (smartelex-13D)
	* 
	* Some of the codes below are gathered from github and forums
	* 
	* Modified by Koustav Betal
	* 
*/


/*
 * TODO : Include your necessary header here
*/


/////////////////////
/// For Micro ROS ///
/////////////////////
#include <Arduino.h>
#include <micro_ros_platformio.h>
#include <stdio.h>
#include <rcl/rcl.h>
#include <rcl/error_handling.h>
#include <rclc/rclc.h>
#include <rclc/executor.h>
#include <rmw_microros/rmw_microros.h>

/*
 * TODO : include your desired msg header file
*/
#include <std_msgs/msg/bool.h>

/*
 * Optional, But Good Practice
 * Define pin for future IO operation 
 * between hardware and ESP32 through micro-ros-agent
*/

#define act1_PIN 21
#define act2_PIN 19
#define pwm1_PIN 4
#define pwm2_PIN 5
#define led_PIN 2  // Built-in LED pin


bool act_state = false;

unsigned long actionStart = 0;
unsigned long actionDuration = 16000;  // 16 seconds
bool actuatorRunning = false;


/*
 * Helper functions to help reconnect
*/
#define EXECUTE_EVERY_N_MS(MS, X)  do { \
		static volatile int64_t init = -1; \
		if (init == -1) { init = uxr_millis();} \
		if (uxr_millis() - init > MS) { X; init = uxr_millis();} \
	} while (0)

enum states {
	WAITING_AGENT,
	AGENT_AVAILABLE,
	AGENT_CONNECTED,
	AGENT_DISCONNECTED
} state;

enum Motion { STOPPED, EXTENDING, RETRACTING }currentMotion;


void extendActuator();
void retractActuator();
void stopActuator();

/*
 * Declare rcl object
*/
rclc_support_t support;
rcl_init_options_t init_options;
rcl_node_t micros_node;
rcl_timer_t timer;
rclc_executor_t executor;
rcl_allocator_t allocator;

/*
 * TODO : Declare your 
 * publisher & subscription objects below
*/
rcl_publisher_t act_pub;

rcl_subscription_t act_sub;

/*
 * TODO : Define your necessary Msg
 * that you want to work with below.
*/
std_msgs__msg__Bool act_msg;


/*
 * TODO : Define your subscription callbacks here
 * leave the last one as timer_callback()
*/
void act_sub_callback(const void *msgin) {
	std_msgs__msg__Bool * act1_msg = (std_msgs__msg__Bool *)msgin;

	act_state = act1_msg->data;
	
	if (act1_msg->data == true) {
        extendActuator();  
    } else {
        retractActuator();  
    }
}


void timer_callback(rcl_timer_t * timer, int64_t last_call_time)
{
	(void) last_call_time;
	if (timer != NULL) {

		/*
			 TODO : Publish anything inside here
			 
			 For example, we are going to echo back
			 the int16array_sub data to int16array_pub data,
			 so we could see the data reflect each other.

			 And also keep incrementing the int16_pub
		*/

		rcl_ret_t ret = RCL_RET_OK;


		/// Switch state ///
		act_msg.data  = act_state;
		ret = rcl_publish(&act_pub, &act_msg, NULL);

		
	}
}

/*
	 Create object (Initialization)
*/

void extendActuator() {
    digitalWrite(act1_PIN, LOW);   
    digitalWrite(act2_PIN, LOW);   
    digitalWrite(pwm1_PIN, HIGH);
    digitalWrite(pwm2_PIN, HIGH);

    actuatorRunning = true;
    currentMotion = EXTENDING;
    actionStart = millis();

    // Serial.println("Actuator 1 EXTENDING");
}

void retractActuator() {
    digitalWrite(act1_PIN, HIGH);   
    digitalWrite(act2_PIN, HIGH);   
    digitalWrite(pwm1_PIN, HIGH);
    digitalWrite(pwm2_PIN, HIGH);

    actuatorRunning = true;
    currentMotion = RETRACTING;
    actionStart = millis();

    // Serial.println("Actuator 1 RETRACTING");
}

void stopActuator() {
  digitalWrite(pwm1_PIN, 0);
  digitalWrite(pwm2_PIN, 0);

  actuatorRunning = false;
  currentMotion = STOPPED;

//   Serial.println("STOPPED.");
}


bool create_entities()
{
	/*
		 TODO : Define your
		 - ROS node name
		 - namespace
		 - ROS_DOMAIN_ID
	*/
	const char * node_name = "esp32_uros_node";
	const char * ns = "";
	const int domain_id = 0;
	
	/*
	 * Initialize node
	 */
	allocator = rcl_get_default_allocator();
	init_options = rcl_get_zero_initialized_init_options();
	rcl_ret_t ret = RCL_RET_OK;
	ret = rcl_init_options_init(&init_options, allocator);
	ret = rcl_init_options_set_domain_id(&init_options, domain_id);
	rclc_support_init_with_options(&support, 0, NULL, &init_options, &allocator);
	rclc_node_init_default(&micros_node, node_name, ns, &support);
	
	/*
	 * TODO : Init your publisher and subscriber 
	 */
	/* ----------------- */
	/* --- Publisher --- */ 
	/* ----------------- */
	rclc_publisher_init_best_effort(
        &act_pub,
        &micros_node,
        ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Bool),
        "/act_state");

	/* -------------------- */
	/* --- Subscription --- */ 
	/* -------------------- */
	rclc_subscription_init_best_effort(
		&act_sub,
		&micros_node,
		ROSIDL_GET_MSG_TYPE_SUPPORT(std_msgs, msg, Bool),
		"/move_act");



	/*
	 * Init timer_callback
	 * TODO : change timer_timeout
	 * 50ms : 20Hz
	 * 20ms : 50Hz
	 * 10ms : 100Hz
	 */
	const unsigned int timer_timeout = 50;
	rclc_timer_init_default2(&timer,&support, RCL_MS_TO_NS(timer_timeout), timer_callback, true);

	/*
	 * Init Executor
	 * TODO : make sure the num_handles is correct
	 * num_handles = total_of_subscriber + timer
	 * publisher is not counted
	 * 
	 * TODO : make sure the name of sub msg and callback are correct
	 */
	unsigned int num_handles = 2;
	executor = rclc_executor_get_zero_initialized_executor();
	rclc_executor_init(&executor, &support.context, num_handles, &allocator);
	rclc_executor_add_subscription(&executor, &act_sub, &act_msg, &act_sub_callback, ON_NEW_DATA);
	rclc_executor_add_timer(&executor, &timer);

	return true;
}
/*
 * Clean up all the created objects
 */
void destroy_entities()
{
	rmw_context_t * rmw_context = rcl_context_get_rmw_context(&support.context);
	(void) rmw_uros_set_context_entity_destroy_session_timeout(rmw_context, 0);

	rcl_ret_t ret = RCL_RET_OK;

	ret = rcl_timer_fini(&timer);
	ret = rclc_executor_fini(&executor);
	ret = rcl_init_options_fini(&init_options);
	ret = rcl_node_fini(&micros_node);
	rclc_support_fini(&support);
	/*
	 * TODO : Make sue the name of publisher and subscriber are correct
	 */
	ret = rcl_publisher_fini(&act_pub, &micros_node);

	ret = rcl_subscription_fini(&act_sub, &micros_node);
}


void setup() {

	Serial.begin(115200);
	/*
	 * TODO : select either of USB or WiFi 
	 * comment the one that not use
	 */
	// set_microros_transports();
	//set_microros_wifi_transports("WIFI-SSID", "WIFI-PW", "HOST_IP", 8888);
	set_microros_serial_transports(Serial);

	/*
	 * Optional, setup output pin for LEDs
	 */
	pinMode(act1_PIN, OUTPUT);
	pinMode(act2_PIN, OUTPUT);
	pinMode(pwm1_PIN, OUTPUT);
	pinMode(pwm2_PIN, OUTPUT);
	pinMode(led_PIN, OUTPUT);



	/*
	 * TODO : Initialze the message data variable
	*/

	act_msg.data = false;

	/*
	 * Setup first state
	 */
	state = WAITING_AGENT;
	currentMotion = STOPPED;


}

void loop() {
	/*
	 * Try ping the micro-ros-agent (HOST PC), then switch the state 
	 * from the example
	 * https://github.com/micro-ROS/micro_ros_arduino/blob/galactic/examples/micro-ros_reconnection_example/micro-ros_reconnection_example.ino
	 * 
	 */
	switch (state) {
		case WAITING_AGENT:
			EXECUTE_EVERY_N_MS(200, state = (RMW_RET_OK == rmw_uros_ping_agent(100, 1)) ? AGENT_AVAILABLE : WAITING_AGENT;);
			break;
		case AGENT_AVAILABLE:
			state = (true == create_entities()) ? AGENT_CONNECTED : WAITING_AGENT;
			if (state == WAITING_AGENT) {
				destroy_entities();
			};
			break;
		case AGENT_CONNECTED:
			// if number of MS is too low less than 1000, it will ping agent too many times
			// and caused reconnect too many times
			EXECUTE_EVERY_N_MS(1000, state = (RMW_RET_OK == rmw_uros_ping_agent(100, 5)) ? AGENT_CONNECTED : AGENT_DISCONNECTED;);
			if (state == AGENT_CONNECTED) {
				rclc_executor_spin_some(&executor, RCL_MS_TO_NS(100));
			}
			break;
		case AGENT_DISCONNECTED:
			destroy_entities();
			state = WAITING_AGENT;
			break;
		default:
			break;
	}

	/*
	 * TODO : 
	 * Do anything else you want to do here,
	 * like read sensor data,  
	 * calculate something, etc.
	 */

	// sw1_state = ! digitalRead(SW1_PIN);
	if (actuatorRunning) {
		digitalWrite(led_PIN, HIGH);  // Turn on LED when actuator is running
		if (millis() - actionStart >= actionDuration) {
			stopActuator();  // Auto stop ONLY after duration is complete
		}
  	}else {digitalWrite(led_PIN, LOW); }

}
```
