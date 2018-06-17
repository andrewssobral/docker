# IMAGE: docker build . -t andrewssobral/opencv:3.4.1
# IMAGE: docker build . -t andrewssobral/opencv:3.4.1_cuda
FROM andrewssobral/opencv:base

ENV OPENCV_VERSION=3.4.1
ENV CMAKE_INSTALL_PREFIX=/usr/local
ENV DOWNLOAD_OPENCV_EXTRAS=NO
ENV DOWNLOAD_OPENCV_CONTRIB=NO
ENV OPENCV_SOURCE_DIR=/root

WORKDIR $OPENCV_SOURCE_DIR
RUN git clone https://github.com/opencv/opencv.git
RUN cd opencv && git checkout -b v${OPENCV_VERSION} ${OPENCV_VERSION}
# For 3.4.1, if CUDA is enabled, use this commit to fix samples/gpu/CMakeLists.txt
#RUN git merge ec0bb66e5e176ffe267948a98508ac6721daf8ad

#WORKDIR $OPENCV_SOURCE_DIR
#RUN git clone https://github.com/opencv/opencv_extra.git
#RUN cd opencv_extra && git checkout -b v${OPENCV_VERSION} ${OPENCV_VERSION}

#WORKDIR $OPENCV_SOURCE_DIR
#RUN git clone https://github.com/opencv/opencv_contrib.git
#RUN cd opencv_contrib && git checkout -b v${OPENCV_VERSION} ${OPENCV_VERSION}

RUN cd $OPENCV_SOURCE_DIR/opencv && mkdir build
WORKDIR $OPENCV_SOURCE_DIR/opencv/build

# if CUDA is enabled:
#   -D WITH_CUDA=ON \
#   -D ENABLE_FAST_MATH=ON \
#   -D CUDA_FAST_MATH=ON \
#
# if GStreamer is enabled:
#   -D WITH_GSTREAMER=ON \
#   -D WITH_GSTREAMER_0_10=OFF \
#
# if DOWNLOAD_OPENCV_EXTRAS is enabled:
#   -D OPENCV_TEST_DATA_PATH=${OPENCV_SOURCE_DIR}/opencv_extra/testdata \
#
# if DOWNLOAD_OPENCV_CONTRIB is enabled:
#   -D OPENCV_EXTRA_MODULES_PATH=${OPENCV_SOURCE_DIR}/opencv_contrib/modules \
#
# if using specific python:
#   -D PYTHON_EXECUTABLE=$(which python2) \
#   -D PYTHON_DEFAULT_EXECUTABLE=$(which python2) \
#   -D PYTHON_INCLUDE_DIR=$(python2 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
#   -D PYTHON_LIBRARY=$(python2 -c "import sysconfig; print(sysconfig.get_config_var('LIBDIR')+'/'+sysconfig.get_config_var('LDLIBRARY'))") \
#   -D PYTHON3_EXECUTABLE=$(which python3) \
#   -D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
#   -D PYTHON3_INCLUDE_PATH=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
#   -D PYTHON3_LIBRARIES=$(python3 -c "import sysconfig; print(sysconfig.get_config_var('LIBDIR')+'/'+sysconfig.get_config_var('LDLIBRARY'))") \
#   -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
RUN cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} \
      -D WITH_CUBLAS=ON \
      -D WITH_LIBV4L=ON \
      -D WITH_QT=ON \
      -D WITH_OPENGL=ON \
      -D WITH_JAVA=OFF \
      -D ENABLE_CXX11=ON \
      -D WITH_IPP=ON \
      -D WITH_TBB=ON \
      -D ENABLE_AVX=OFF \
      -D BUILD_TESTS=OFF \
      -D INSTALL_TESTS=OFF \
      -D BUILD_PERF_TESTS=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D INSTALL_C_EXAMPLES=OFF \
      -D INSTALL_PYTHON_EXAMPLES=OFF \
      ../

RUN make -j $(nproc) && \
    make install && \
    ldconfig

# For info:
# -- Installing: /usr/local/lib/python2.7/dist-packages/cv2.so
# -- Set runtime path of "/usr/local/lib/python2.7/dist-packages/cv2.so" to "/usr/local/lib"
# -- Installing: /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-x86_64-linux-gnu.so
# -- Set runtime path of "/usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-x86_64-linux-gnu.so" to "/usr/local/lib"

#RUN rm -rf $OPENCV_SOURCE_DIR/opencv

WORKDIR /root
