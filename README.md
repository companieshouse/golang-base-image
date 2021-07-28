# golang-base-image

Base Docker images for go applications.

- [Supported images](#supported-images)
- [Image properties](#image-properties)
- [Sample use](#sample-use)
- [Licence](#license)

## Known Issues

Docker releases `>=2.4.0.0` have [BuildKit](https://github.com/moby/buildkit) enabled by default - this breaks image builds due to a known issue (https://github.com/moby/buildkit/issues/816) with `BuildKit` and `ONBUILD COPY --from` directives which we use in our `runtime` image.

To fix this you can disable BuildKit in one of two ways:

1. Prefix `docker build` commands with `DOCKER_BUILDKIT=0`
2. Disable BuildKit system wide by configuring the Docker daemon for your system - see: https://docs.docker.com/config/daemon/#configure-the-docker-daemon


## Supported images

| Tag                                                                                  | OS         | Go version |
| ------------------------------------------------------------------------------------ | ---------- | ---------- |
| `169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/golang:1.16-alpine-debug-builder` | Alpine     | 1.16.x     |
| `169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/golang:alpine-debug-runtime`      | Alpine     | None       |

## Image properties

Here are the build arguments used in the base builder images:

| Name                         | Default value | Required | Comment                                                             |
| ---------------------------- | ------------- | -------- | ------------------------------------------------------------------- |
| `SSH_PRIVATE_KEY`            | none          | yes      | used to pull dependencies from GitHub during build phase            |
| `SSH_PRIVATE_KEY_PASSPHRASE` | none          | no       | used to remove passphrase from key, needed only if key is protected |

Here are the properties used in the base runtime images:

| Directive    | Value      |
| ------------ | ---------- |
| `WORKDIR`    | `/app`     |
| `ENTRYPOINT` | `/app/app` |

## Sample use

Follow below steps to package go application in debug mode as a Docker image:

1. create `.dockeringore` file to filter out files that should not be copied to Docker image

2. create `Dockerfile` file with instructions below and adjust port number to one application uses (CMD --listen flag and EXPOSE):

   ```dockerfile
   FROM 169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/golang:1.16-alpine-debug-builder

   FROM 169942020521.dkr.ecr.eu-west-1.amazonaws.com/base/golang:alpine-debug-runtime

   CMD ["./dlv", "exec", "--listen=:9999", "--headless=true", "--api-version=2", "./app"]

   EXPOSE 9999
   ```

Please note that you may need to alter the docker-chs-development compose file to add a "ports" variable allowing you to map a port on your local machine to the
port used by your application on docker-chs-development.

E.g.

```
expose:
  - 9999
ports:
  - "9999:9999"
```

3. build Docker image using `DOCKER_BUILDKIT=0 docker build --build-arg SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)" --build-arg SSH_PRIVATE_KEY_PASSPHRASE -t 169942020521.dkr.ecr.eu-west-1.amazonaws.com/local/[application name] .` command

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Using the debugger

### Goland IDE

1. With your application open in Goland IDE go to Menu -> Run -> Edit Configurations...

2. Press the + button in the top left of the pop-up window to create a new Configuration

3. Select Go Remote and apply the following values:
        1. Name: Docker debug
        2. Host: api.chs.local
        3. Port: Port number matching the CMD --listen flag and EXPOSE command

### VSCode
Add steps here for VSCode.

## Important

These Docker containers create a debugging environment which should not be used to create new images of your application on AWS ECR. They are for local use only.
