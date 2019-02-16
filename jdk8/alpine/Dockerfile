# https://hub.docker.com/r/adoptopenjdk/openjdk8
FROM adoptopenjdk/openjdk8:jdk8u202-b08-alpine

LABEL maintainer="eed3si9n@gmail.com"

RUN set -x \
  && apk --update add --no-cache --virtual .build-deps curl \
  && SBT_VER="1.2.8" \
  && ESUM="9bb9212541176d6fcce7bd12e4cf8a9c9649f5b63f88b3aff474e0b02c7cfe58" \
  && SBT_URL="https://piccolo.link/sbt-${SBT_VER}.tgz" \
  && apk add shadow \
  && apk add bash \
  && apk add openssh \
  && apk add rsync \
  && apk add git \
  && curl -Ls ${SBT_URL} > /tmp/sbt-${SBT_VER}.tgz \
  && sha256sum /tmp/sbt-${SBT_VER}.tgz \
  && (echo "${ESUM}  /tmp/sbt-${SBT_VER}.tgz" | sha256sum -c -) \
  && mkdir /opt/sbt \
  && tar -zxf /tmp/sbt-${SBT_VER}.tgz -C /opt/sbt \
  && sed -i -r 's#run \"\$\@\"#unset JAVA_TOOL_OPTIONS\nrun \"\$\@\"#g' /opt/sbt/sbt/bin/sbt \
  && apk del --purge .build-deps \
  && rm -rf /tmp/sbt-${SBT_VER}.tgz /var/cache/apk/*

RUN id -u demiourgos1 2> /dev/null || useradd --system --create-home --uid 1001 --gid 0 demiourgos1

WORKDIR /opt/workspace
RUN chown -R 1001:0 /opt/workspace

USER 1001

ENV PATH="/opt/sbt/sbt/bin:$PATH" \
    JAVA_OPTS="-XX:+UseContainerSupport -Dfile.encoding=UTF-8" \
    SBT_OPTS="-Xmx2048M -Xss2M"

RUN sbt about