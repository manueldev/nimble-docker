FROM ubuntu:jammy
MAINTAINER Sergei Ovchinnikov (https://wmspanel.com/help)
LABEL	description="Nimble Streamer with SRT"

RUN	apt-get update -y
RUN	apt-get install wget gnupg sudo vim less -y

ENV	TZ=Asia/Vladivostok
RUN	ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN	wget -q -O - http://nimblestreamer.com/gpg.key | sudo tee /etc/apt/trusted.gpg.d/nimble.asc
RUN	apt-get update -y
RUN	sudo bash -c 'echo -e "deb http://nimblestreamer.com/ubuntu jammy/" > /etc/apt/sources.list.d/nimble.list' \
	&& sudo apt-get update -y
RUN	sudo apt-get install nimble nimble-srt-1.5 -y

ARG	WMSPANEL_SERVER_NAME=SRT-MACOS
ARG	WMSPANEL_ACCOUNT=
ARG	WMSPANEL_PASS=

RUN	sudo /usr/bin/nimble_regutil -u $WMSPANEL_ACCOUNT -p $WMSPANEL_PASS --server-name $WMSPANEL_SERVER_NAME --host nimble.wmspanel.com
RUN	sudo echo "management_listen_interfaces = *" >> /etc/nimble/nimble.conf 

RUN	sudo mkdir -p /var/run/nimble

#EXPOSE 8081 1935 554 4444/udp
ENTRYPOINT	["/usr/bin/nimble", "--conf-dir=/etc/nimble", "--log-dir=/var/log/nimble","--pidfile=/var/run/nimble/nimble.pid"]
