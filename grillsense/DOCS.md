# GrillSense Thermometer — Documentation

## How it works

The add-on runs the `grillsense` binary, which polls the vendor cloud
API (cloud mode) or receives UDP packets directly from the thermometer
(local mode), then publishes to Home Assistant via MQTT auto-discovery.

**MQTT is required.** Install the Mosquitto broker add-on (or configure
an external broker) before starting GrillSense.

## Quick start (cloud mode)

The add-on starts in **cloud mode** by default. It auto-discovers
your thermometer via LAN broadcast and polls temperatures through the
vendor cloud API. No configuration is needed if:

1. Your thermometer is already provisioned and on your WiFi network
2. The Mosquitto add-on is installed

Just install the add-on, start it, and entities appear in Home Assistant.

## Configuration

### Operating mode

| Mode | Description |
|------|-------------|
| `cloud` (default) | Polls the GrillSense cloud API. Works with a factory-provisioned device — no reconfiguration needed. Requires internet. |
| `local` | Device sends UDP packets directly to this machine. Lower latency, no cloud dependency, but requires one-time device reconfiguration (see below). |

### Options

| Option | Required | Description |
|--------|----------|-------------|
| `mode` | no | `cloud` (default) or `local` |
| `device_mac` | no | WiFi MAC address of the thermometer. Leave empty to auto-discover. |
| `device_name` | no | Name shown in Home Assistant (default "GrillSense Thermometer") |
| `udp_port` | local only | UDP port the device targets (default `17000`) |
| `mqtt_host` | no | Override MQTT broker host (auto-detected from Mosquitto add-on) |
| `mqtt_port` | no | Override MQTT broker port (default `1883`) |
| `mqtt_user` | no | Override MQTT username |
| `mqtt_pass` | no | Override MQTT password |

## Reconfiguring the device for local mode

Local mode requires redirecting the thermometer to send UDP packets to
your Home Assistant machine instead of the vendor cloud. This is a
one-time setup that requires the `grillsense` CLI tool running on a
Linux machine on the same network:

```sh
# Install from source
cargo install --git https://github.com/bjornwein/grillsense

# Redirect device to your HA machine
grillsense local configure \
    --ip <device-ip> \
    --ssid <your-wifi-ssid> \
    -P <wifi-password> \
    --server <ha-machine-ip>
```

This sends AT commands to the device's WiFi module over the LAN.

To restore cloud operation:
```sh
grillsense local configure \
    --ip <device-ip> \
    --ssid <your-wifi-ssid> \
    -P <wifi-password> \
    --server smartserver.emaxtime.cn
```

## Entities created

| Entity | Type | Description |
|--------|------|-------------|
| Temperature CH1–CH6 | sensor | Probe temperature in °C (up to 6, depending on hardware) |
| Online | binary_sensor | Device connectivity status |
| Alarm CH1 | number | Alarm setpoint for channel 1 (0–300 °C) |
| Alarm CH2 | number | Alarm setpoint for channel 2 (0–300 °C) |
