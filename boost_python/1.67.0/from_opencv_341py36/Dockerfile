# IMAGE: docker build . -t andrewssobral/boost_python:1.67.0_opencv_3.4.1py36
FROM andrewssobral/opencv:3.4.1py36

ENV BOOST_VERSION=1.67.0
ENV BOOST_DIR=/root

WORKDIR $BOOST_DIR

# Download Boost C++
RUN wget https://dl.bintray.com/boostorg/release/1.67.0/source/boost_1_67_0.zip && \
    unzip boost_1_67_0.zip && \
    rm -r boost_1_67_0.zip && \
    mv boost_1_67_0/ boost

# Build and install Boost
# ./bootstrap.sh
# ./bootstrap.sh --with-libraries=python
# ./bootstrap.sh --with-python=/usr/local/bin/python3 --with-python-version=3.6 --with-python-root=/usr/local/lib/python3.6
# Original:
# if ! [ python.configured ]
# {
#     using python : 3.6 : /usr/local ;
# }
# New:
# if ! [ python.configured ]
# {
#     using python : 3.6 : /usr/local/lib/python3.6 ;
# }
# To solve "/usr/local/include/python3.6m/pyconfig.h" issue
# export CPLUS_INCLUDE_PATH="$CPLUS_INCLUDE_PATH:/usr/local/include/python3.6m/"
RUN cd ~/boost && \
    ./bootstrap.sh --with-libraries=python --with-python=/usr/local/bin/python3 --with-python-version=3.6 --with-python-root=/usr/local/lib/python3.6 && \
    export CPLUS_INCLUDE_PATH="$CPLUS_INCLUDE_PATH:/usr/local/include/python3.6m/" && \
    ./b2 && \
    ./b2 install && \
    /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/boost.conf' && \
    ldconfig && \
    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# The Boost C++ Libraries were successfully built!
# 
# The following directory should be added to compiler include paths:
# 
#     /root/boost
# 
# The following directory should be added to linker library paths:
# 
#     /root/boost/stage/lib

# Not work:
# if ! [ python.configured ]
# {
#     using python : 3.6 : /usr/local/bin/python3 : /usr/local/include/python3.6m : /usr/local/lib ;
# }
#
# From: https://gist.github.com/melvincabatuan/a5a4a10b15ef31a5a481
#     using python : 3.4 : /usr/local/bin/python3 : /usr/local/include/python3.4m : /usr/local/lib ;
#
# From: https://medium.com/@royendgel/boost-boost-python-dlib-python3-on-centos-or-amazon-linux-4039f70a3614
#     using python : 3.6 : /usr/local/bin/python3.6 : /usr/local/include/python3.6m : /usr/local/lib ;
#
# From: https://github.com/boostorg/build/issues/289
# if ! [ python.configured ]
# {
#     using python : 3.6 : /home/joey/anaconda3 : /home/joey/anaconda3/include/python3.6m ;
# }
#
#sed -i 's/original/new/g' project-config.jam
#sed -i 's+/opt/conda+/opt/conda : /opt/conda/include/python3.6m : /opt/conda/lib+g' project-config.jam && \
#sed -i 's+/usr/local+/usr/local/bin/python3.6m : /usr/local/include/python3.6m : /usr/local/lib+g' project-config.jam && \

WORKDIR /root
