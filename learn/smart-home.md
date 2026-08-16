# Smart Home 

#sence #monitor #control #logger 

## Smart Home Wiring
1. WIFi Ethernet socket & wire
2. Dish socket & wire -TV, Speaker
3. Radio socket & wire - Radio
4. Doorbell Wire
5. Walkie talkie wire
---
## Smart Home things
### Live System
- Live Home Brain - Raspberry Pi
	1. AI Supervision
	2. Listen and Speak - Voice Assistant (Communicate)
	3. See and Show - Camera Setup & Display setup
	4. Actions and Sense - The Compute System (Do some actions, Log)
- speaker
- camera
- screens

### Compute System
1. Water tank sensor  #sence #monitor #control 
2. Current & voltage power consume sensor #sence #monitor 
3. invertor battery indication #sence #monitor 
4. Temperature & Humidity sensor #monitor #logger
5. Smart Switches #control 
6. Motion & presence sensor with wifi

### Non-Compute System
1. Plant watering system
2. Almira door light sensor
3. Walkie Talkie

---
### Things to BUY
1. Barometric pressure and altitude sensor - BMP280
2. Ethernet modules
3. zomei-led-ac-current-voltage-frequency-light-60-500v-0-100a-digital-ammeter-voltmeter-tester

---
# MQTT
## Raspberry Pi Zero W MQTT Setup


A **Raspberry Pi Zero 2 W** is a great MQTT hub for a smart home. It can run the MQTT broker, Home Assistant, Node-RED, and other services while your ESP8266 boards act as sensors and switches.

## Recommended Architecture

```
                         Wi-Fi                           │        ┌──────────────────┼──────────────────┐        │                  │                  │   ESP8266 #1         ESP8266 #2        ESP8266 #3   DHT11 Sensor       Relay Switch      PIR Sensor        │                  │                  │        └──────────────────┼──────────────────┘                           │                  Raspberry Pi Zero 2 W                  ├── Mosquitto MQTT Broker                  ├── Home Assistant                  ├── Node-RED (optional)                  ├── InfluxDB (optional)                  └── Grafana (optional)                           │                    Phone / Laptop
```

## Hardware

### Server

- Raspberry Pi Zero 2 W (recommended over the original Zero W)
- 16–32 GB microSD card (Class 10)
- 5 V / 2.5 A power supply

### Devices

- ESP8266 NodeMCU boards
- DHT11/DHT22
- Relay modules
- PIR sensors
- Ultrasonic sensors
- Any other ESP8266-based devices

---

# Step 1: Install Raspberry Pi OS

1. Download **Raspberry Pi Imager**.
2. Flash **Raspberry Pi OS Lite (64-bit)**.
3. Configure:
    - Wi-Fi
    - Username
    - Password
    - Enable SSH

Insert the SD card and boot the Pi.

Find its IP address:

```
hostname -I
```

Example:

```
192.168.1.20
```

SSH into it:

```
ssh pi@192.168.1.20
```

---

# Step 2: Update the Pi

```
sudo apt updatesudo apt upgrade -y
```

---

# Step 3: Install Mosquitto MQTT Broker

```
sudo apt install mosquitto mosquitto-clients -y
```

Enable it:

```
sudo systemctl enable mosquittosudo systemctl start mosquitto
```

Check status:

```
sudo systemctl status mosquitto
```

---

# Step 4: Test MQTT

Open Terminal 1:

```
mosquitto_sub -t home/test
```

Open Terminal 2:

```
mosquitto_pub -t home/test -m "Hello MQTT"
```

You should see:

```
Hello MQTT
```

---

# Step 5: Install Home Assistant

The easiest supported approach is **Home Assistant OS** on supported hardware. Since the Pi Zero 2 W has limited resources, many people instead run **Home Assistant Container** or use a more powerful Pi (such as a Pi 4 or Pi 5) for the best experience.

If you keep the Zero 2 W dedicated to MQTT and Node-RED, it performs much better.

---

# Step 6: Install Node-RED

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

Enable it:

```
sudo systemctl enable nodered.service
```

Open:

```
http://PI_IP:1880
```

---

# Step 7: Install MQTT Explorer (PC)

Connect to

```
Broker:192.168.1.20Port:1883
```

You will see every topic.

---

# Step 8: ESP8266 Publisher

Connect to Wi-Fi and publish sensor values.

Example topics:

```
home/room1/temperaturehome/room1/humidityhome/room1/motionhome/room1/light
```

---

# Step 9: ESP8266 Subscriber

Subscribe:

```
home/room1/light
```

Receive:

```
ONOFF
```

Turn relay ON/OFF.

---

# Example MQTT Topic Structure

```
home/    livingroom/        temperature        humidity        light        fan    bedroom/        light        fan    kitchen/        gas    gate/        status
```

---

# Security

Create an MQTT user:

