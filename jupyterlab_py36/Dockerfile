FROM python:3.6

WORKDIR /root

RUN apt-get update && apt-get install -y openjdk-8-jdk
RUN apt-get install ca-certificates-java && update-ca-certificates -f
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME
RUN apt-get clean

RUN pip install -U jupyter && pip install -U jupyterlab

RUN pip install -U numpy scikit-learn h2o pandas pandas-ml pymc3 matplotlib

RUN pip install -U statsmodels networkx luminol

# dlib depends cmake
RUN pip install -U scikit-image Pillow graphviz opencv-contrib-python-headless

RUN pip install -U tensorflow Keras
RUN pip install -U http://download.pytorch.org/whl/cpu/torch-0.4.0-cp36-cp36m-linux_x86_64.whl
RUN pip install -U torchvision

RUN pip install -U bokeh visdom

RUN pip install -U dask distributed joblib --upgrade

RUN pip install -U py4j wget lxml
RUN pip install -U proactive --upgrade

# http://jupyter-notebook.readthedocs.io/en/stable/security.html
ADD jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

ADD proactive_python_client.ipynb /root

EXPOSE 8888

ENTRYPOINT ["jupyter", "lab", "--port=8888", "--ip=0.0.0.0", "--no-browser", "--allow-root"]