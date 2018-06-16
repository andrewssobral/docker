# IMAGE: docker build . -t andrewssobral/opencv:base
FROM ubuntu:16.04

WORKDIR /root

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    pkg-config \
    wget \
    git \
    unzip \
    nano \
    curl \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libeigen3-dev \
    libglew-dev \
    libgtk2.0-dev \
    libgtk-3-dev \
    libjasper-dev \
    libjpeg-dev \
    libpng12-dev \
    libpostproc-dev \
    libswscale-dev \
    libtbb2 \
    libtbb-dev \
    libtiff5-dev \
    libv4l-dev \
    libxvidcore-dev \
    libx264-dev \
    qt5-default \
    zlib1g-dev \
    libatlas-base-dev \
    gfortran \
    libhdf5-dev \
    libgl1-mesa-glx \
    python-dev \
    python3-dev \
    && apt-get autoclean && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install pip
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
RUN python3 get-pip.py
RUN python2 get-pip.py

RUN pip install numpy virtualenv
RUN pip3 install numpy virtualenv

WORKDIR /root
