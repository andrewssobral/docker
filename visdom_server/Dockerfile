FROM continuumio/miniconda3

# Install dependencies
RUN apt-get update && apt-get install -y \
        build-essential \
        cmake \
        pkg-config \
        wget \
        git \
        unzip \
        nano \
        libjpeg-dev \
        libtiff-dev \
        libjasper-dev \
        libpng-dev \
        libgtk2.0-dev \
        libavcodec-dev \
        libavformat-dev \
        libswscale-dev \
        libv4l-dev \
        libatlas-base-dev \
        gfortran \
        libhdf5-dev \
        libtbb2 \
        libtbb-dev \
        libgl1-mesa-glx \
        && apt-get autoclean && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#RUN pip install visdom
RUN git clone https://github.com/andrewssobral/visdom.git
RUN cd visdom/ && pip install -e . && python setup.py install

WORKDIR /root