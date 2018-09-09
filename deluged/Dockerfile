#Maintainer lateen.adria@protonmail.com
#"App Name"
#"App Description"

FROM ubuntu:18.04

ENV \
  TZ=Europe/Berlin \
  APP_VERSION="0.0" \
  APP_ARCH="amd64" \
  APP_DIR="/opt/app" \
  APP_USER="app" \
  APP_GROUP="app" \
  APP_UID="1000" \
  APP_GID="1000" \
  APP_STATUS_PORT="8080" \
  APP_HTTP_PORT="80" \
  APP_HTTPS_PORT="443" 

RUN \
  mkdir \
    --parents --verbose \
      ${APP_DIR} \
      ${APP_DIR}/config \
      ${APP_DIR}/logs && \
  addgroup \
    --system \
    --gid ${APP_GID} \
      ${APP_GROUP} && \
  adduser \
    --system \
    --home ${APP_DIR} \
    --no-create-home \
    --uid ${APP_UID} \
    --gecos "App system user" \
    --ingroup ${APP_GROUP} \
    --disabled-login \
      ${APP_USER}

RUN \
  apt-get update && apt-get install -y \
    wget && \
  wget -O /usr/local/bin/app "https://github.com/user/repo/releases/download/v${APP_VERSION}/app_linux-${APP_ARCH}" && \
  chmod +x /usr/local/bin/app && \
  apt-get remove -y \
    wget && \
  rm -rf \
    /tmp/* \
    /var/tmp/* \
    /var/lib/apt/lists/* && \
  chown -R \
    ${APP_USER}:${APP_GROUP} \
    ${APP_DIR}

EXPOSE \
  ${APP_STATUS_PORT} \
  ${APP_HTTP_PORT} \
  ${APP_HTTPS_PORT}

USER \
  ${APP_UID}:${APP_GID}
ENTRYPOINT \
  ["/usr/local/bin/app"]
CMD \
  ["--configfile=/opt/", "--accesslog.filepath=/opt/"]

LABEL \
  maintainer="lateen@adria@protonmail.com" \
  image.name="stable" \
  git.branch="master" \
  from="ubuntu:18.04" \
  c_software_name="app" \
  c_software_url="https://app.io/" \
  image.version="0.0" \
  date.version="0.0.2018" \
  web_interface="true" \
  web_interface_port=${APP_STATUS_PORT} \
  exposed_ports=${APP_STATUS_PORT},${APP_HTTP_PORT},${APP_HTTPS_PORT}  \
  docker_volumes=${APP_DIR}