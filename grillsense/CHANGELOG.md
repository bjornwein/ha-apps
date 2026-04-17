# Changelog

## 0.1.3
- Fix keepalive echo: respond to 14-byte registration packets so device
  starts sending temperature data
- Add echo to proxy mode (works even without cloud forwarding)
- Move build_echo to protocol module with full test coverage
- Update PROTOCOL.md: document keepalive packet format, correct WiFi module
  (HF-LPT230) and BLE chip (Freqchip FR-BLE-1.0)

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
