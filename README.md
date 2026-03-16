# Shairport Sync add-on for Home Assistant

[![GitHub Release][releases-shield]][releases]
[![License][license-shield]](LICENSE)

AirPlay and AirPlay 2 audio receiver for Home Assistant, powered by
[Shairport Sync](https://github.com/mikebrady/shairport-sync) 5.0.1.
Publishes metadata over MQTT so the
[hass-shairport-sync](https://github.com/parautenbach/hass-shairport-sync)
HACS integration can display a full media-player entity.

## Supported architectures

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

## Prerequisites

- **Mosquitto broker** add-on installed and running (for MQTT metadata)
- **hass-shairport-sync** installed via HACS (for the media-player entity)

## Installation

1. Go to **Settings → Add-ons → Add-on store** in Home Assistant.
2. Click the three-dot menu → **Repositories** and add this repo URL.
3. Find and install **Shairport Sync**.
4. Configure the add-on options (see below).
5. Start the add-on.

## Configuration options

| Option | Default | Description |
|--------|---------|-------------|
| `log_level` | `info` | Log verbosity: `trace` `debug` `info` `notice` `warning` `error` `fatal` |
| `airplay_name` | `Home Assistant` | Name shown on AirPlay device lists |
| `avahi_interfaces` | *(auto)* | Network interface for Avahi; leave empty to auto-detect |
| `avahi_hostname` | *(auto)* | Avahi hostname; leave empty to use the system hostname |
| `avahi_domainname` | `local` | mDNS domain |
| `enable_ipv6` | `false` | Enable IPv6 |
| `interpolation` | `soxr` | Sample-rate conversion quality: `auto` `basic` `soxr` |
| `alsa_output_device` | *(default)* | ALSA device to output to, e.g. `hw:CARD=PO100,DEV=0`. Leave empty to use the HA audio system (PulseAudio). |
| `audio_backend_buffer_desired_length_in_seconds` | `0.5` | Audio buffer size in seconds. Increase (e.g. `1.0`) if you hear dropouts or underruns. |
| `mqtt_enabled` | `false` | Enable MQTT metadata publishing |
| `mqtt_hostname` | `core-mosquitto` | MQTT broker hostname (`core-mosquitto` for the Mosquitto add-on) |
| `mqtt_port` | `1883` | MQTT broker port |
| `mqtt_topic` | `shairport-sync` | MQTT topic prefix. Must match the topic configured in hass-shairport-sync. |
| `mqtt_username` | *(none)* | MQTT username (if broker requires authentication) |
| `mqtt_password` | *(none)* | MQTT password |

## Finding your USB audio device name

If you use a USB S/PDIF adapter (e.g. SMSL PO100), find its ALSA name by running
this command in the **Terminal & SSH** add-on:

```bash
aplay -l
```

Look for a line like `card 1: PO100 [SMSL PO100], device 0` and set
`alsa_output_device` to `hw:CARD=PO100,DEV=0`.

If `alsa_output_device` is left empty, audio is routed through the HA audio
system (PulseAudio), which also works and allows device selection via the
system audio settings.

## MQTT / hass-shairport-sync setup

1. Enable the **Mosquitto broker** add-on.
2. In this add-on, set `mqtt_enabled: true` and configure `mqtt_hostname`,
   `mqtt_port`, and `mqtt_topic`.
3. In hass-shairport-sync, set the same `mqtt_topic` prefix.
4. Restart both add-ons. A `media_player` entity will appear in Home Assistant.

## Advanced configuration (custom config file)

For settings not exposed in the UI, place a hand-crafted config at
`/config/shairport-sync.conf`. When this file exists the add-on uses it
as-is and ignores all UI options. Refer to the
[shairport-sync documentation](https://github.com/mikebrady/shairport-sync/blob/master/README.md)
for the full configuration reference.

## AirPlay 2 notes

AirPlay 2 requires the **nqptp** timing daemon, which is built into this
add-on and starts automatically. nqptp uses PTP ports 319/320 (UDP); 
Requires iOS 11.4+ or
macOS 10.15+ on the sending device.

## Technical details

- Shairport Sync **5.0.1** (compiled from source with ALSA, PulseAudio, Avahi,
  soxr, MQTT, AirPlay 2)
- nqptp **1.2.6**
- Base image: `ghcr.io/hassio-addons/base:20.0.1` (Alpine 3.21)
- s6-overlay v3 service management

[aarch64-shield]: https://img.shields.io/badge/architecture-aarch64-blue.svg
[amd64-shield]: https://img.shields.io/badge/architecture-amd64-blue.svg
[license-shield]: https://img.shields.io/github/license/WangChuDi/addon-shairport-sync.svg
[releases-shield]: https://img.shields.io/github/release/WangChuDi/addon-shairport-sync.svg
[releases]: https://github.com/WangChuDi/addon-shairport-sync/releases
