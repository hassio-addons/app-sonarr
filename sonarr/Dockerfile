ARG BUILD_FROM=ghcr.io/hassio-addons/base:20.1.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
ARG SONARR_VERSION=4.0.17.2952
ARG BUILD_ARCH=amd64
ARG BUILD_VERSION=dev
ENV XDG_CONFIG_HOME="/config/xdg"
# hadolint ignore=DL3003,DL3042
RUN \
  apk add --no-cache \
    icu-libs=76.1-r1 \
    sqlite-libs=3.51.2-r0 \
    xmlstarlet=1.6.1-r2 \
  \
  && if [ "${BUILD_ARCH}" = "aarch64" ]; then SONARR_ARCH="arm64"; \
  elif [ "${BUILD_ARCH}" = "amd64" ]; then SONARR_ARCH="x64"; fi \
  \
  && curl -J -L -o /tmp/sonarr.tar.gz \
      "https://github.com/Sonarr/Sonarr/releases/download/v${SONARR_VERSION}/Sonarr.main.${SONARR_VERSION}.linux-musl-${SONARR_ARCH}.tar.gz" \
  && mkdir -p /opt/bin \
  && tar zxvf \
      /tmp/sonarr.tar.gz \
      --strip 1 -C /opt/bin \
  \
  && echo -e "UpdateMethod=docker\nBranch=master\nPackageVersion=${BUILD_VERSION}\nPackageAuthor=Home Assistant Community Apps" \
    > /opt/package_info \
  \
  && rm -f -r \
    /tmp/* \
    /opt/bin/Sonarr.Update

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <opensource@frenck.dev>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Apps" \
    org.opencontainers.image.authors="Franck Nijhof <opensource@frenck.dev>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://frenck.dev/home-assistant-apps" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
