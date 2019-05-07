FROM debian:stretch AS Rawfie

MAINTAINER bcand

# Install Java 8
RUN apt-get update \
	&& apt-get install -y \
       	openjdk-8-jdk \
       	python \
       	python-pip \
       	python3 \
    && rm -rf /var/lib/apt/lists/*


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
		ant \
		maven \
		scala \
    && rm -rf /var/lib/apt/lists/*




# Install Python 2.7





# Install python 3.5



	

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
RUN wget http://apache.otenet.gr/dist/spark/spark-2.4.2/spark-2.4.2-bin-hadoop2.7.tgz \
	&& tar -xvzf spark-2.4.2-bin-hadoop2.7.tgz \
	&& mv spark-2.4.2-bin-hadoop2.7 /usr/local/ \
	&& ln -s /usr/local/spark-2.4.2-bin-hadoop2.7/ /usr/local/spark 

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
#	&& mvn clean package exec:java -Dexec.mainClass="com.iot.video.app.spark.processor.VideoStreamProcessor"
#	-Djava.library.path=/opencv/build/lib	