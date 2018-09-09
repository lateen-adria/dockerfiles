#Maintainer lateen.adria@protonmail.com
#"Deluge"
#"Deluge Daemon"

FROM ubuntu:18.04

ENV \
  TZ=Europe/Berlin \
  APP_VERSION="1.3.15" \
  APP_ARCH="amd64" \
  APP_DIR="/opt/deluge" \
  APP_USER="deluge" \
  APP_GROUP="deluge" \
  APP_UID="1000" \
  APP_GID="1000" \
  APP_RPC_PORT="58846" \
  APP_INCOMING_PORT="50500" \
  APP_INCOMING_PORT_UDP="50500/udp" 

RUN \
set -x && \
mkdir \
  --parents --verbose \
    "$APP_DIR" \
    "$APP_DIR/config" \
    "$APP_DIR/complete" \
    "$APP_DIR/downloads" \
    "$APP_DIR/logs" && \
addgroup \
  --system \
  --gid "$APP_GID" \
    "$APP_GROUP" && \
adduser \
  --system \
  --home "$APP_DIR" \
  --no-create-home \
  --uid "$APP_UID" \
  --gecos "Deluge system user" \
  --ingroup "$APP_GROUP" \
  --disabled-login \
    "$APP_USER"

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
  apt-get install -y \
    tzdata gnupg-utils && \
  apt-key adv \
    --keyserver keyserver.ubuntu.com --recv-keys 249AD24C && \
  echo "deb http://ppa.launchpad.net/deluge-team/ppa/ubuntu bionic main " >> /etc/apt/sources.list.d/deluge.list && \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
  apt-get install -y \
    deluged deluge-console && \
  apt-get --purge remove -y \
    gnupg-utils && \
  apt-get --purge autoremove -y && \
  rm -rf \
    /tmp/* \
    /var/tmp/* \
    /var/lib/apt/lists/* && \
  chown -R \
    "$APP_USER":"$APP_GROUP" \
    "$APP_DIR"

RUN \
  /usr/bin/deluged \
    -c ${APP_DIR}/config \
    -l ${APP_DIR}/config/deluged-init.log \
    -L debug && \
  sleep 10 && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s allow_remote true" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s move_completed_path ${APP_DIR}/complete" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s download_location ${APP_DIR}/downloads" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s daemon_port ${APP_RPC_PORT}" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s random_port false" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "config -s listen_ports (${APP_INCOMING_PORT},${APP_INCOMING_PORT})" && \
  /usr/bin/deluge-console \
    -c ${APP_DIR}/config "halt" && \
  rm -rf \
    ${APP_DIR}/config/ssl/* && \
  chown -R \
    ${APP_USER}:${APP_GROUP} \
    ${APP_DIR}

EXPOSE \
  ${APP_RPC_PORT} \
  ${APP_INCOMING_PORT} \
  ${APP_INCOMING_PORT_UDP}

USER \
  ${APP_UID}:${APP_GID}
ENTRYPOINT \
  ["deluged", "-d"]
CMD \
  ["-c", "/opt/deluge/config", "-l", "/opt/deluge/config/deluged.log", "-L", "info"]

LABEL \
  maintainer="lateen.adria@protonmail.com" \
  image.name="stable" \
  git.branch="master" \
  from="ubuntu:18.04" \
  c_software_name="Deluge" \
  c_software_url="http://deluge-torrent.org/" \
  image.version="0.1" \
  date.version="9.9.2018" \
  web_interface="true" \
  web_interface_port=${APP_RPC_PORT} \
  exposed_ports=${APP_RPC_PORT},${APP_INCOMING_PORT},${APP_INCOMING_PORT_UDP}  \
  docker_volumes=${APP_DIR}