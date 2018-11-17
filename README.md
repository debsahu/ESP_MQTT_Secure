# ESP_MQTT_Secure 

:warning: :warning: :warning: **Will not be maintained/updated** :warning: :warning: :warning:

Demonstration on ESP8266 & ESP32 using SSL/TLSv1.2 two-way handshake with secured mosquitto broker.

[![ESP_MQTT_Secure](https://img.youtube.com/vi/xxxxxxxxxx/0.jpg)](https://www.youtube.com/watch?v=xxxxxxxxxxxx)

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
