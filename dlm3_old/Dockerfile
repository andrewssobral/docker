FROM continuumio/miniconda3

# Install dependencies for OpenCV
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

# Install deep learning libraries 
RUN conda install -y numpy
RUN conda install -y tensorflow
RUN conda install -y keras
#RUN conda install -y opencv
#RUN conda install -y caffe
RUN conda install -y scikit-learn
RUN conda install -y scikit-image
RUN pip install visdom
RUN conda install -y pytorch torchvision -c soumith

# Download OpenCV 3.3.0
RUN cd ~ && \ 
    wget https://github.com/Itseez/opencv/archive/3.3.0.zip && \
    unzip 3.3.0.zip && \
    mv ~/opencv-3.3.0/ ~/opencv/ && \
    rm -rf ~/3.3.0.zip

# Download contrib module for OpenCV 3.3.0
RUN cd ~ && \
    wget https://github.com/opencv/opencv_contrib/archive/3.3.0.zip -O 3.3.0-contrib.zip && \
    unzip 3.3.0-contrib.zip && \
    mv opencv_contrib-3.3.0 opencv_contrib && \
    rm -rf ~/3.3.0-contrib.zip

# For Python2
# -D PYTHON2_EXECUTABLE=$(which python2) \
# -D PYTHON2_INCLUDE_DIR=$(python2 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
# -D PYTHON2_PACKAGES_PATH=$(python2 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \

# For Python3
# -D PYTHON3_EXECUTABLE=$(which python3) \
# -D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
# -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \

# Default Python
# -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
# -D PYTHON_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
# -D PYTHON_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \

# Configure OpenCV (Force python3 from anaconda instead of local python2)
RUN cd ~/opencv && \
    mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_C_EXAMPLES=OFF \
        -D INSTALL_PYTHON_EXAMPLES=ON \
	-D WITH_IPP=OFF \
	-D WITH_TBB=ON \
        -D ENABLE_AVX=OFF \
        -D WITH_CUDA=OFF \
        -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
	-D PYTHON_EXECUTABLE=$(which python3) \
        -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
        -D PYTHON_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -D PYTHON_LIBRARY=$(python3 -c "import sysconfig; print(sysconfig.get_config_var('LIBDIR')+'/'+sysconfig.get_config_var('LDLIBRARY'))") \
        -D PYTHON3_EXECUTABLE=$(which python3) \
        -D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -D PYTHON3_INCLUDE_PATH=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
        -D PYTHON3_LIBRARIES=$(python3 -c "import sysconfig; print(sysconfig.get_config_var('LIBDIR')+'/'+sysconfig.get_config_var('LDLIBRARY'))") \
        -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
        -D BUILD_EXAMPLES=OFF ..

# Build OpenCV
RUN cd ~/opencv/build && \
    make -j $(nproc) && \
    make install && \
    ldconfig

# Clean OpenCV repos
RUN rm -rf ~/opencv/build && \
    rm -rf ~/opencv/3rdparty && \
    rm -rf ~/opencv/doc && \
    rm -rf ~/opencv/include && \
    rm -rf ~/opencv/platforms && \
    rm -rf ~/opencv/modules && \
    rm -rf ~/opencv_contrib/build && \
    rm -rf ~/opencv_contrib/doc

WORKDIR /root

# Download Boost C++ 1.65.0
RUN wget https://dl.bintray.com/boostorg/release/1.65.0/source/boost_1_65_0.zip && \
    unzip boost_1_65_0.zip && \
    rm -r boost_1_65_0.zip && \
    mv boost_1_65_0/ boost

# Build and install Boost
# ./bootstrap.sh --with-libraries=python
RUN cd ~/boost && \
    ./bootstrap.sh && \
    sed -i 's+/opt/conda+/opt/conda : /opt/conda/include/python3.6m : /opt/conda/lib+g' project-config.jam && \
    ./b2 --with=all && \
    ./b2 install && \
    /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/boost.conf' && \
    ldconfig && \
    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

WORKDIR /root

# Download Dlib 19.4
#RUN git clone https://github.com/davisking/dlib.git
RUN wget http://dlib.net/files/dlib-19.4.tar.bz2 && \
    tar -jxvf dlib-19.4.tar.bz2 && \
    rm -r dlib-19.4.tar.bz2 && \
    mv dlib-19.4/ dlib

# Build and install Dlib
RUN cd ~/dlib && \
    sed -i 's+set(_PYTHON3_VERSIONS 3.4 3.3 3.2 3.1 3.0)+set(_PYTHON3_VERSIONS 3.6 3.5 3.4 3.3 3.2 3.1 3.0)+g' /usr/share/cmake-3.0/Modules/FindPythonInterp.cmake && \
    sed -i 's+set(_PYTHON3_VERSIONS 3.4 3.3 3.2 3.1 3.0)+set(_PYTHON3_VERSIONS 3.6 3.5 3.4 3.3 3.2 3.1 3.0)+g' /usr/share/cmake-3.0/Modules/FindPythonLibs.cmake && \
    python setup.py install --yes USE_AVX_INSTRUCTIONS && \
    python -c 'import dlib; print(dlib.__version__)'

# Build and install YOLO
RUN cd ~ && git clone https://github.com/andrewssobral/darknet.git
WORKDIR /root/darknet
RUN make
RUN wget https://pjreddie.com/media/files/yolo.weights
RUN ./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg

WORKDIR /root