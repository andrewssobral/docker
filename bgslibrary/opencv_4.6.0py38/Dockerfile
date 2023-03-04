# docker build . -t andrewssobral/bgslibrary:opencv_4.6.0py38
# docker run -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY andrewssobral/bgslibrary:opencv_4.6.0py38 bash
FROM andrewssobral/opencv4:4.6.0

WORKDIR /opt

RUN git clone --recursive https://github.com/andrewssobral/bgslibrary.git

# Compile C++ library
WORKDIR /opt/bgslibrary/build
RUN cmake .. && make -j4 && make install

# Build and install the python wrapper
WORKDIR /opt/bgslibrary
RUN python setup.py build
RUN python setup.py install
ENV PYTHONPATH=$PYTHONPATH:/opt/bgslibrary/build/lib.linux-x86_64-cpython-38

# Compile the C++ QT User Interface
WORKDIR /opt/bgslibrary/gui/qt/build
RUN cmake .. && make -j4
RUN cp bgslibrary_gui ../../..

# Compile examples
WORKDIR /opt/bgslibrary/examples/build
RUN cmake .. && make -j4

# Turn all shell scripts executable
WORKDIR /opt/bgslibrary
RUN chmod +x *.sh
