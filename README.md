#ecgkit
An IoT-inspired Arduino Electrocardiograph Project

Riley Rainey, 2016
NOTE: this is a forked version of my [original open source project](https://github.com/rrainey/ecgkit) -- forked here to test Cloud Foundry in SAP HANA Cloud Platform

With an Arduino and inexepensive off-the-shelf shield, you can turn your Android device into an experimental ECG (EKG). Use this Android application, accompanying Arduino sketch, and Node.JS server code to collect and plot ECG data on your device and a server.

**The Architecture**
The Olimex ECG Shield is precalibrated to sense a heartbeat using standard electrodes. That input is fed into an Arduino A/D channel. A small Arduino sketch collects data and passes those via Bluetooth to your Android device for plotting. The Android application has the capability to upload the sampled data via an HTTPS RESTful API. Data on the server is stored in a Mongo database.

**Arduino and Bluetooth**
This application uses the xxxx USB/BT library. I chose the Arduino Mega ADK for this project because it has a built-in USB Host interface. This allows for Bluetooth connectivity using a BTLE USB dongle. Today, you can buy Arduino variants that include built-in Bluetooth hardware. Older Arduino models also might be able to leverage either a Bluetooth Shield or USB Host Shield, although none of those non-MegaADK configurations have been tested by me.

![Alt text](ecgkit-architecture.png "ecgkit architecture")

**Hardware Requirments**
- Android Device running Android 4.4.2 or later with Bluetooth

- [Arduino Mega ADK](https://www.arduino.cc/en/Main/ArduinoBoardMegaADK)  (any version; available from a number of sources)

- Bluetooth LE USB dongle - to connect from the Arduino ADK to the phone/tablet

- [Olimex ECG Shield](https://www.olimex.com/Products/Duino/Shields/SHIELD-EKG-EMG/) - model ID SHIELD-EKG-EMG (I ordered from Digi-key).

- Electrodes and cabling - Olimex also produces some covenient electrode cabling for use with their shield: mode ID [SHIELD-EKG-EMG-PRO](https://www.olimex.com/Products/Duino/Shields/SHIELD-EKG-EMG-PRO/). This particular cable requires disposable ECG Electrodes (I ordered these from Cables and Sensors).
- (optional) A Linux/OS X/Windows server capable of running Node.JS and Mongo DB.

**Directory Structure**

<code>src/android/MakerECG</code>- Android Studio project

<code>src/arduino/bt_ecgkit</code>- Arduino sketch (bt_ecgkit is the BLE version)

<code>src/server/ecgsvc</code>- Node.JS based web service for uploading and viewing data sets


**Arduino Build Notes**
Built using Arduino tools 1.6.7

Download the Google Accessory Development Kit 2011 edition

http://developer.android.com/tools/adk/adk.html

https://dl-ssl.google.com/android/adk/adk_release_20120606.zip


**Android Build Notes**
Using Android Studio 1.5.1

**Server**
The server is implemented using Node.JS. MongoDB is used for persistent storage. A simple data viewing web site was implemented using Bootstrap.

Use npm and [bower](https://github.com/bower/bower) to install the dependent components.

<code>$ npm install -g bower</code>

<code>$ cd src/server/ecgsvc</code>

<code>$ npm install</code>

Bower needs some coaxing to install the browser JS components in the "public" folder. You must manually create a file in your home directory named <code>.bowerrc</code>.

<code>$ vi ~/.bowerrc</code>

Add these lines to the file:

<code>{

  "directory" : "public/components"
  
}</code>

You can then run Bower.

<code>$ bower install</code>

Start your MongoDB instance. Check the settings in <code>config.json</code>.  Update the Mongo URL as required. The defaults reference a database on localhost named "site".

<code>$ node index.js</code># this will run the server

For a simulated production environment, I run all of this on an Ubuntu server using the Node pm2 package to manage the service.
