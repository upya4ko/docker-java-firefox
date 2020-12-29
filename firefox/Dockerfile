FROM  ubuntu:14.04
#FROM debian:9
LABEL com.local.version="0.0.2"
LABEL com.local.is-production="True"

ARG appPath=/usr/src/app
ARG appUser=vncserver

RUN apt update \
  && apt install -y \
  sudo \
  tar \
  xz-utils \
  openbox \
  xterm \
  x11vnc \
  xvfb \
  wget \
  tzdata \
  tzdata-java \
#  firefox-esr \
  firefox \
  libbz2-1.0 \
  icedtea-6-plugin \
  icedtea-netx \
  openjdk-6-jre \
  openjdk-6-jre-headless \
#  openjdk-8-jdk \
  && apt-get clean \
  && rm -rf /var/cache/* \
  && rm -rf /var/lib/apt/lists/*

RUN useradd --home-dir $appPath \
    --create-home \
    $appUser

# Need for debug
#RUN echo "$appUser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$appUser

COPY mozilla $appPath/.mozilla/
RUN chown -R $appUser: $appPath

WORKDIR $appPath
USER $appUser

RUN wget https://ftp.mozilla.org/pub/firefox/releases/51.0/firefox-51.0.linux-x86_64.sdk.tar.bz2 \
    && tar -xf firefox-51.0.linux-x86_64.sdk.tar.bz2 \
    && rm firefox-51.0.linux-x86_64.sdk.tar.bz2

RUN echo "exec openbox-session & \n \
         ~/firefox-sdk/bin/firefox \
         " > ~/.xinitrc \
         && chmod 755 ~/.xinitrc

ENV USER=$appUser \
    HOME=$appPath \
    DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    LC_ALL=C.UTF-8 \
    DISPLAY=:0.0 \
    RUN_XTERM=no \
    RUN_FLUXBOX=no \
#    X11VNC_CREATE_GEOM=1475x770x16
    X11VNC_CREATE_GEOM=1475x700x16

EXPOSE 5900

#CMD ["/bin/bash"]
CMD ["/usr/bin/x11vnc",\
     "-forever", \
     "-shared", \
     "-sloppy_keys", \
     "-clear_mods", \
     "-desktop", "Java-Firefox",\
     "-create"]


