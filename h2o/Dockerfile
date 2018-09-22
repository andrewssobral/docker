FROM python:3.6

WORKDIR /root

# Install generic packages
# ------------------------
RUN apt-get update && apt-get install -y \
        wget \
        git \
        unzip \
        nano \
        && apt-get autoclean && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Java 8
# --------------
RUN apt-get update && apt-get install -y openjdk-8-jdk
RUN apt-get install ca-certificates-java && update-ca-certificates -f
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME
RUN apt-get clean

# Install python libraries
# ------------------------
RUN pip install scikit-learn matplotlib pandas

# Install H2O
# -----------
RUN wget http://h2o-release.s3.amazonaws.com/h2o/latest_stable -O latest && \
  wget --no-check-certificate -i latest -O /opt/h2o.zip && \
  unzip -d /opt /opt/h2o.zip && \
  rm /opt/h2o.zip && \
  cd /opt && \
  cd `find . -name 'h2o.jar' | sed 's/.\///;s/\/h2o.jar//g'` && \ 
  cp h2o.jar /opt && \
  /usr/bin/pip install `find . -name "*.whl"` && \
  cd / && \
  wget https://raw.githubusercontent.com/h2oai/h2o-3/master/docker/start-h2o-docker.sh && \
  chmod +x start-h2o-docker.sh

EXPOSE 54321
EXPOSE 54322

ENTRYPOINT ["java", "-Xmx4g", "-jar", "/opt/h2o.jar"]