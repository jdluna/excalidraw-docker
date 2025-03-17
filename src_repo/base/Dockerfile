FROM debian:stretch-slim

ARG FREP_VERSION=1.3.11

ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

COPY rootfs /

RUN \
	echo "deb http://archive.debian.org/debian stretch main contrib non-free deb http://archive.debian.org/debian-security/ stretch/updates main contrib non-free deb http://archive.debian.org/debian/ stretch-backports main contrib non-free" > /etc/apt/sources.list && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y --allow-downgrades apt=1.4.10 apt-transport-https apt-utils ca-certificates gnupg && \
	apt-dpkg-wrap apt-get install -y wget && \
	wget -qO - https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-amd64.tar.gz | tar xfz - -C / && \
	wget -q https://github.com/subchen/frep/releases/download/v$FREP_VERSION/frep-$FREP_VERSION-linux-amd64 -O /usr/bin/frep && \
	apt-dpkg-wrap apt-get --purge remove -y wget && \
	echo "deb http://archive.debian.org/debian/ stretch-backports main" > /etc/apt/sources.list.d/backports.list && \
	echo "Acquire::Check-Valid-Until false;" | tee -a /etc/apt/apt.conf.d/10-nocheckvalid && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get dist-upgrade -y && \
	apt-cleanup && \
	chmod +x /usr/bin/frep

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jq procps curl vim iputils-ping net-tools && \
	apt-cleanup || \
	true

ENTRYPOINT [ "/init" ]
