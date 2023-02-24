FROM debian:bullseye

WORKDIR /app

ONBUILD COPY --from=0 /tmp/app ./

ENTRYPOINT ["/app/app"]
