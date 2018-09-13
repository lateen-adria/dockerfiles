#Maintainer lateen.adria@protonmail.com
#Sonarr
#Sonarr PVR

FROM ubuntu:18.04

ENV \
  TZ="Europe/Berlin" \
  APP_ARCH="amd64" \
  APP_DIR="/opt/sonarr" \
  APP_USER="sonarr" \
  APP_GROUP="sonarr" \
  APP_UID="1000" \
  APP_GID="1000" \
  APP_HTTP_PORT="8989"

RUN \
  set -x && \
  mkdir \
    --parents --verbose \
      ${APP_DIR} && \  
  addgroup \
    --system \
    --gid ${APP_GID} \
      ${APP_GROUP} && \
  adduser \
    --system \
    --home ${APP_DIR} \
    --no-create-home \
    --uid ${APP_UID} \
    --gecos "Deluge system user" \
    --ingroup ${APP_GROUP} \
    --disabled-login \
      ${APP_USER}

RUN \
  set -x && \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
  apt-get install -y \
    tzdata gnupg-utils && \
  apt-key adv \
    --keyserver keyserver.ubuntu.com --recv-keys FDA5DFFC && \
  echo "deb http://apt.sonarr.tv/ master main" >> /etc/apt/sources.list.d/sonarr.list && \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
  apt-get install -y \
    libcurl3 nzbdrone && \
  apt-get --purge remove -y \
    gnupg-utils && \
  apt-get --purge autoremove -y && \
  rm -rf \
    /tmp/* \
    /var/tmp/* \
    /var/lib/apt/lists/* && \
  chown -R \
    ${APP_USER}:${APP_GROUP} ${APP_DIR}

EXPOSE \
  ${APP_HTTP_PORT}

USER \
  ${APP_UID}:${APP_GID}
ENTRYPOINT \
  ["/usr/bin/mono"]
CMD \
  ["/opt/NzbDrone/NzbDrone.exe","-nobrowser","-data=/opt/sonarr/config"]

LABEL \
  maintainer="lateen.adria@protonmail.com" \
  image.name="stable" \
  git.branch="master" \
  from="ubuntu:18.04" \
  c_software_name="sonarr" \
  c_software_url="https://sonarr.tv" \
  image.version="0.1" \
  date.version="13.9.2018" \
  web_interface="true" \
  web_interface_port=${APP_STATUS_PORT} \
  exposed_ports=${APP_STATUS_PORT},${APP_HTTP_PORT},${APP_HTTPS_PORT}  \
  docker_volumes=${APP_DIR}