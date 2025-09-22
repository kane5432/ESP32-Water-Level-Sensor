# Water Tank Level Monitor (ESPHome + Home Assistant)

If you found this project helpful, please consider supporting me:  

<a href="https://www.buymeacoffee.com/kanejarrats" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-blue.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>



This project uses an **ESP32‑C3** with an **ultrasonic sensor** to measure the water level in a tank, report the **fill percentage**, and **deep sleep** between readings to save power.  

It integrates with **Home Assistant**, storing the last known reading so you can still see the fill level even while the device is asleep.  

---

## Features
- Reads water tank level using an **AJ-SR04M** or similar ultrasonic sensor  
- Converts distance → **tank fill percentage**  
- **Deep sleep** between readings for extended battery life  
- **Persistent storage** in Home Assistant: keeps last known value when the ESP is offline  
- **OTA updates**: 2‑minute update window at each wake‑up for easy firmware flashing  

---

## Hardware
- **[Soldering Iron](https://s.click.aliexpress.com/e/_oFJoBJz)**
- **[ESP32‑C3 Super Mini](https://s.click.aliexpress.com/e/_opsH2Xv)** (or compatible ESP32 board)  
- **[Ultrasonic Distance Sensor](https://s.click.aliexpress.com/e/_oocTPPV)** e.g. JSN‑SR04T or AJ-SR04M for waterproof applications)
- **[6V 1W Solar Panel](https://s.click.aliexpress.com/e/_onX79Zh)**
- **[TP4056 Charging Module](https://s.click.aliexpress.com/e/_omYIa87)**
- **[5v Boost Converter](https://s.click.aliexpress.com/e/_oBvVHe7)**
- **[470μF Capacitor](https://s.click.aliexpress.com/e/_oE1Fw4j)**
- **[1 kΩ + 2 kΩ resistors](https://s.click.aliexpress.com/e/_oFfpK9d)** (for voltage divider on echo pin)
- **[Wires](https://s.click.aliexpress.com/e/_oD4px7l) and a small enclosure** (optional but recommended)


## Wire the Ultrasonic Sensor to ESP32
- VCC → 5V (from boost converter)
- GND → GND
- Trigger → GPIO 3 (change in YAML if needed)
- Echo → Voltage divider (2 kΩ to Echo, 1 kΩ to GND, take signal from between resistors) → GPIO 2

### Why?
The sensor outputs 5V on Echo, but the ESP32 only tolerates 3.3V, the voltage divider drops it to a safe level.

## Wire the Power System
1. Solar panel → TP4056 input (IN+) & (IN–)
2. TP4056 output (BAT+ & BAT–) → Li‑ion battery
3. TP4056 output (BAT+ & BAT–) → Boost converter input
4. Boost converter output (5V & GND) → ESP32 5V pin and Ultrasonic sensor VCC
5. 470 µF capacitor across the boost converter output (helps prevent brown‑outs when ESP wakes up).

![Schematic preview](main/Schematics.png)

---

## ESPHome Firmware
1. Copy the provided **`watertank-level.yaml`** file into your ESPHome directory (e.g., `esphome/watertank-level.yaml`).  
2. Replace the placeholders for Wi‑Fi credentials, API key, and OTA password.  
3. Adjust `max_distance` and `min_distance` in the YAML file to match your tank’s dimensions.  
4. Compile and flash the firmware to your ESP32 device using ESPHome.  

---

## Home Assistant Configuration
1. Open your `configuration.yaml`.  
2. Add the provided **input number** and **template sensor** sections for retaining the last known tank level.  
3. Restart Home Assistant to apply the changes.  

---

## Automation
1. Open your `automations.yaml`.  
2. Add the provided **automation** that updates the persistent tank level value whenever a new reading is received.  
3. Reload automations or restart Home Assistant.  

---

## Calibration
1. **Measure your tank’s depth when empty** → Set `max_distance` in the ESPHome file.  
2. **Measure the distance from the sensor to the water surface when full** → Set `min_distance`.  
3. **Flash the firmware** to your ESP device.  
4. **Monitor the ESPHome logs** to verify that readings are correct.  

---

## How it works
1. The ESP wakes up and connects to Wi‑Fi & Home Assistant API.  
2. It opens a **2‑minute OTA window** (can be changed in YAML) to allow firmware updates.  
3. The device takes a distance reading and converts it to a **fill percentage**.  
4. The reading is sent to Home Assistant.  
5. **If the reading succeeded:** the ESP goes into deep sleep for **1.5 hours**.  
6. **If the reading failed:** it retries after **1 minute**.

## Support this project
If you found this project helpful, you can support my work here:  

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-yellow?logo=buy-me-a-coffee)](https://buymeacoffee.com/kanejarrats)

