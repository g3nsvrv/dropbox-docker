# Dropbox as docker-compose

Run the Linux Dropbox client inside Docker with persistent storage and configurable user mapping.

This project builds a Docker image that installs the Dropbox daemon on a Debian-based image, creates a service account with custom UID/GID values, and stores Dropbox data inside a persistent volume. The account is created at container startup, so UID/GID can be changed per-container without rebuilding the image.

## Features

* Runs the official Linux Dropbox client inside Docker
* Custom user and group creation, performed at container startup
* UID/GID mapping for host filesystem compatibility, configurable via environment variables
* Persistent user home directory using Docker volumes
* Dropbox daemon installed once at build time, no network dependency on container start
* No credentials generated or stored in the image

## Project structure

```text
.
├── .env
├── docker-compose.yaml
└── dropbox/
    ├── Dockerfile
    └── entrypoint.sh
```

## Configuration

Configuration is stored in `.env`:

```env
CONTEXT=dropbox
IMAGE=debian:stable-slim
USERACCOUNT=dropbox
GROUPACCOUNT=dropbox
HOSTNAME=dropbox-docker
PUID=1000
PGID=100
```

### Variables

| Variable        | Description                                    |
| --------------- | ----------------------------------------------- |
| `CONTEXT`       | Docker build context                            |
| `IMAGE`         | Base image used for the build                   |
| `USERACCOUNT`   | Username created inside the container           |
| `GROUPACCOUNT`  | Group name created inside the container         |
| `HOSTNAME`      | Container hostname                              |
| `PUID`          | User ID used inside the container               |
| `PGID`          | Group ID used inside the container              |

`USERACCOUNT`, `GROUPACCOUNT`, `PUID`, and `PGID` are read at **container startup**, not build time. They can be overridden per-container (e.g. in `docker-compose.yaml`, or with `docker run -e PUID=2000 -e PGID=2000`) without rebuilding the image.

## Usage

Build and start:

```bash
docker compose up -d --build
```

Check logs:

```bash
docker logs <container-name>
```

On first startup Dropbox will display an authorization URL. Open it in a browser and link your Dropbox account.

## Storage

The compose file creates a persistent volume:

```yaml
volumes:
  dropbox-home:
```

This volume is mounted as:

```text
/home/${USERACCOUNT}
```

The entire Dropbox user environment is persisted, including:

* Dropbox files
* Dropbox account configuration
* Dropbox application data

## Extending this image

The entrypoint performs account setup, then runs whatever command it's given, defaulting to `dropboxd` (`CMD ["dropboxd"]`). A child image can supply its own `CMD` and it will still run after account setup, as the unprivileged user — as long as the entrypoint itself isn't overridden.

## Notes

* No password is generated for the created account, and no credentials are baked into the image.
* Dropbox data persists between container recreations through the Docker volume.

## TODO

* Mount paths and custom volumes

## License

This project is licensed under the Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0).

https://creativecommons.org/licenses/by-nc-sa/4.0/
