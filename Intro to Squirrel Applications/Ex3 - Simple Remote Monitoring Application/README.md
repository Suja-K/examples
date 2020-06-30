# Simple Remote Monitoring Application

In this example we will create an simple remote monitoring application that takes synchronous readings from multiple sensors and sends them to the cloud using the Initial State webservice. We will use a Hardware Abstraction Layer (HAL) to reference all hardware objects, and to organize our application code we will use a class. This code can be easily configured for use with an imp006 Breakout Board, impExplorer Developer Kit, impAccelerator Battery Powered Sensor Node or impC001 Breakout Board.

## Skill level

**Beginner**

This example will focus on writing squirrel code. Please visit the [getting started guide](https://developer.electricimp.com/gettingstarted) on the Electric Imp Dev Center for insturctions on how to configure your device with BlinkUp and how to use the Electric Imp IDE, impCentral.

## What You Learn

* How to use Electric Imp libraries
* How to use a Hardware Abstraction Layer (HAL)
* How to write a `class` in Squirrel
* How to configure sensors to take synchronous readings
* How to send data between device and agent
* How to send data to Initial State

## What You Need

* Your 2.4GHz 802.11bgn WiFi network name (SSID) and password (not needed for Cellular, impC001)
* A computer with a web browser
* A smartphone with the Electric Imp app ([iOS](https://itunes.apple.com/us/app/electric-imp/id547133856) or [Android](https://play.google.com/store/apps/details?id=com.electricimp.electricimp))
* A free [Electric Imp Developer Account](https://impcentral.electricimp.com/login)
* A free [Initial State Account](https://www.initialstate.com/)
* One of the imp hardware boards listed below
    * [imp006 Breakout Board Kit](https://store.electricimp.com/collections/breakout-boards/products/imp006-cellular-and-wifi-breakout-board-kit?variant=30294487924759)
    * [impExplorer Developer Kit](https://store.electricimp.com/collections/featured-products/products/impexplorer-developer-kit?variant=31118866130)
    * [impAccelerator Battery Powered Sensor Node](https://developer.electricimp.com/hardware/resources/reference-designs/sensornode)
    * [impC001 Cellular Breakout Board Kit](https://developer.electricimp.com/hardware/resources/reference-designs/impc001breakout)

## Instructions

* BlinkUp your device.
* Log into the [impCentral](https://impcentral.electricimp.com/login).
* Create a new [Product](https://developer.electricimp.com/tools/impcentral/impcentralintroduction#app-products) and [Development Device Group](https://developer.electricimp.com/tools/impcentral/impcentralintroduction#app-development-devicegroup).
* Copy and Paste the Device Code into the Device coding pane in the impCentral code editor.
* Locate the HAL for your hardware. The HAL files can be found on Github in the repositories linked below. Find the `.HAL.nut` file in the repository that matches your hardware.
    * [imp006 Breakout Board HAL](https://github.com/electricimp/imp006BreakoutBoardHAL)
    * [impExplorer Developer Kit HAL](https://github.com/electricimp/ExplorerKitHAL)
    * [impAccelerator Battery Powered Sensor Node](https://github.com/electricimp/SensorNodeHAL)
    * [impC001 Cellular Breakout Board](https://github.com/electricimp/CellularBreakoutHAL)
* Copy and Paste the HAL table into the code in the HARDWARE ABSTRACTION LAYER section. Below is an example of what a HAL table will look like when insterted into the code. *Please note:* DO NOT copy and paste from thie example, use the HAL found in github.

```
// HARDWARE ABSTRACTION LAYER
// ---------------------------------------------------
// HAL's are tables that map human readable names to
// the hardware objects used in the application.

// Copy and Paste Your HAL here
ExplorerKit_001 <- {
    "LED_SPI" : hardware.spi257,
    "SENSOR_AND_GROVE_I2C" : hardware.i2c89,
    "TEMP_HUMID_I2C_ADDR" : 0xBE,
    "ACCEL_I2C_ADDR" : 0x32,
    "PRESSURE_I2C_ADDR" : 0xB8,
    "POWER_GATE_AND_WAKE_PIN" : hardware.pin1,
    "AD_GROVE1_DATA1" : hardware.pin2,
    "AD_GROVE2_DATA1" : hardware.pin5
}


// REMOTE MONITORING APPLICATION CODE
// ---------------------------------------------------
```

* Assign your hardware class variables. In the Application class before the constructor you will find a number of class variables. You will need to re-assign the hardware variables so they look something like the example below. *Please note:* DO NOT copy and paste from thie example, as these values may differ from the ones in your HAL.

```
// REMOTE MONITORING APPLICATION CODE
// ---------------------------------------------------
// Application code, take readings from our sensors
// and send the data to the agent

class Application {

    // Time in seconds to wait between readings
    static READING_INTERVAL_SEC = 30;
    // Accelerometer data rate in Hz
    static ACCEL_DATARATE = 1;

    // Hardware variables
    i2c             = ExplorerKit_001.SENSOR_AND_GROVE_I2C;
    tempHumidAddr   = ExplorerKit_001.TEMP_HUMID_I2C_ADDR;
    accelAddr       = ExplorerKit_001.ACCEL_I2C_ADDR;

    // Sensor variables
    tempHumid = null;
    accel = null;

    constructor() {...}
```

* Copy and paste the Agent Code into the Agent Code pane in the impCentral code editor.
* Sign into [Initial State](https://app.initialstate.com/#/login/account).
* Find your Streaming Access Key on the [My Account page](https://app.initialstate.com/#/account).
* Navigate back to the [impCentral](https://impcentral.electricimp.com/).
* In the Agent code enter your Initial State Streaming Access Key into the Application class static STREAMING_ACCESS_KEY variable on line 23.
* Hit Build and Force Restart button to start the code.
* Note the agent ID in the logs.
* Navigate back to Initial State, find the Bucket that matches your agent ID.
* Watch your data update in the Source, Lines, Waves, and Tile views on the Inital State website.
