# GrillSense Thermometer — Documentation

## How it works

The add-on runs the `grillsense` binary, which receives temperature
packets from the thermometer over UDP (local mode) or polls the vendor
cloud API (cloud mode), then publishes to Home Assistant via MQTT
auto-discovery.

## Configuration

### Operating mode

| Mode | Description |
|------|-------------|
| `local` | Device sends UDP packets directly to this machine. Requires one-time device reconfiguration (see below). |
| `cloud` | Polls the GrillSense cloud API. No device reconfiguration needed but requires internet and a cloud account. |

### MQTT

MQTT credentials are **auto-detected** from the Mosquitto add-on via
the Supervisor services API. You only need to set `mqtt_host`,
`mqtt_user`, and `mqtt_pass` if you're using a different broker.

### Options

| Option | Required | Description |
|--------|----------|-------------|
| `mode` | yes | `local` or `cloud` |
| `udp_port` | local only | UDP port the device targets (default `17000`) |
| `device_name` | no | Name shown in Home Assistant (default "GrillSense Thermometer") |
| `cloud_email` | cloud only | GrillSense account email |
| `cloud_password` | cloud only | GrillSense account password |
| `mqtt_host` | no | Override MQTT broker host |
| `mqtt_port` | no | Override MQTT broker port (default `1883`) |
| `mqtt_user` | no | Override MQTT username |
| `mqtt_pass` | no | Override MQTT password |

## Reconfiguring the device for local mode

The thermometer ships configured to send data to
`smartserver.emaxtime.cn:17000`. For local mode, redirect it to your
Home Assistant machine:

```sh
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
| Temperature CH1–CH6 | sensor | Probe temperature in °C |
| Online | binary_sensor | Device connectivity status |
| Alarm CH1 | number | Alarm setpoint for channel 1 (0–300 °C) |
| Alarm CH2 | number | Alarm setpoint for channel 2 (0–300 °C) |
