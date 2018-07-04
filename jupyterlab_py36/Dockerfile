FROM python:3.6

WORKDIR /root

RUN apt-get update && apt-get install -y openjdk-8-jdk

RUN apt-get install ca-certificates-java && update-ca-certificates -f

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/

RUN export JAVA_HOME

RUN apt-get clean

RUN pip install jupyter -U && pip install jupyterlab

RUN pip install py4j

RUN pip install proactive --upgrade

# http://jupyter-notebook.readthedocs.io/en/stable/security.html
ADD jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

ADD proactive_python_client.ipynb /root

EXPOSE 8888

ENTRYPOINT ["jupyter", "lab", "--port=8888", "--ip=0.0.0.0", "--no-browser", "--allow-root"]