```
sudo mosquitto_passwd -c /etc/mosquitto/passwd akash
```

Create a configuration file:

```
sudo nano /etc/mosquitto/conf.d/auth.conf
```

Add:

```
allow_anonymous falsepassword_file /etc/mosquitto/passwdlistener 1883
```

Restart Mosquitto:

```
sudo systemctl restart mosquitto
```

Your ESP8266 devices will then connect using the username and password you created.

---

## Recommended Software Stack

|Device|Software|
|---|---|
|Raspberry Pi Zero 2 W|Raspberry Pi OS Lite|
|MQTT Broker|Mosquitto|
|Automation|Node-RED|
|Dashboard|Home Assistant (better on Pi 4/5)|
|ESP8266|Arduino IDE + PubSubClient|
|Monitoring|MQTT Explorer|

### Suggested Learning Order

1. Install Raspberry Pi OS Lite.
2. Install and test Mosquitto.
3. Connect one ESP8266 and publish a test message.
4. Add a DHT11 sensor that publishes temperature and humidity.
5. Add another ESP8266 with a relay that subscribes to commands.
6. Add Node-RED flows for automation.
7. Add Home Assistant if you want dashboards and device management.

This staged approach makes it much easier to troubleshoot each component before building a larger smart home system.

now setup in esp8266

Great! Now let's set up your **ESP8266 (NodeMCU)** to connect to the MQTT broker running on your **Raspberry Pi Zero 2 W**.

## Step 1: Install Required Libraries

In the Arduino IDE, install:

- ✅ **PubSubClient** by Nick O'Leary
- ✅ **ESP8266** board package (via Boards Manager)

---

## Step 2: Replace These Values

```
const char* ssid = "YOUR_WIFI_NAME";const char* password = "YOUR_WIFI_PASSWORD";const char* mqtt_server = "192.168.1.20";   // Raspberry Pi IPconst int mqtt_port = 1883;// If you enabled MQTT authenticationconst char* mqtt_user = "akash";const char* mqtt_pass = "your_password";
```

Replace:

- `YOUR_WIFI_NAME`
- `YOUR_WIFI_PASSWORD`
- `192.168.1.20` with your Pi's IP address.

---

## Step 3: Simple MQTT Publisher

This example publishes `"Hello from ESP8266"` every 5 seconds.

```
#include <ESP8266WiFi.h>#include <PubSubClient.h>const char* ssid = "YOUR_WIFI_NAME";const char* password = "YOUR_WIFI_PASSWORD";const char* mqtt_server = "192.168.1.20";const int mqtt_port = 1883;WiFiClient espClient;PubSubClient client(espClient);void connectWiFi() {  Serial.print("Connecting to WiFi");  WiFi.begin(ssid, password);  while (WiFi.status() != WL_CONNECTED) {    delay(500);    Serial.print(".");  }  Serial.println();  Serial.println("WiFi Connected");}void reconnect() {  while (!client.connected()) {    Serial.print("Connecting to MQTT...");    if (client.connect("ESP8266_Client")) {      Serial.println("Connected");    } else {      Serial.print("Failed, rc=");      Serial.print(client.state());      Serial.println(" Retrying...");      delay(3000);    }  }}void setup() {  Serial.begin(115200);  connectWiFi();  client.setServer(mqtt_server, mqtt_port);}void loop() {  if (!client.connected()) {    reconnect();  }  client.loop();  static unsigned long lastMsg = 0;  if (millis() - lastMsg > 5000) {    lastMsg = millis();    client.publish("home/test", "Hello from ESP8266");    Serial.println("Message Published");  }}
```

---

## Step 4: Test It

On the Raspberry Pi, open a terminal:

```
mosquitto_sub -t home/test
```

You should receive:

```
Hello from ESP8266Hello from ESP8266Hello from ESP8266
```

---

## Step 5: Publish DHT11 Data

After the basic example works, add the DHT11 library and publish the sensor readings.

```
float temp = dht.readTemperature();float hum  = dht.readHumidity();char tempStr[10];char humStr[10];dtostrf(temp, 4, 1, tempStr);dtostrf(hum, 4, 1, humStr);client.publish("home/room1/temperature", tempStr);client.publish("home/room1/humidity", humStr);
```

---

## Step 6: Next Device (Relay)

A second ESP8266 can subscribe to:

```
home/room1/light
```

Messages:

```
ONOFF
```

and switch a relay accordingly.

---

### My recommendation for your project

Since you've said you want to build a complete smart home, we can build it in stages:

1. ✅ ESP8266 → MQTT (Hello World)
2. ✅ ESP8266 + DHT11 → Publish temperature and humidity
3. ✅ ESP8266 + Relay → Subscribe and control appliances
4. ✅ Node-RED automation on Raspberry Pi
5. ✅ Home Assistant dashboard

This sequence keeps each step simple and easy to test before adding the next feature.

----

## ESP MQTT SETUP