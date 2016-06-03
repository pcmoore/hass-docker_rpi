FROM raspbian-jessie
MAINTAINER Paul Moore <paul@paul-moore.com>

# update the base system
RUN apt-get update && apt-get upgrade -y

# configuration directory
RUN mkdir -p /srv/config
VOLUME /srv/config

# working directory
RUN mkdir -p /srv/workdir

# build directory
RUN mkdir -p /srv/builddir
WORKDIR /srv/builddir

# install system tools
RUN apt-get install -y --no-install-recommends wget git sed gawk

# install python3 basics
RUN apt-get install -y --no-install-recommends python3 python3-pip

# install openzwave
#  -> https://github.com/OpenZWave/open-zwave
RUN apt-get install -y --no-install-recommends make gcc g++
RUN apt-get install -y --no-install-recommends libudev-dev
RUN git clone https://github.com/OpenZWave/open-zwave.git
RUN (cd open-zwave; make)

# install openzwave-controlpanel
#  -> https://github.com/OpenZWave/open-zwave-control-panel
RUN apt-get install -y --no-install-recommends make gcc g++
RUN apt-get install -y --no-install-recommends \
        libgnutls-deb0-28 libgnutlsxx28 libgnutls28-dev \
        libmicrohttpd10 libmicrohttpd-dev
RUN git clone https://github.com/OpenZWave/open-zwave-control-panel.git
RUN (cd open-zwave-control-panel; sed -e 's/^OPENZWAVE := .*/OPENZWAVE := ..\/open-zwave\//' -i Makefile)
RUN (cd open-zwave-control-panel; make)
RUN (cd open-zwave-control-panel; ln -s ../open-zwave/config config)

# start OpenZWave Control Panel (ozwcp)
CMD (cd open-zwave-control-panel; ./ozwcp -p 8080) 
