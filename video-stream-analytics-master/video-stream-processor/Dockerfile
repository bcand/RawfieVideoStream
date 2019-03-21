#FROM python:3.5.6-stretch AS Rawfie
#FROM buildpack-deps:stretch AS Rawfie
FROM debian:stretch-slim AS Rawfie

MAINTAINER bcand


# Libs	

RUN apt-get update \
	&& apt-get install -y \
		apt-utils \
		build-essential \
       	cmake \
      	git \
       	wget \
       	unzip \
       	yasm \
       	pkg-config \
       	libswscale-dev \
       	libtbb2 \
       	libtbb-dev \
       	libjpeg-dev \
       	libpng-dev \
       	libtiff-dev \
       	libavformat-dev \
       	libavcodec-dev \
       	libswscale-dev \
       	libv4l-dev \
       	libxvidcore-dev \
       	libx264-dev \
       	libatlas-base-dev \
       	gfortran \
       	libpq-dev \
       	openjdk-8-jdk \
		ant \
		maven \
		scala \
    && rm -rf /var/lib/apt/lists/*


# Install Java 8



# Install Python 2.7

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8
# https://github.com/docker-library/python/issues/147
ENV PYTHONIOENCODING UTF-8

# extra dependencies (over what buildpack-deps already includes)
RUN apt-get update && apt-get install -y --no-install-recommends \
		tk-dev \
	&& rm -rf /var/lib/apt/lists/*

ENV GPG_KEY C01E1CAD5EA2C4F0B8E3571504C367C218ADD4FF
ENV PYTHON_VERSION 2.7.16

RUN set -ex \
	\
	&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& { command -v gpgconf > /dev/null && gpgconf --kill all || :; } \
	&& rm -rf "$GNUPGHOME" python.tar.xz.asc \export \	
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-shared \
		--enable-unicode=ucs4 \
	&& make -j "$(nproc)" \
	&& make install \
	&& ldconfig \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' + \
	&& rm -rf /usr/src/python \
	\
	&& python2 --version

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 19.0.3

RUN set -ex; \
	\
	wget -O get-pip.py 'https://bootstrap.pypa.io/get-pip.py'; \
	\
	python get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		"pip==$PYTHON_PIP_VERSION" \
	; \
	pip --version; \
	\
	find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' +; \
	rm -f get-pip.py




# Install python 3.5

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# extra dependencies (over what buildpack-deps already includes)
RUN apt-get update && apt-get install -y --no-install-recommends \
		tk-dev \
	&& rm -rf /var/lib/apt/lists/*

ENV GPG_KEY 97FC712E4C024BBEA48A61ED3A5CA953F73C700D
ENV PYTHON_VERSION 3.5.6

RUN set -ex \
	\
	&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& { command -v gpgconf > /dev/null && gpgconf --kill all || :; } \
	&& rm -rf "$GNUPGHOME" python.tar.xz.asc \
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-loadable-sqlite-extensions \
		--enable-shared \
		--with-system-expat \
		--with-system-ffi \
		--without-ensurepip \
	&& make -j "$(nproc)" \
	&& make install \
	&& ldconfig \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' + \
	&& rm -rf /usr/src/python \
	\
	&& python3 --version

# make some useful symlinks that are expected to exist
#RUN cd /usr/local/bin \
#	&& ln -s idle3 idle \
#	&& ln -s pydoc3 pydoc \
#	&& ln -s python3 python \
#	&& ln -s python3-config python-config




	

RUN pip install numpy

WORKDIR /
ENV OPENCV_VERSION="3.2.0"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
	&& unzip ${OPENCV_VERSION}.zip \
	&& mv opencv-${OPENCV_VERSION} opencv \
	&& mkdir /opencv/build \
	&& cd /opencv/build \
	&& cmake -DBUILD_PERF_TESTS=OFF \
		-DBUILD_SHARED_LIBRARY=OFF \
		-DBUILD_TESTS=OFF \
		-DBUILD_opencv_python=OFF \
		-DCMAKE_BUILD_TYPE=RELEASE \
	 	.. \
	&& make -j8 \
	&& cp lib/libopencv_java320.so /usr/lib/x86_64-linux-gnu/ \
#	&& make install \
#	&& ldconfig
	&& rm /${OPENCV_VERSION}.zip 
#	&& rm -r /opencv 



# Install Spark
RUN wget http://apache.otenet.gr/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz \
	&& tar -xvzf spark-2.4.0-bin-hadoop2.7.tgz \
	&& mv spark-2.4.0-bin-hadoop2.7 /usr/local/ \
	&& ln -s /usr/local/spark-2.4.0-bin-hadoop2.7/ /usr/local/spark 

ENV SPARK_HOME /usr/local/spark

RUN mvn -v \
	&& java -version \
	&& echo $JAVA_HOME \
	&& python2 --version \ 
	&& python3 --version \
	&& ant -version \
	&& export ANT_HOME=/usr/share/ant \
	&& echo $ANT_HOME \
#	&& spark-submit --version \
	&& echo $SPARK_HOME \
	&& scala -version 
#	&& ps -ef|grep kafka

#RUN cp lib/libopencv_java320.so /usr/lib/x86_64-linux-gnu/
# mkdir /opencv-java-bin \
#	cp bin/opencv-320.jar lib/libopencv_java320.so /opencv-java-bin/ \

RUN mkdir Repo \
	&& cd Repo \
	&& git clone https://github.com/bcand/RawfieVideoStream.git \
	&& cd RawfieVideoStream/video-stream-analytics-master/video-stream-processor \
#	&& mvn clean \	
	&& mvn clean package exec:java -Dexec.mainClass="com.iot.video.app.spark.processor.VideoStreamProcessor"
#	-Djava.library.path=/opencv/build/lib	