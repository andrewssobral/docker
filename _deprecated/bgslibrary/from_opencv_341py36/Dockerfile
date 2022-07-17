# IMAGE: docker build . -t andrewssobral/bgslibrary:opencv_3.4.1py36
FROM andrewssobral/opencv:3.4.1py36

ENV BGSLIB_DIR=/root

WORKDIR $BGSLIB_DIR

RUN git clone https://github.com/andrewssobral/bgslibrary.git

RUN cd bgslibrary/build && cmake .. && make

RUN cd bgslibrary && chmod +x *.sh

WORKDIR /root
