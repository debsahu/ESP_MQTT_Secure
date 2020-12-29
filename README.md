# ESP_MQTT_Secure 

:warning: :warning: :warning: **Will not be maintained/updated** :warning: :warning: :warning:

Demonstration on ESP8266 & ESP32 using SSL/TLSv1.2 two-way handshake with secured mosquitto broker.

[![ESP_MQTT_Secure](https://img.youtube.com/vi/ytQUbyab4es/0.jpg)](https://www.youtube.com/watch?v=ytQUbyab4es)

## Dependencies

Listed below are the dpendencies used by Arduino IDE, but use **[PlatformioIO](https://platformio.org/)** instead!

| Library                   | Link                                                            | Use                 |
|---------------------------|-----------------------------------------------------------------|---------------------|
|PubSubClient               |https://github.com/knolleary/pubsubclient                        |mqtt comm impl       |
|MQTT                       |https://github.com/256dpi/arduino-mqtt                           |mqtt comm impl       |

## Installing mosquitto on RPi (Stretch) as of Nov 16th 2018

See video to find out the steps to obtain ca.crt, raspberrypi.crt, raspberry.key
```
$ sudo apt-get update
$ sudo apt-get install -y mosquitto mosquitto-clients
$ sudo systemctl enable mosquitto.service 

$ sudo cp ca.crt /etc/mosquitto/certs
$ sudo cp raspberrypi.crt /etc/mosquitto/certs
$ sudo cp raspberrypi.key /etc/mosquitto/certs

$ sudo chown mosquito: /etc/mosquitto/certs 

$ sudo mosquitto_passwd -c /etc/mosquitto/passwd miot
```
Add following lines to bottom of **/etc/mosquitto/mosquitto.conf**
```
allow_anonymous false
password_file /etc/mosquitto/passwd

listener 1883 127.0.0.1

listener 8883
cafile /etc/mosquitto/certs/ca.crt
certfile /etc/mosquitto/certs/raspberrypi.crt
keyfile /etc/mosquitto/certs/raspberrypi.key
require_certificate false

listener 9883
protocol websockets
cafile /etc/mosquitto/certs/ca.crt
certfile /etc/mosquitto/certs/raspberrypi.crt
keyfile /etc/mosquitto/certs/raspberrypi.key
require_certificate false

```
After updating mosquitto.conf, start the mosquitto server
```
$ sudo systemctl start mosquitto.service 
```
Remember to forward ports 8883 and 9883 to the internet!

## Sidenotes

# Rebase from fork
git remote add upstream https://github.com/debsahu/ESP_MQTT_Secure.git
git fetch upstream
git checkout master
git rebase upstream/master

# Modification needed
Create secrets_local.h, compile and run it.
This modified project uses the forked iostack https://github.com/menghin/IOTstack 

# Proposed hostname and mqtt channel

The hostname uses the first letter of the city and the street in its name and then an increasing index.
This makes it easier to figure out which sensor this is without adding too much infos
This can be defined in the two defines LOCATION and HOSTNAME
Example:
```
  #define LOCATION "g0_m0"
  #define HOSTNAME LOCATION "_0"
```

The resulting mqtt channel is then LOCATION "/" HOSTNAME "/out/sensorid";

# Links
https://maker.pro/arduino/tutorial/how-to-use-platformio-in-visual-studio-code-to-program-arduino
https://www.youtube.com/watch?v=ytQUbyab4es
