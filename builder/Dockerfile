FROM golang:1.19.6-bullseye as builder

ENV GOPRIVATE="github.com/companieshouse"

ONBUILD ARG SSH_PRIVATE_KEY
ONBUILD ARG SSH_PRIVATE_KEY_PASSPHRASE

COPY ./builder/bin/go_build /bin/

RUN chmod +x /bin/go_build && \
    git config --global url."git@github.com:".insteadOf https://github.com/

WORKDIR /build

ONBUILD COPY . /build/
