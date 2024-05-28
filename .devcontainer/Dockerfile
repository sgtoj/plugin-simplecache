FROM mcr.microsoft.com/devcontainers/base:jammy

ENV DEBIAN_FRONTEND=noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN=true
ENV PATH=$PATH:/usr/local/go/bin:

RUN touch dpkg-preseed \
    && echo "tzdata tzdata/Areas select America" >> dpkg-preseed \
    && echo "tzdata tzdata/Zones/America select New_York" >> dpkg-preseed \
    && debconf-set-selections dpkg-preseed

COPY --from=docker:dind /usr/local/bin/docker /usr/local/bin/

RUN apt update && apt install -y \
    build-essential \
    curl \
    gettext-base \
    git \
    jq \
    less \
    unzip \
    vim \
    wget \
    xxd

# install go
ARG GOLANG_VERSION=1.19.13
RUN SYSTEM_ARCH=$(dpkg --print-architecture) \
    && curl -sL "https://go.dev/dl/go${GOLANG_VERSION}.linux-${SYSTEM_ARCH}.tar.gz" | tar -C /usr/local -xzf - \
    && /usr/local/go/bin/go install -v golang.org/x/tools/gopls@latest

ARG GOLANGCI_LINT_VERSION=v1.50.0
RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin ${GOLANGCI_LINT_VERSION}

ARG YAEGI_VERSION=v0.14.2
RUN curl -sfL https://raw.githubusercontent.com/traefik/yaegi/master/install.sh | bash -s -- -b $(go env GOPATH)/bin ${YAEGI_VERSION}

# arkade
RUN curl -sLS https://get.arkade.dev | sh

WORKDIR /
