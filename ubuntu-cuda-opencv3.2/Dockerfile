FROM ubuntu:16.04

# Set one or more individual labels
LABEL com.example.version="0.0.1-beta"
LABEL author="ffarhour"
LABEL com.example.release-date="2017-11-26"

# install cuda libraries
RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y nvidia-cuda-toolkit \ 
	&& rm -rf /var/lib/apt/lists/*

# install common programs and libraries + cuda
RUN apt-get update \
    	&& apt-get install -y python3 python3-dev python3-setuptools \
    	nano vim wget curl git \
    	software-properties-common python-software-properties \
	sudo \
    	&& easy_install3 pip \ 
	&& rm -rf /var/lib/apt/lists/*

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
RUN git clone https://github.com/Itseez/opencv.git \
	&& cd /opencv \
	&& git checkout 3.2.0 \
	&& cd / \
	&& git clone https://github.com/Itseez/opencv_contrib.git \
	&& cd /opencv_contrib \
	&& git checkout 3.2.0 \
	&& mkdir /opencv/build \
	&& cd /opencv/build \
	&& cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENGL=ON -D WITH_CUDA=ON -D BUILD_EXAMPLES=OFF -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_SHARED_LIBS=ON -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ../ \
	&& make -j4 \
	&& make install \
	&& ldconfig \
	&& cd / \
	&& rm -rf /opencv \
	&& rm -rf /opencv_contrib

# enable gui
# Replace 1000 with your user / group id
RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer


# install firefox	
#RUN apt-get update && apt-get install -y firefox \
#	&& rm -rf /var/lib/apt/lists/*

# install thunar
RUN apt-get update && apt-get install -y thunar \
	&& rm -rf /var/lib/apt/lists/*

USER developer
ENV HOME /home/developer

# run firefox
#CMD /usr/bin/firefox

# run thunar  
CMD /usr/bin/thunar