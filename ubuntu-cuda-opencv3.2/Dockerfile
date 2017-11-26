FROM ubuntu:16.04

# Set one or more individual labels
LABEL com.example.version="0.0.1-beta"
LABEL author="ffarhour"
LABEL com.example.release-date="2017-11-26"

# install common programs and libraries
RUN apt-get update \
    && apt-get install -y python3 python3-dev python3-setuptools \
    nano vim wget curl git \
    software-properties-common python-software-properties \
    && easy_install3 pip

# install cuda
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y nvidia-cuda-toolkit

# install opencv
RUN apt-get update \
	&& apt-get upgrade -y \
    	&& apt-get install -y libopencv-dev build-essential cmake git libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev libtbb-dev libqt4-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip \
	build-essential cmake git pkg-config \
	libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev \
	libgtk2.0-dev \
	libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
	libatlas-base-dev gfortran \
	&& rm -rf /var/lib/apt/lists/*

#RUN wget https://bootstrap.pypa.io/get-pip.py
#RUN python get-pip.py
#RUN apt-get install python2.7-dev
#RUN pip install numpy

WORKDIR /
RUN git clone https://github.com/Itseez/opencv.git
WORKDIR /opencv
RUN git checkout 3.2.0
WORKDIR /
RUN git clone https://github.com/Itseez/opencv_contrib.git
WORKDIR /opencv_contrib
RUN git checkout 3.2.0
WORKDIR /opencv
RUN mkdir build
WORKDIR /opencv/build
RUN cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENGL=ON -D WITH_CUDA=ON -D BUILD_EXAMPLES=OFF -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_SHARED_LIBS=ON -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ../ 
RUN make -j4
RUN make install
RUN ldconfig
WORKDIR ~
RUN rm -rf /opencv
RUN rm -rf /opencv_contrib
