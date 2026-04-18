# Changelog

## 0.1.5
- Add MAC auto-discovery via LAN broadcast (no manual MAC config needed)
- Detect stale cloud data using server timestamp (marks offline after 60s)
- MQTT auto-reconnect with backoff on connection loss
- Infinite retry for autodiscovery in service mode
- Console monitor no longer exits after repeated errors
- Remove unused --token parameter from monitor and set-alarm
- Fix console line clearing artifacts
- Simplify HA add-on config: replace cloud email/password with device_mac
- Document cloud 10-minute staleness timeout and error 101

## 0.1.4
- Fix temperature parsing: use little-endian u16 at correct offsets
  (previously read as big-endian at wrong offset, broke above 25.6°C)
- Update PROTOCOL.md with corrected temperature encoding

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
