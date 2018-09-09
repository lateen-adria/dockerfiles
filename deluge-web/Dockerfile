#Maintainer lateen.adria@protonmail.com
#Deluge
#Deluge web interface


FROM ubuntu:18.04

ENV \
  TZ=Europe/Ljubljana \
  APP_VERSION="1.3.15" \
  APP_ARCH="amd64" \
  APP_DIR="/opt/deluge" \
  APP_USER="deluge" \
  APP_GROUP="deluge" \
  APP_UID="1000" \
  APP_GID="1000" \
  APP_WEB_PORT="8112" \
  APP_W_LOG_LEVEL="info"

RUN \
  set -x && \
  mkdir \
    --parents --verbose \
      "$APP_DIR" \
      "$APP_DIR/config" && \
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
    deluge-web && \
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

EXPOSE \
  ${APP_WEB_PORT}

USER \
  ${APP_UID}:${APP_GID}
ENTRYPOINT \
  ["/usr/bin/deluge-web"]
CMD \
  ["-c", "/opt/deluge/config", "-l", "/opt/deluge/config/deluge-web.log", "-L", "info"]

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
  web_interface_port="$APP_WEB_PORT" \
  exposed_ports="$APP_WEB_PORT"  \
  docker_volumes="$APP_DIR"