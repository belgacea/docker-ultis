FROM python:3.10.11-slim
#FROM python:3.11.3-slim

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --fix-broken --fix-missing  \
      sudo bash bash-completion less git vim htop screen \
      wget util-linux mount bsdutils apt-file  \
      build-essential gcc g++ cmake valgrind gdb emacs  \
      libsasl2-dev libsasl2-2 libsasl2-modules-gssapi-mit && \
    apt-file update && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    apt-get autoclean -y && \
    rm -rf /tmp/* && \
    rm -rf /var/lib/apt/lists/*

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

# Overwrite sh with bash
RUN mv /bin/sh /bin/sh_tmp && ln -s /bin/bash /bin/sh

# Set oracle client lib path
ENV LD_LIBRARY_PATH=/opt/oracle/instantclient_21_10

# Restore sh
# RUN rm /bin/sh && mv /bin/sh_tmp /bin/sh