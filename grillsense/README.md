# GrillSense Thermometer

Monitor your GrillSense / Dangrill WiFi BBQ thermometer directly from
Home Assistant.

## Requirements

- **MQTT broker** (e.g. the Mosquitto add-on) — required for HA integration
- A GrillSense/Dangrill WiFi thermometer already provisioned and connected to WiFi

## Quick start

Install the add-on and start it. By default it runs in **cloud mode**,
which auto-discovers your thermometer on the local network and polls
temperature data via the vendor cloud API. Entities appear automatically
in Home Assistant via MQTT auto-discovery.

No configuration needed if you have the Mosquitto add-on installed.

## Features

- **Up to 6 temperature probe sensors** (CH1–CH6, depending on hardware)
- **Device online/offline status**
- **Alarm setpoint control** (CH1 & CH2, 0–300 °C)
- **MQTT auto-discovery** — entities appear automatically
- **Two operating modes**: cloud (default) or local (UDP direct)

## Source

<https://github.com/bjornwein/grillsense>
