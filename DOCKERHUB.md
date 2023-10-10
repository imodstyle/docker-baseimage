# A minimal docker baseimage to ease creation of long-lived application containers based of Jlesage, this intended for educational purpose only

This is a docker baseimage that can be used to create containers for any
long-lived application.

## Images

Different docker images are available:

| Base Distribution  | Docker Image Base Tag |
|--------------------|-----------------------|
| [Alpine 3.18.4]      | alpine-3.18.4       |
| [Ubuntu 22.04 LTS] | ubuntu-22.04          |

[Alpine 3.18.4]: https://alpinelinux.org
[Ubuntu 22.04 LTS]: http://releases.ubuntu.com/22.04/

### Content

Here are the main components of the baseimage:

  * An init system.
  * A process supervisor, with proper PID 1 functionality (proper reaping of
    processes).
  * Useful tools to ease container building.
  * Environment to better support dockerized applications.


### Versioning

Images are versioned.  Version number follows the [semantic versioning].  The
version format is `MAJOR.MINOR.PATCH`, where an increment of the:

  - `MAJOR` version indicates that a backwards-incompatible change has been done.
  - `MINOR` version indicates that functionality has been added in a backwards-compatible manner.
  - `PATCH` version indicates that a bug fix has been done in a backwards-compatible manner.

[semantic versioning]: https://semver.org

### Tags

For each distribution-specific image, multiple tags are available:

| Tag           | Description                                              |
|---------------|----------------------------------------------------------|
| distro-vX.Y.Z | Exact version of the image.                              |
| distro-vX.Y   | Latest version of a specific minor version of the image. |
| distro-vX     | Latest version of a specific major version of the image. |

## Getting started

The `Dockerfile` for your application can be very simple, as only three things
are required:

  * Instructions to install the application.
  * A script that starts the application (stored at `/startapp.sh` in
    container).
  * The name of the application.

Here is an example of a docker file that would be used to run a simple web
NodeJS server.
In `Dockerfile`:
```Dockerfile
# Pull base image.
FROM imodstyle/baseimage:alpine-3.18

# Install http-server.
RUN add-pkg nodejs-npm && \
    npm install http-server -g

# Copy the start script.
COPY startapp.sh /startapp.sh

# Set the name of the application.
RUN set-cont-env APP_NAME "http-server"

# Expose ports.
EXPOSE 8080
```

In `startapp.sh`:
```shell
#!/bin/sh
exec /usr/bin/http-server
```

Then, build your docker image:

    docker build -t docker-http-server .

And run it:

    docker run --rm -p 8080:8080 docker-http-server

You should be able to access the HTTP server by opening in a web browser:

```
http://[HOST IP ADDR]:8080
```

## Documentation

Full documentation is available at https://github.com/imodstyle/docker-baseimage.

