FROM debian:buster-slim as builder

LABEL author="Miles Smith <miles-smith@omrf.org>"

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update --fix-missing && \
    apt-get install --no-install-recommends -y \
        git \
        zlib1g-dev \
        libhts-dev \
        dh-autoreconf \
        libbz2-dev \
        liblzma-dev \
        libcurl4-openssl-dev \
        libcrypto++-dev \
        libssl-dev \
        gcc && \
    cp /usr/share/zoneinfo/America/Chicago /etc/localtime && \
    apt-get clean && \
    rm -rf /tmp/downloaded_packages/* && \
    rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/samtools/htslib /opt/htslib
WORKDIR /opt/htslib
RUN autoheader && autoconf && \
    ./configure --prefix=/opt/htslib && \
    make && \
    make install

RUN git clone https://github.com/milescsmith/demuxlet /opt/demuxlet-git
WORKDIR /opt/demuxlet-git
RUN autoreconf -vfi && \
    ./configure --prefix=/opt/demuxlet && \
    make && \
    make install && \
    rm -fr /opt/demuxlet-git


RUN groupadd -r sinnombre && useradd -r -s /bin/false -g sinnombre sinnombre
RUN chown -R sinnombre:sinnombre /opt

ENTRYPOINT ["/opt/demuxlet/bin/demuxlet"]
CMD []
