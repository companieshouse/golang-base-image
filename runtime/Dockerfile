FROM alpine:3.12

WORKDIR /app

ONBUILD COPY --from=0 /tmp/app ./

ENTRYPOINT ["/app/app"]
