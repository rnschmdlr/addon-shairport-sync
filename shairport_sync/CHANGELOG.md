# Changelog

## 1.1.9

- Fix nqptp readiness check: nqptp uses POSIX shared memory (`/dev/shm/nqptp`), not a Unix socket — the wrong path caused shairport-sync to hang indefinitely waiting for nqptp, which is why AirPlay 2 never announced itself and the finish script timed out
- Add 30-second timeout with warning so startup never hangs silently again

## 1.1.8

- Fix restart loop: clean up stale `/run/dbus/dbus.pid` in `init-dbus` before starting D-Bus — an ungraceful shutdown (finish script SIGKILL) left this file behind, causing D-Bus to refuse to start on the next attempt, taking down Avahi and shairport-sync with it
- Also clean up stale Avahi PID and socket files in `init-avahi` for the same reason

## 1.1.7

- Add `NET_ADMIN` capability — required for nqptp PTP multicast operations (AirPlay 2 timing sync)
- Wait for nqptp socket (`/tmp/nqptp`) before starting shairport-sync to eliminate startup race condition

## 1.1.6

- Remove `--with-apple-alac` build flag — deprecated and superseded by the FFmpeg ALAC decoder already included for AirPlay 2

## 1.1.5

- Fix build: add `ffmpeg-dev` / `ffmpeg-libs` — AirPlay 2 requires `libavutil` from FFmpeg

## 1.1.4

- Fix plistutil build: binary is at `tools/plistutil` not `src/plistutil`; use `autogen.sh` with `--with-tools` flag

## 1.1.3

- Pre-build Docker images via GitHub Actions and push to ghcr.io — eliminates on-device compilation, preventing install timeouts and crashes on Raspberry Pi

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
