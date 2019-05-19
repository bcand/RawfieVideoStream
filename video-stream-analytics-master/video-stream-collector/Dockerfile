#FROM debian:stretch-slim AS Rawfie
FROM debian:stretch AS Rawfie
#FROM balenalib/raspberrypi3-debian

MAINTAINER bcand

# Install Java 8
RUN apt-get update \
	&& apt-get install -y \
       	openjdk-8-jdk \
       	python \
       	python3 \
       	python-setuptools \
       	python-pip \
#       	python3 \
    && rm -rf /var/lib/apt/lists/*
    
#ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-armhf

# Libs	

RUN apt-get update \
	&& apt-get install -y \
		build-essential \
       	cmake \
		pkg-config \
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
		ant \
		maven \
    && rm -rf /var/lib/apt/lists/*
    
    


# Install Python 2.7





# Install python 3.5

	
# Install python packages
#RUN pip install --no-cache-dir \
    # OpenCV dependency
#    numpy \
    # other usefull stuff
#   ipython \
    # cleanup
#    && find /usr/local \
#       \( -type d -a -name test -o -name tests \) \
#       -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
#       -exec rm -rf '{}' + \
#    && cd / \
#    && rm -rf /usr/src/python ~/.cache


RUN pip install numpy
#pip install wheel \
#	&& python setup.py bdist_wheel \
	

WORKDIR /
ENV OPENCV_VERSION="3.2.0"
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip \
	&& unzip ${OPENCV_VERSION}.zip \
	&& mv opencv-${OPENCV_VERSION} opencv \
	&& mkdir /opencv/build \
	&& cd /opencv/build \
	&& cmake -DBUILD_PERF_TESTS=OFF \
#		-DWITH_FFMPEG=OFF \
		-DBUILD_SHARED_LIBRARY=OFF \
		-DBUILD_TESTS=OFF \
		-DBUILD_opencv_python=OFF \
		-DCMAKE_BUILD_TYPE=RELEASE \
	 	.. \
	&& make -j3 \
#	for x86_64	
	&& cp lib/libopencv_java320.so /usr/lib/x86_64-linux-gnu/ \
#	for arm
#	&& cp lib/libopencv_java320.so /usr/lib/arm-linux-gnueabihf \
#	&& make install \
#	&& ldconfig
	&& rm /${OPENCV_VERSION}.zip 
#	&& rm -r /opencv 

RUN mvn -v \
	&& java -version \
	&& echo $JAVA_HOME \
	&& python2 --version \ 
	&& python3 --version \
	&& ant -version \
	&& export ANT_HOME=/usr/share/ant \
	&& echo $ANT_HOME 
#	&& ffmpeg -version 
#	&& ps -ef|grep kafka

#RUN cp lib/libopencv_java320.so /usr/lib/x86_64-linux-gnu/
# mkdir /opencv-java-bin \
#	cp bin/opencv-320.jar lib/libopencv_java320.so /opencv-java-bin/ \

RUN mkdir Repo \
	&& cd Repo \
	&& git clone https://github.com/bcand/RawfieVideoStream.git \
	&& cd RawfieVideoStream/video-stream-analytics-master/video-stream-collector \
#	&& mvn clean install \
#	&& mvn clean package exec:java -Dexec.mainClass="com.iot.video.app.kafka.collector.VideoStreamCollector" -Dexec.cleanupDaemonThreads=false 
#	-Djava.library.path=/opencv/build/lib	