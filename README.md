# Water Tank Level Monitor (ESPHome + Home Assistant)

If you found this project helpful, consider supporting me:  

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-yellow?logo=buy-me-a-coffee)](https://buymeacoffee.com/kanejarrats)


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
- **ESP32‑C3 DevKitM‑1** (or compatible ESP32 board)  
- **Ultrasonic distance sensor** (e.g., HC‑SR04, JSN‑SR04T, AJ-SR04M for waterproof applications)  
- Wires & power supply (battery or mains)  

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
2. It opens a **2‑minute OTA window** to allow firmware updates.  
3. The device takes a distance reading and converts it to a **fill percentage**.  
4. The reading is sent to Home Assistant.  
5. **If the reading succeeded:** the ESP goes into deep sleep for **1.5 hours**.  
6. **If the reading failed:** it retries after **1 minute**.

## Support this project
If you found this project helpful, you can support my work here:  

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-yellow?logo=buy-me-a-coffee)](https://buymeacoffee.com/kanejarrats)

