FROM raspbian-jessie
MAINTAINER Paul Moore <paul@paul-moore.com>

# TODO: move the ozwcp setup to a script which we ADD to the container

# configuration directory
RUN mkdir -p /srv/config
VOLUME /srv/config

# working directory
RUN mkdir -p /srv/workdir

# build directory
RUN mkdir -p /srv/builddir
WORKDIR /srv/builddir

# bust the cache and update the base system
RUN date > /tmp/cache_busting.timestamp
RUN apt-get update && \
	apt-get upgrade -y --no-install-recommends

# install system tools
RUN apt-get install -y --no-install-recommends wget git sed gawk

# install python3 basics
RUN apt-get install -y --no-install-recommends python3 python3-pip

# install openzwave
#  -> https://github.com/OpenZWave/open-zwave
RUN apt-get install -y --no-install-recommends make gcc g++ libudev-dev
RUN git clone https://github.com/OpenZWave/open-zwave.git
RUN (cd open-zwave; make)

# install openzwave-controlpanel
#  -> https://github.com/OpenZWave/open-zwave-control-panel
RUN apt-get install -y --no-install-recommends \
        libgnutls-deb0-28 libgnutlsxx28 libgnutls28-dev \
        libmicrohttpd10 libmicrohttpd-dev
RUN git clone https://github.com/OpenZWave/open-zwave-control-panel.git
ADD https://raw.githubusercontent.com/OpenZWave/open-zwave-control-panel/c243d3722e81ee69d400c36d01e10a372f10c8bd/Makefile open-zwave-control-panel
RUN (cd open-zwave-control-panel; sed -e 's/^OPENZWAVE := .*/OPENZWAVE := ..\/open-zwave\//' -i Makefile)
RUN (cd open-zwave-control-panel; make)
RUN (cd open-zwave-control-panel; ln -s ../open-zwave/config config)

# cleanup
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*

# start OpenZWave Control Panel (ozwcp)
CMD (cd open-zwave-control-panel; ./ozwcp -p 8080) 

