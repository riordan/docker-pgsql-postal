FROM postgres:9.6


# Don't use this in production. It's my first "real" docker image.
# Not to mention Libpostal's stable but "new" and might change (i.e.
# add a more deliberate versioning scheme of the code + the data)
# and pgsql-postal is also subject to change. So yeah...
# Consider this "radically experimental".
MAINTAINER David Riordan <dr@daveriordan.com>

# LIBPOSTAL
RUN apt-get update

# Install Libpostal dependencies
RUN apt-get install -y \
	git \
	make \
	curl \
	libsnappy-dev \
	autoconf \
	automake \
	libtool \
	pkg-config
# Download libpostal source to /usr/local/libpostal
RUN cd /usr/local && \
	git clone https://github.com/openvenues/libpostal

# Create Libpostal data directory at /var/libpostal/data
RUN cd /var && \
	mkdir libpostal && \
	cd libpostal && \
	mkdir data

# Install Libpostal from source
RUN cd /usr/local/libpostal && \
	./bootstrap.sh && \
	./configure --datadir=/var/libpostal/data && \
	make && \
	make install && \
    make check

## PGSQL-POSTAL -- Bindings
# Download bindings - pgsql-postal - from source to /usr/lib/pgsql-postal
RUN cd /usr/lib && \
	git clone https://github.com/openvenues/pgsql-postal.git

# Download dependencies
RUN apt-get install -y postgresql-server-dev-9.6

RUN cd /usr/lib/pgsql-postal && git checkout 1.0 &&\
	make && \
	make install

## Attempting this with appropriate environment cleanup. We'll call this "security".
## But it will be insufficient. 
RUN apt-get remove -y \
	postgresql-server-dev-9.6 \
	pkg-config \
	libtool \
	automake \
	autoconf \
	git \
	make \
	curl

run apt-get autoclean
