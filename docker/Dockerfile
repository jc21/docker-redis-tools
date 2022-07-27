# This is a Dockerfile intended to be built using `docker buildx`
# for multi-arch support. Building with `docker build` may have unexpected results.
FROM redis:bullseye AS redis
FROM jc21/gotools:latest AS gobuild

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG RENAMER_VERSION
ARG MIGRATOR_VERSION
ARG TARGETPLATFORM

ENV RENAMER_VERSION="${RENAMER_VERSION:-1.0.0}" \
	MIGRATOR_VERSION="${MIGRATOR_VERSION:-1.0.4}" \
	GO111MODULE=on

RUN mkdir -p /workspace
WORKDIR /workspace
RUN wget "https://github.com/jc21/redis-renamer/archive/refs/tags/v${RENAMER_VERSION}.tar.gz" -O renamer.tgz
RUN wget "https://github.com/jc21/redis-migrator/archive/refs/tags/v${MIGRATOR_VERSION}.tar.gz" -O migrator.tgz
RUN tar -xzf renamer.tgz
RUN tar -xzf migrator.tgz
COPY scripts/gobuild /gobuild

WORKDIR /workspace/redis-renamer-${RENAMER_VERSION}
RUN /gobuild redis-renamer "$RENAMER_VERSION"

WORKDIR /workspace/redis-migrator-${MIGRATOR_VERSION}
RUN /gobuild redis-migrator "$MIGRATOR_VERSION"

#############
# Final Image
#############

FROM debian:bullseye-slim
LABEL maintainer="Jamie Curnow <jc@jc21.com>"

COPY --from=gobuild /workspace/redis-renamer /usr/local/bin/redis-renamer
COPY --from=gobuild /workspace/redis-migrator /usr/local/bin/redis-migrator
COPY --from=redis /usr/local/bin/redis-cli /usr/local/bin/redis-cli

RUN apt update \
	&& apt install -y openssl \
	&& apt-get clean

# test the comands
RUN exec /usr/local/bin/redis-renamer --help
RUN exec /usr/local/bin/redis-migrator --help
RUN exec /usr/local/bin/redis-cli --help

LABEL org.label-schema.schema-version="1.0" \
	org.label-schema.license="MIT" \
	org.label-schema.name="redis-tools" \
	org.label-schema.url="https://github.com/jc21/docker-redis-tools" \
	org.label-schema.vcs-url="https://github.com/jc21/docker-redis-tools.git" \
	org.label-schema.cmd="docker run --rm -ti jc21/redis-tools:latest redis-cli -h"
