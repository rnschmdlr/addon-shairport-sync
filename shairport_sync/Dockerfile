ARG BUILD_FROM=ghcr.io/hassio-addons/base:20.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM} AS builder

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install build dependencies
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
        mosquitto-dev \
        openssl-dev \
        popt-dev \
        pulseaudio-dev \
        soxr-dev \
        xxd

# Build plistutil from source — required for AirPlay 2 configure check;
# not shipped as a standalone binary in Alpine's libplist packages.
# hadolint ignore=DL3003
RUN cd /tmp \
 && git clone --depth 1 https://github.com/libimobiledevice/libplist.git \
 && cd libplist \
 && autoreconf -fi \
 && ./configure --without-cython \
 && make -j $(nproc) \
 && install -m 755 src/plistutil /usr/local/bin/plistutil

RUN cd /tmp \
 && git clone --branch 5.0.1 --depth 1 https://github.com/mikebrady/shairport-sync.git \
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
        --with-mqtt-client \
        --with-airplay-2 \
        --with-apple-alac \
        --with-stdout \
 && make -j $(nproc) \
 && make install DESTDIR=/tmp/shairport-install

# Build nqptp (required for AirPlay 2)
RUN cd /tmp \
 && git clone --branch 1.2.6 --depth 1 https://github.com/mikebrady/nqptp.git \
 && cd nqptp \
 && autoreconf -fi \
 && ./configure \
 && make -j $(nproc) \
 && cp nqptp /tmp/shairport-install/usr/bin/nqptp

# Runtime stage
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Install runtime dependencies
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
        mosquitto-libs \
        popt \
        soxr

# Copy shairport-sync, nqptp, and default config from builder
COPY --from=builder /tmp/shairport-install/usr/bin/shairport-sync /usr/bin/shairport-sync
COPY --from=builder /tmp/shairport-install/usr/bin/nqptp /usr/bin/nqptp
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
