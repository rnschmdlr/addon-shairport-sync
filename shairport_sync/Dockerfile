ARG BUILD_FROM=ghcr.io/hassio-addons/base:20.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM} AS builder

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install build dependencies (based on official shairport-sync Dockerfile)
# hadolint ignore=DL3018
RUN apk add --no-cache --virtual .build-deps \
        git \
        build-base \
        autoconf \
        automake \
        libtool \
        pkgconf \
        alsa-lib-dev \
        avahi-dev \
        dbus \
        libconfig-dev \
        libgcrypt-dev \
        libplist-dev \
        libsodium-dev \
        openssl-dev \
        popt-dev \
        pulseaudio-dev \
        soxr-dev \
        xxd \
 && cd /tmp \
 && git clone --depth 1 https://github.com/mikebrady/shairport-sync.git \
 && cd shairport-sync \
 && autoreconf -fi \
 && mkdir build && cd build \
 && ../configure \
        --prefix=/usr \
        --sysconfdir=/etc \
        --with-alsa \
        --with-pulseaudio \
        --with-pipe \
        --with-avahi \
        --with-ssl=openssl \
        --with-soxr \
        --with-metadata \
        --with-stdout \
 && make -j $(nproc) \
 && make install DESTDIR=/tmp/shairport-install

# Runtime stage
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Install runtime dependencies (matching official shairport-sync image)
# hadolint ignore=DL3018
RUN apk add --no-cache \
        alsa-lib \
        alsa-plugins-pulse \
        avahi \
        dbus \
        glib \
        libconfig \
        libgcrypt \
        libplist \
        libpulse \
        libcrypto3 \
        libsodium \
        libuuid \
        popt \
        soxr

# Copy shairport-sync binary and config from builder
COPY --from=builder /tmp/shairport-install/usr/bin/shairport-sync /usr/bin/shairport-sync
COPY --from=builder /tmp/shairport-install/etc/shairport-sync.conf /etc/shairport-sync.conf

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
