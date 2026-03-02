ARG BUILD_FROM=ghcr.io/hassio-addons/base:20.0.1
# hadolint ignore=DL3006
FROM mikebrady/shairport-sync:latest AS shairport

# Runtime stage with HA base
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Install runtime dependencies
# hadolint ignore=DL3018
RUN apk add --no-cache \
        dbus \
        alsa-lib \
        alsa-plugins-pulse \
        pulseaudio \
        popt \
        openssl \
        soxr \
        avahi \
        libconfig \
        ffmpeg-libs \
        libplist \
        libsodium \
        libgcrypt \
        libuuid

# Copy shairport-sync binary and config from official image
COPY --from=shairport /usr/local/bin/shairport-sync /usr/local/bin/shairport-sync
COPY --from=shairport /usr/local/etc/shairport-sync.conf /etc/shairport-sync.conf

# Copy NQPTP if it exists in the official image (needed for AirPlay 2)
COPY --from=shairport /usr/local/bin/nqptp /usr/local/bin/nqptp

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="WangChuDi" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Addons" \
    org.opencontainers.image.authors="WangChuDi" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
