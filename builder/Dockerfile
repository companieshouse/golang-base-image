FROM amazonlinux:2023 as builder

ENV GOPRIVATE="github.com/companieshouse"

RUN yum -y install golang-1.19.3 git \
    && yum clean all

RUN git config --global url."git@github.com:".insteadOf https://github.com/

WORKDIR /build

ONBUILD ARG SSH_PRIVATE_KEY
ONBUILD ARG SSH_PRIVATE_KEY_PASSPHRASE

ONBUILD RUN mkdir -m 0600 ~/.ssh \
    && ssh-keyscan github.com >> ~/.ssh/known_hosts \
    && echo "${SSH_PRIVATE_KEY}" > ~/.ssh/id_rsa \
    && chmod 600 ~/.ssh/id_rsa \
    && ssh-keygen -p -f ~/.ssh/id_rsa -P "${SSH_PRIVATE_KEY_PASSPHRASE}" -N ""

ONBUILD COPY go.mod go.sum ./

ONBUILD RUN go mod download

ONBUILD COPY . ./

ONBUILD RUN go fmt ./... && go build -o /tmp/app
