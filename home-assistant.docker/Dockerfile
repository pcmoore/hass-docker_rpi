FROM raspbian-jessie
MAINTAINER Paul Moore <paul@paul-moore.com>

# TODO: move the python-openzwave setup/build to a script

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
RUN apt-get install -y --no-install-recommends wget git unzip sed

# install python3 basics
RUN apt-get install -y --no-install-recommends python3 python3-pip

# install python-openzwave
#  -> https://github.com/OpenZWave/python-openzwave
RUN apt-get install -y --no-install-recommends make gcc g++ libudev-dev
ADD https://github.com/OpenZWave/python-openzwave/archive/master.zip .
RUN unzip -q master.zip
RUN basename $(ls -1 python-openzwave-master/archives/python-openzwave-* | sort -V | tail -n 1) | sed -e 's/.tgz$//' > python-openzwave.version
RUN tar zxf python-openzwave-master/archives/$(cat python-openzwave.version).tgz
RUN (cd $(cat python-openzwave.version); PYTHON_EXEC=$(which python3) make deps)
RUN (cd $(cat python-openzwave.version); PYTHON_EXEC=$(which python3) make build)
RUN (cd $(cat python-openzwave.version); PYTHON_EXEC=$(which python3) make install)
RUN rm -rf master.zip python-openzwave-master $(cat python-openzwave.version)

# install Home Assistant
#  -> https://home-assistant.io
RUN pip3 install homeassistant

# cleanup
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*

# start Home Assistant (hass)
WORKDIR /srv/workdir
CMD /usr/local/bin/hass --config /srv/config

