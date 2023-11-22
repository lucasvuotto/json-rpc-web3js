FROM node:18.18.0-buster-slim

RUN apt-get update && apt-get install -yqq --no-install-recommends \
    build-essential ca-certificates git netcat-openbsd python3

ENV JAVA_HOME=/opt/java/openjdk
COPY --from=eclipse-temurin:11-jre $JAVA_HOME $JAVA_HOME
ENV PATH="${JAVA_HOME}/bin:${PATH}"

USER node
WORKDIR /home/node

COPY --chown=node:node package*.json ./
# Workaround dependency pulled over SSH.
# Workaround "ERR! Maximum call stack size exceeded".
#    rm -f package-lock.json && \
RUN git config --global url."https://github.com/web3-js/scrypt-shim".insteadOf "ssh://git@github.com/web3-js/scrypt-shim" && \
    npm install
COPY --chown=node:node . ./

# Eases interoperatibility with resources, inputs and outputs.
USER root
