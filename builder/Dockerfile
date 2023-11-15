FROM golang:1.19.6-bullseye as builder

ENV GOPRIVATE="github.com/companieshouse"

ADD ./builder/lib/build /lib/

RUN chmod +x /lib/build && \
    git config --global url."git@github.com:".insteadOf https://github.com/

WORKDIR /build

ONBUILD COPY . /build/
