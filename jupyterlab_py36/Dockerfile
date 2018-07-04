FROM python:3.6

WORKDIR /root

RUN pip install jupyter -U && pip install jupyterlab

RUN pip install proactive --upgrade

# http://jupyter-notebook.readthedocs.io/en/stable/security.html
ADD jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

ADD proactive_python_client.ipynb

EXPOSE 8888

ENTRYPOINT ["jupyter", "lab", "--port=8888", "--ip=0.0.0.0", "--no-browser", "--allow-root"]