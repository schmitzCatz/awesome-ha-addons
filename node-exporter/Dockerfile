ARG BUILD_FROM=ghcr.io/hassio-addons/base:16.3.1
FROM $BUILD_FROM

# Building
RUN apk update && apk add --no-cache --virtual .build-deps \
    lm-sensors \
    python3 \
    python3-dev \
    linux-headers \
    libc-dev \
    gcc \
    git \
    go \
    make

RUN git clone https://github.com/prometheus/node_exporter.git build

WORKDIR /build
RUN XDG_CACHE_HOME=. make build && apk del .build-deps && cp node_exporter / && cd / && rm -rf /build

# Run Stage
WORKDIR /
COPY run.sh /
RUN chmod a+x /run.sh

LABEL \
  io.hass.version="1.0.0" \
  io.hass.type="addon" \
  io.hass.arch="armhf|aarch64|i386|amd64"

CMD [ "/run.sh" ]