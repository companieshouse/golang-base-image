FROM golang:1.15-alpine

WORKDIR /app

ONBUILD COPY --from=0 /tmp/app ./

ENTRYPOINT ["/app/app"]
