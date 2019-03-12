FROM wurstmeister/kafka AS kafka
FROM mesosphere/spark AS spark
FROM python:3.5 AS builder1
FROM python:2.7 AS builder2

MAINTAINER bcand

RUN apt-get update \
	&& apt-get install -y \
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
       	libpq-dev \
		openjdk-8-jdk \
		maven \
    && rm -rf /var/lib/apt/lists/*

RUN pip install numpy

WORKDIR /
ENV OPENCV_VERSION="3.2.0"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
	&& unzip ${OPENCV_VERSION}.zip \
	&& mkdir /opencv-${OPENCV_VERSION}/cmake_binary \
	&& cd /opencv-${OPENCV_VERSION}/cmake_binary \
	&& cmake -DBUILD_TIFF=ON \
	  -DBUILD_opencv_java=ΟΝ \
	  -DWITH_CUDA=OFF \
	  -DWITH_OPENGL=ON \
	  -DWITH_OPENCL=ON \
	  -DWITH_IPP=ON \
	  -DWITH_TBB=ON \
	  -DWITH_EIGEN=ON \
	  -DWITH_V4L=ON \
	  -DBUILD_TESTS=OFF \
	  -DBUILD_PERF_TESTS=OFF \
	  -DCMAKE_BUILD_TYPE=RELEASE \
	  -DCMAKE_INSTALL_PREFIX=$(python3.5 -c "import sys; print(sys.prefix)") \
	  -DPYTHON_EXECUTABLE=$(which python3.5) \
	  -DPYTHON_INCLUDE_DIR=$(python3.5 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
	  -DPYTHON_PACKAGES_PATH=$(python3.5 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
	  .. \
	&& make install \
	&& rm /${OPENCV_VERSION}.zip \
	&& rm -r /opencv-${OPENCV_VERSION} 

RUN mvn -v \
	&& java -version \
	&& python2 --version \ 
	&& python3 --version \
	&& ps -ef|grep kafka

RUN mkdir Repo \
	&& cd Repo \
	&& git clone https://github.com/bcand/RawfieVideoStream.git \
	&& cd RawfieVideoStream/video-stream-analytics-master \
	&& mvn clean install \
	&& mvn clean package exec:java -Dexec.mainClass="com.iot.video.app.kafka.collector.VideoStreamCollector" -Dexec.cleanupDaemonThreads=false 
	