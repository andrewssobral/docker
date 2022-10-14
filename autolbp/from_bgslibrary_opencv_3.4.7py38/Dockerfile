# docker build . -t andrewssobral/autolbp:bgslibrary_opencv_3.4.7py38
# docker run -it --rm andrewssobral/autolbp:bgslibrary_opencv_3.4.7py38 bash
# docker run -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY andrewssobral/autolbp:bgslibrary_opencv_3.4.7py38 bash
FROM andrewssobral/bgslibrary:opencv_3.4.7py38

WORKDIR /tmp

RUN python -m pip install --upgrade pip

RUN python -m pip uninstall pybgs
RUN python -m pip install pybgs --upgrade

RUN python -m pip install pandas
RUN python -m pip install numba
RUN python -m pip install git+https://github.com/ow2-proactive/chocolate@master
RUN python -m pip install optuna

