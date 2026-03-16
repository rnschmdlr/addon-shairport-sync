# Changelog

## 1.1.2

- Fix Docker build failure: build `plistutil` from source (not packaged as standalone binary on Alpine Linux, required for AirPlay 2 configure check)

## 1.1.1

- Add `volume_control_profile` option (`dasl_tapered` / `standard` / `flat`)
- Add `allow_session_interruption` option (was hardcoded `yes`)
- Add `log_verbosity` option for shairport-sync internal log detail
- Add `vernier` interpolation mode
- Fix AirPlay 2 build flag (`--with-airplay-2` was incorrectly `--with-airplay2`)
- Promote nqptp to a supervised s6 service with proper restart-on-crash
- Fix avahi readiness check (poll socket instead of blind sleep)
- Fix release badge URLs

## 1.1.0

- Pin shairport-sync to 5.0.1, nqptp to 1.2.6
- Add MQTT support (`--with-mqtt-client`) for hass-shairport-sync integration
- Add AirPlay 2 support via nqptp
- Add persistent config file at `/config/shairport-sync.conf` (user file wins)
- Expose MQTT, audio buffer, and interpolation options in the UI
- Rewrite README in English with setup guide
- Update base image to `ghcr.io/hassio-addons/base:20.0.1`
- Fix invalid `NET_BIND_SERVICE` capability that prevented the add-on from appearing in the store

## 1.0.9

- Initial fork from WangChuDi/addon-shairport-sync
- ALSA backend with PulseAudio routing
- s6-overlay v3 service structure
