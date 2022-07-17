# IMAGE: docker build . -t andrewssobral/bgslibrary:opencv_3.4.1py36_boost_167py
FROM andrewssobral/boost_python:1.67.0_opencv_3.4.1py36

ENV BGSLIB_DIR=/root
ENV BOOST_DIR=/root/boost

WORKDIR $BGSLIB_DIR

RUN git clone https://github.com/andrewssobral/bgslibrary.git

WORKDIR ${BGSLIB_DIR}/bgslibrary/build

RUN cp $BOOST_DIR/stage/lib/libboost_python36.so $BOOST_DIR/stage/lib/libboost_python3.so

RUN cmake \
  -D BGS_PYTHON_VERSION=3 \
  -D BGS_PYTHON_SUPPORT=ON \
  -D PYTHON_INCLUDE_DIR=$(python -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
  -D PYTHON_LIBRARY=$(python -c "import distutils.sysconfig as sysconfig; print(sysconfig.get_config_var('LIBDIR'))") \
  -D BOOST_INCLUDEDIR=$BOOST_DIR \
  -D BOOST_LIBRARYDIR=$BOOST_DIR/stage/lib \
  ../

RUN make -j$(nproc)

# export PYTHONPATH="${PYTHONPATH}:/root/bgslibrary/build"
RUN export PYTHONPATH="${PYTHONPATH}:${BGSLIB_DIR}/bgslibrary/build"

WORKDIR ${BGSLIB_DIR}/bgslibrary

RUN chmod +x *.sh

WORKDIR /root
