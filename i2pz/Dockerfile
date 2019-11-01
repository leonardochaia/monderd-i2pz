FROM ubuntu:18.04 as build
ARG I2PZ_BRANCH=v1.13

# https://github.com/i2p-zero/i2p-zero#building-the-launchers
RUN apt-get update && apt-get -y install build-essential \
    git zip unzip bzip2 curl wget

WORKDIR /i2pz

RUN git clone --recursive -b ${I2PZ_BRANCH} \
    https://github.com/knaccc/i2p-zero.git . \
    && git submodule init && git submodule update

RUN ./bin/build-all-and-zip.sh

FROM ubuntu:18.04

RUN useradd -ms /bin/bash i2pz && mkdir -p /home/i2pz/ \
    && chown -R i2pz:i2pz /home/i2pz/

WORKDIR /home/i2pz

COPY --chown=i2pz:i2pz --from=build /i2pz/dist/linux/router /etc/i2pz/

RUN ln -s /etc/i2pz/bin/tunnel-control.sh /usr/bin/tunnel-control

USER i2pz
ENTRYPOINT [ "/etc/i2pz/bin/launch.sh" ]