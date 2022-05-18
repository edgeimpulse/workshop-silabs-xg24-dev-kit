# Workshop using SiLabs xG24 Dev Kit

* **Objectives:** Build a keyword recognition application from scratch by collecting data from the embedded sensors, training and validating your machine learning pipeline using Edge Impulse Studio, and, deploying the custom model back to your device.
* **Duration:** ~90 min

## Workshop Agenda

1. **Introduction**

2. **Setup local environment**

* Install dependencies
* Flash default Edge Impulse firmware

3. **Build your machine learning pipeline**

* Collect data
* Create an Impulse
* Preprocess your data
* Train your machine learning model using Neural Networks
* Validate your model

 1. **Run the inference locally**

Deploy your custom model:

* Using Edge Impulse ready-to-go firmware
* Using Simplicity Studio 5 to add you business logic

# Hardware overview

The Silicon Labs xG24 Dev Kit (xG24-DK2601B) is a compact, feature-packed development platform built for the EFR32MG24 Cortex-M33 microcontroller. It provides the fastest path to develop and prototype wireless IoT products. This development platform supports up to +10 dBm output power and includes support for the 20-bit ADC as well as the xG24's AI/ML hardware accelerator. The platform also features a wide variety of sensors, a microphone, Bluetooth Low Energy and a battery holder - and it's fully supported by Edge Impulse! You'll be able to sample raw data as well as build and deploy trained machine learning models directly from the Edge Impulse Studio - and even stream your machine learning results over BLE to a phone.

The Edge Impulse firmware for this development board is open source and hosted on GitHub: [edgeimpulse/firmware-silabs-xg24](https://github.com/edgeimpulse/firmware-silabs-xg24).

![Silicon Labs xG24 Dev Kit Hardware Layout](assets/xg24-dk-hw-details.png)

# Setup local environment

## Install dependencies

To set this device up with Edge Impulse, you will need to install the following software:
1. [Simplicity Studio 5](https://www.silabs.com/developers/simplicity-studio)
1. [Simplicity Commander](https://community.silabs.com/s/article/simplicity-commander). A command line program we will use to flash firmware images onto the target.
1. The [Edge Impulse CLI](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation) which will enable you to connect your xG24 Dev Kit directly to Edge Impulse Studio, so that you can collect raw data and trigger in-system inferences.

> Problems installing the CLI?
> 
> See the [Installation and troubleshooting](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation) guide.

### Updating the firmware

Edge Impulse Studio can collect data directly from your xG24 Dev Kit and also help you trigger in-system inferences to debug your model, but in order to allow Edge Impulse Studio to interact with your xG24 Dev Kit you first need to flash it with our base firmware image.

### 1. Download the base firmware image

[Download the latest Edge Impulse firmware](https://cdn.edgeimpulse.com/firmware/silabs-xg24.zip), and unzip the file. Once downloaded, unzip it to obtain the `firmware-xg24.hex` file, which we will be using in the following steps.

### 2. Connect the xG24 Dev Kit to your computer

Use a micro-USB cable to connect the xG24 Dev Kit to your development computer (where you downloaded and installed [Simplicity Commander](https://community.silabs.com/s/article/simplicity-commander)).

![Connecting the xG24 Dev Kit to your computer](assets/xg24-dk-connect.png)

### 3. Load the base firmware image with Simplicity Commander

You can use [Simplicity Commander](https://community.silabs.com/s/article/simplicity-commander) to flash your xG24 Dev Kit with our [base firmware image](https://cdn.edgeimpulse.com/firmware/silabs-xg24.zip). To do this, first select your board from the dropdown list on the top left corner:

![Connecting the xG24 Dev Kit to Simplicity Commander](assets/xg24-dk-commander-select-board.png)

Then go to the "Flash" section on the left sidebar, and select the base firmware image file you downloaded in the first step above (i.e., the file named `firmware-xg24.hex`). You can now press the `Flash` button to load the base firmware image onto the xG24 Dev Kit.

![Flashing the xG24 Dev Kit base image](assets/xg24-dk-commander-flash.png)
> Keep Simplicity Commander Handy
> 
> Simplicity Commander will be needed to upload any other project built on Edge 
> Impulse, but the base firmware image only has to be loaded once.


## Connecting to Edge Impulse

With all the software in place, it's time to connect the xG24 Dev Kit to Edge Impulse.

### 1. Connect the development board to your computer

Use a micro-USB cable to connect the development board to your computer.

### 2. Setting keys

From a command prompt or terminal, run:

```
edge-impulse-daemon
```

This will start a wizard which will ask you to log in, and choose an Edge Impulse project. If you want to switch projects run the command with `--clean`.

Alternatively, recent versions of Google Chrome and Microsoft Edge can collect data directly from your development board, without the need for the Edge Impulse CLI. See [this blog post](https://www.edgeimpulse.com/blog/collect-sensor-data-straight-from-your-web-browser) for more information.

### 3. Verifying that the device is connected

That's all! Your device is now connected to Edge Impulse. To verify this, go to [your Edge Impulse project](https://studio.edgeimpulse.com/studio/select-project?autoredirect=1), and click **Devices** on the left sidebar. The device will be listed there:

![Device connected to Edge Impulse.](assets/xg24-dk-device-connected.png)

# Build your machine learning pipeline

If you do not have an Edge Impulse account yet, start by creating an account on Edge Impulse Studio and create a project.

On this new project, select the `SiLabs EFR32MG24` board so the latency and performance estimations will calculated for your microcontroller:

![studio dashboard](assets/studio-dashboard.png)

## Collect data from the device

To start collecting some data, go to the Data acquisition view and click on the Connect using WebUSB button on the upper right corner:

![studio webUSB](assets/studio-webusb.png)

![studio sensors](assets/studio-sensors.png)