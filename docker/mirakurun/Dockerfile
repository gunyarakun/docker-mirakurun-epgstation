FROM ubuntu:22.04
WORKDIR /app
ENV DOCKER=YES NODE_ENV=production
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        make \
        gcc \
        g++ \
        pkg-config \
        pcscd \
        libpcsclite-dev \
        libccid \
        libdvbv5-dev \
        pcsc-tools \
        dvb-tools \
        curl \
        git \
        build-essential libproc-processtable-perl \
        linux-headers-$(uname -r) kmod \
        gnupg

# Install Driver for Digital Devices DVB cards
RUN curl -OL https://github.com/DigitalDevices/dddvb/archive/0.9.38.tar.gz && tar xf 0.9.38.tar.gz
RUN cd dddvb-0.9.38 && make && make install

# Install Node.js 18
ENV NODE_MAJOR=18
RUN mkdir -p /etc/apt/keyrings && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Mirakurun from npm
RUN npm i mirakurun

# Copy a launch script
COPY ./docker/mirakurun/container-init.sh /app
CMD ["./container-init.sh"]

EXPOSE 40772 9229


