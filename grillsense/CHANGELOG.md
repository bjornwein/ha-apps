# Changelog

## 0.1.2
- Fix s6-overlay startup (add init: false)
- Fix Supervisor API access for MQTT service discovery
- Use native ARM runner for faster aarch64 builds

## 0.1.1
- Fix docker build

## 0.1.0

- Initial release
- Local and cloud operating modes
- 6 temperature probe sensors via MQTT auto-discovery
- Alarm setpoint control for CH1 and CH2
- Auto-detect device ID from UDP packets
- Auto-detect MQTT credentials from Mosquitto add-on
