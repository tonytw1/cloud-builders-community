ARG BASE_IMAGE=eclipse-temurin:11
FROM ${BASE_IMAGE}

ARG SBT_VERSION=1.10.1
ARG SHA=5647898e972313ad6abcbc8df8ad54974a49f8f5020dfd9b7cc89f4821047a5b
ARG BASE_URL=https://github.com/sbt/sbt/releases/download

RUN apt-get update -qqy \
  && apt-get install -qqy curl bc unzip \
  && mkdir -p /usr/share \
  && curl -fsSL -o "sbt-${SBT_VERSION}.zip" "${BASE_URL}/v${SBT_VERSION}/sbt-${SBT_VERSION}.zip" \
  && echo "${SHA}  sbt-${SBT_VERSION}.zip" | sha256sum -c - \
  && unzip -qq "sbt-${SBT_VERSION}.zip" \
  && rm -f "sbt-${SBT_VERSION}.zip" \
  && mv sbt "/usr/share/sbt-${SBT_VERSION}" \
  && ln -s "/usr/share/sbt-${SBT_VERSION}/bin/sbt" /usr/bin/sbt \
  && install -m 0755 -d /etc/apt/keyrings \
  && curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
  && chmod a+r /etc/apt/keyrings/docker.asc \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
  && apt-get update \
  && apt-get install -y docker-ce docker-ce-cli \
  && apt-get remove -qqy --purge curl \
  && rm /var/lib/apt/lists/*_*


ENTRYPOINT ["/usr/bin/sbt"]
