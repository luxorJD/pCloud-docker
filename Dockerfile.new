FROM ubuntu:focal as builder
ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/Berlin"
RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends cmake g++ git libboost-program-options-dev \
    libboost-system-dev libcurl4-gnutls-dev libfuse-dev libudev-dev make zlib1g-dev libpthread-stubs0-dev libmbedtls-dev \
    gcc libsqlite3-dev make pkg-config libreadline-dev \
    && apt-get install -y --reinstall ca-certificates \
    && cd /usr/src \
    && git clone https://github.com/luxorJD/pcloud-console-client \
    && cd pcloud-console-client \
    && git submodule init \
    && git submodule update \
    #&& ls -laF \
    && mkdir build \
    #&& pwd \
    #&& ls -laF \
    && cd /usr/src/pcloud-console-client/build \
    #&& pwd \
    #&& ls -laF \
    && cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr .. \
    && cmake --build . --target install 
        

FROM ubuntu:focal as base
RUN apt-get update \
    && apt-get install -y --no-install-recommends fuse lsb-release nano \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/bin/pcloudcc /usr/bin/pcloudcc
# COPY --from=builder /usr/lib/libpcloudcc_lib.so /usr/lib/libpcloudcc_lib.so

COPY fuse.conf /etc/fuse.conf

# Add startup script
COPY start.sh /

# Start Ganesha NFS daemon by default
CMD ["/start.sh"]
