FROM         --platform=$TARGETOS/$TARGETARCH debian:bullseye-slim

LABEL        author="Michael Parker" maintainer="parker@pterodactyl.io"

LABEL        org.opencontainers.image.source="https://github.com/pterodactyl/yolks"
LABEL        org.opencontainers.image.licenses=MIT

ENV          DEBIAN_FRONTEND noninteractive

RUN          useradd -m -d /home/container -s /bin/bash container

RUN          ln -s /home/container/ /nonexistent

ENV          USER=container HOME=/home/container

## Update base packages
RUN          apt update && apt upgrade -y

## Install dependencies
RUN          apt install -y gcc g++ libgcc1 libc++-dev gdb libc6 git wget curl tar zip unzip binutils xz-utils liblzo2-2 cabextract iproute2 net-tools netcat telnet libatomic1 libsdl1.2debian libsdl2-2.0-0 \
             libfontconfig libicu67 icu-devtools libunwind8 libssl-dev sqlite3 libsqlite3-dev libmariadbclient-dev-compat libduktape205 locales ffmpeg gnupg gnupg2 apt-transport-https software-properties-common ca-certificates \
             liblua5.3-0 libz-dev rapidjson-dev tzdata libevent-dev libzip4 libprotobuf23 libfluidsynth2 procps sudo
			 
# Add Docker to Debian Image
RUN          mkdir -m 0755 -p /etc/apt/keyrings
RUN          curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN          echo \
             "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
             $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
	     
RUN	     chmod a+r /etc/apt/keyrings/docker.gpg
	     
## Update packages
RUN          apt update
	     
RUN	     apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

## Configure locale
RUN          update-locale lang=en_US.UTF-8 \
             && dpkg-reconfigure --frontend noninteractive locales
     
RUN          usermod -aG docker container && newgrp docker
RUN          usermod -aG sudo container && newgrp container
RUN          echo 'container:container' | chpasswd
RUN          echo 'container ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh
CMD         [ "/bin/bash", "/entrypoint.sh" ]
