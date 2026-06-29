
## Electronic knowledge

## Protocols

| Protocol          | Wires                         | Distance           | Speed               | Typical Devices                       |
| ----------------- | ----------------------------- | ------------------ | ------------------- | ------------------------------------- |
| Digital GPIO      | 1 signal (+ power and ground) |                    | Simple              | LEDs, relays, buttons, PIR, DHT11     |
| Analog            | 1 signal                      |                    | Voltage measurement | Potentiometer, LDR, analog gas sensor |
| I²C               | 2                             | <1 m               | Medium              | OLED, RTC, BMP280, BME280             |
| SPI               | 4 + chip select               | <1 m               | Fast                | SD card, TFT display, RFID            |
| UART (Serial)     | 2                             | 1–5 m              | Medium/Fast         | GPS, GSM, another microcontroller     |
| Wi-Fi (MQTT/HTTP) | 0                             | Entire house       | HIGH                | WiFi Compatable Device                |
| RS-485            | 2                             | Hundreds of meters | Medium              | to another RS-485 module              |


## Pin Mapping Table 

| Board Label | GPIO Number  | Can use in code as |
| ----------- | ------------ | ------------------ |
| D0          | 16           | `D0` or `16`       |
| D1          | 5            | `D1` or `5`        |
| D2          | 4            | `D2` or `4`        |
| D3          | 0            | `D3` or `0`        |
| D4          | 2            | `D4` or `2`        |
| D5          | 14           | `D5` or `14`       |
| D6          | 12           | `D6` or `12`       |
| D7          | 13           | `D7` or `13`       |
| D8          | 15           | `D8` or `15`       |
| RX          | 3            | `RX` or `3`        |
| TX          | 1            | `TX` or `1`        |
| A0          | Analog input | `A0` only          |
|             |              |                    |

## Assign a variable `in code`
```
// For store int (e.g - pin number) variable
// --------------------------
// Pins
const int led = D2; // Board label  
const int relay = 14; // GPIO number
const uint8_t LED_PIN = D2;

// General numbers  
const int YEAR = 2026;  
const long BIG_NUMBER = 20260627;


// For store a string variable
// ------------------------------
const char* variable = "MyName"
// (or)
const char variable[] = "MyName"
// (or)
String variable = "MyName";
```

---
# Questions?
1. MQTT broker?
2. RS-485 modules?