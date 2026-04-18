# Changelog

## 0.1.11

### Bug fixes

- **Fix alarm echo loop in local mode** — echoing alarm packets back to the device caused an infinite acknowledgment loop. Alarm packets (16 bytes) are now excluded from echoing.
- **Fix alarm log source** — correctly shows "from device" or "from cloud" based on packet direction.

### New features

- **Alarm persistence (local/proxy mode)** — alarm setpoints are saved to disk and re-pushed to the device on (re)connect, surviving both device power cycles and process restarts.
  - HA add-on: `/data/alarms.json`
  - Standalone: `~/.local/share/grillsense/alarms.json`

## 0.1.10

### Bug fixes

- **Fix alarm echo loop in local monitor mode** — when the device acknowledged an alarm command, the echo handler would echo it back (same packet framing), causing the device to re-acknowledge in an infinite loop. Alarm packets are now excluded from echoing.
- **Fix incorrect log source** — alarm log messages in local mode incorrectly said "from cloud"; now correctly shows "from device" or "from cloud" based on packet direction.

### Other

- Updated copilot instructions (protocol endianness corrections, module descriptions, release process)

## 0.1.9

## What's New

### Bug fixes from code review
- **Local time in logs** — timestamps now show local time instead of UTC
- **MQTT keepalive fix** — separate 30s ping timer prevents broker disconnects during slow cloud API calls
- **Alarm state always reported** — HA shows 0 instead of "unknown" when no alarm is set
- **No more double cloud polling** — MQTT bridge shares data with display task via channel
- **BLE frame overflow detection** — panics with clear message instead of silently truncating long WiFi passwords
- **Consistent Fahrenheit** — local monitor now uses the same rounding as cloud mode

### Local/proxy mode improvements
- **Offline detection** — device marked offline in HA after 60s silence (was stuck online forever)
- **MQTT reconnect backoff** — 5s → 60s cap (was fixed 5s)
- **Reader task death fix** — triggers reconnect instead of silent exit

### Other
- MIT / Apache-2.0 dual license added
- HA add-on defaults to cloud mode (zero-config)

**Full Changelog**: https://github.com/bjornwein/grillsense/compare/v0.1.8...v0.1.9

## 0.1.8

## What's New

### MQTT alarm control fix
- **Fixed**: MQTT bridge no longer exits silently after receiving an alarm command. The reader task death (e.g. broker keepalive timeout during slow cloud API call) was incorrectly treated as clean shutdown instead of triggering reconnection.

### Console output cleanup
- MQTT monitoring mode now uses normal newlines for clean HA add-on logs (console-only mode keeps in-place overwrite)

**Full Changelog**: https://github.com/bjornwein/grillsense/compare/v0.1.7...v0.1.8

## 0.1.7

## What's New

### Cloud alarm control via MQTT
- Setting alarm thresholds in Home Assistant now forwards to the cloud API
- Alarm setpoints (ch1/ch2) included in MQTT state payload
- Bidirectional MQTT bridge using tokio::select! for concurrent poll + command handling

### Cloud API improvements
- `set_alarm_temp()` now supports per-channel alarm setting (ch1 or ch2)

### Full Changelog
**Full Changelog**: https://github.com/bjornwein/grillsense/compare/v0.1.6...v0.1.7

## 0.1.6
- Fix duplicate HA device when switching between local and cloud mode
- Fix console line clearing artifacts
- Detect stale cloud data using server timestamp (marks offline after 60s)
- Document cloud 10-minute staleness timeout and error 101
- Fix ha-apps translations

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
