# src: https://github.com/WoozyMasta/kaniko-tools/blob/master/Dockerfile
# BUILD
# -----
FROM golang:latest as builder

ARG GOARCH=amd64
#ARG KANIKO_RELEASE="1.6.0"
ARG KANIKO_RELEASE="1.9.1"
# TODO wait for a fix ? https://github.com/GoogleContainerTools/kaniko/issues/2222

WORKDIR /go/src/github.com/GoogleContainerTools

RUN set -eux && \
    mkdir ./kaniko && \
    wget -O /tmp/kaniko.tar.gz --progress=dot:giga \
      https://github.com/GoogleContainerTools/kaniko/archive/refs/tags/v$KANIKO_RELEASE.tar.gz && \
    tar -xvf /tmp/kaniko.tar.gz -C ./kaniko --strip-components 1

WORKDIR /go/src/github.com/GoogleContainerTools/kaniko

RUN mkdir -p /kaniko/.docker && \
    make && make out/warmer


# MAKE CONTAINER
# --------------
FROM alpine:3.16

ARG JQ_RELEASE="1.6"
ARG YQ_RELEASE="4.29.2"
ARG PUSHRM_RELEASE="1.9.0"

# hadolint ignore=DL3018
RUN set -eux && \
    apk add --update --no-cache \
    bash git grep util-linux tar xz gzip bzip2 curl coreutils openssl ca-certificates && \
    curl -sLo /usr/bin/jq \
      "https://github.com/stedolan/jq/releases/download/jq-$JQ_RELEASE/jq-linux64" && \
    curl -sLo /usr/bin/yq \
      "https://github.com/mikefarah/yq/releases/download/v$YQ_RELEASE/yq_linux_amd64" && \
    curl -sLo /usr/bin/pushrm \
      "https://github.com/christian-korneck/docker-pushrm/releases/download/v$PUSHRM_RELEASE/docker-pushrm_linux_amd64" && \
    chmod +x /usr/bin/jq /usr/bin/yq /usr/bin/pushrm

COPY --from=builder /go/src/github.com/GoogleContainerTools/kaniko/out/* /kaniko/
COPY --from=builder /go/src/github.com/GoogleContainerTools/kaniko/out/warmer /kaniko/warmer
COPY --from=builder /go/src/github.com/GoogleContainerTools/kaniko/files/nsswitch.conf /etc/nsswitch.conf

ENV HOME /root
ENV USER root
ENV PATH $PATH:/kaniko
ENV DOCKER_CONFIG /kaniko/.docker/

WORKDIR /workspace

ENTRYPOINT ["/kaniko/executor"]