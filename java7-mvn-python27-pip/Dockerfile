FROM buildpack-deps:jessie

ENV LANG C.UTF-8
ENV JAVA_VERSION 7u85
ENV JAVA_DEBIAN_VERSION 7u85-2.6.1-6+deb8u1

ENV PYTHON_VERSION 2.7.10
ENV PYTHON_PIP_VERSION 7.1.2

# Install Java & maven
RUN set -x && \
  export DEBIAN_FRONTEND=noninteractive && \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y unzip openjdk-7-jdk="$JAVA_DEBIAN_VERSION" maven && \
  apt-get purge -y python.* && \
  apt-get autoremove -y && \
  rm -rf /var/lib/apt/lists/*

# Install Python & Setuptools
RUN set -x && \
  mkdir -p /usr/src/python && \
  curl -SL "https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz" -o python.tar.xz && \
  tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz && \
  rm python.tar.xz* && \
  cd /usr/src/python && \
  ./configure --enable-shared --enable-unicode=ucs4 && \
  make -j$(nproc) && \
  make install && \
  ldconfig && \
  curl -SL 'https://bootstrap.pypa.io/get-pip.py' | python2 && \
  pip install --no-cache-dir --upgrade pip==$PYTHON_PIP_VERSION && \
  find /usr/local \
		\( -type d -a -name test -o -name tests \) \
		-o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
		-exec rm -rf '{}' + && \
  rm -rf /usr/src/python

# some modules would not be built, and most of them are platform-dependent, undocumented, or deprecated.
# _bsddb             _tkinter           bsddb185
# dbm                dl                 gdbm
# imageop            sunaudiodev