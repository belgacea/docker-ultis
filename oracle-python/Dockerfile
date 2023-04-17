FROM python:3.9.15-slim-buster

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    apt-get autoclean -y && \
    rm -rf /tmp/* && \
    rm -rf /var/lib/apt/lists/*

# Overwrite sh with bash
RUN mv /bin/sh /bin/sh_tmp && ln -s /bin/bash /bin/sh

# Utils
RUN apt-get install -y --fix-broken --fix-missing sudo git vim wget util-linux mount bsdutils htop screen
# bash-completion less
# ftp ncftp doxygen graphviz software-properties-common libopenblas-dev liblapack-dev

# C++
RUN apt-get install -y --fix-broken --fix-missing sudo build-essential gcc g++ cmake valgrind gdb emacs
# doxygen graphviz software-properties-common libopenblas-dev liblapack-dev

# SASL
RUN apt-get install -y --fix-broken --fix-missing libsasl2-dev libsasl2-2 libsasl2-modules-gssapi-mit

# Installing Oracle instant client
WORKDIR /opt/oracle
RUN apt-get update && apt-get install -y libaio1 unzip \
  && wget https://download.oracle.com/otn_software/linux/instantclient/instantclient-basiclite-linuxx64.zip \
  && unzip instantclient-basiclite-linuxx64.zip \
  && rm -f instantclient-basiclite-linuxx64.zip \
  && cd /opt/oracle/instantclient* \
  && rm -f *jdbc* *occi* *mysql* *README *jar uidrvci genezi adrci \
  && echo /opt/oracle/instantclient* > /etc/ld.so.conf.d/oracle-instantclient.conf \
  && ldconfig

# TODO install oracle client lib
#ENV LD_LIBRARY_PATH=

# Restore sh
# RUN rm /bin/sh && mv /bin/sh_tmp /bin/sh

#ENV APP_PATH=/home/generator
#RUN mkdir -p $APP_PATH
#WORKDIR $APP_PATH
#
#COPY .. $APP_PATH
#RUN pip install --upgrade pip
#RUN pip install -r requirements.txt
#
#ENTRYPOINT ["/bin/bash", "/home/generator/run.sh"]