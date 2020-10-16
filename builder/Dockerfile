FROM golang:1.15-alpine as builder

RUN apk add --no-cache git=~2.26 openssh-client=~8.3 build-base=~0.5

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
