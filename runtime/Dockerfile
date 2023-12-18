FROM debian:bullseye AS RUNNER

RUN apt-get update && \
    apt-get install -y --allow-downgrades --no-install-recommends\
    ca-certificates=20210119 && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir /app/

WORKDIR /app

ENTRYPOINT ["/app/app"]
