FROM debian:bullseye

RUN apt-get update && \
    apt-get install -y --allow-downgrades --no-install-recommends\
    ca-certificates=20210119 && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

ONBUILD COPY --from=0 /tmp/app ./

ENTRYPOINT ["/app/app"]
