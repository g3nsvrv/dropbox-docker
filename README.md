# Dropbox Docker

<<<<<<< HEAD
Run the Linux Dropbox client inside Docker with persistent storage and configurable user mapping.

This project builds a Docker image that installs the Dropbox daemon on top of a configurable Debian-based image, creates a user with custom UID/GID values, and stores Dropbox data inside a persistent volume.

## Features

* Runs the official Linux Dropbox client inside Docker
* Configurable base image
* Custom user and group creation
* UID/GID mapping for host filesystem compatibility
* Persistent user home directory using Docker volumes
* Automatically downloads and installs Dropbox

## Project structure

```text
.
├── .env
├── docker-compose.yaml
└── dropbox/
    └── Dockerfile
```

## Configuration

Configuration is stored in `.env`:

```env
CONTEXT=dropbox
IMAGE=debian:stable-slim
USERACCOUNT=dropbox
HOSTNAME=docker-dropbox
UID=1000
GID=100
```

### Variables

| Variable      | Description                           |
| ------------- | ------------------------------------- |
| `CONTEXT`     | Docker build context                  |
| `IMAGE`       | Base image used for the build         |
| `USERACCOUNT` | Username created inside the container |
| `HOSTNAME`    | Container hostname                    |
| `UID`         | User ID used inside the container     |
| `GID`         | Group ID used inside the container    |

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

## Notes

* The image generates a random password for the created user during build time.
* The generated password is written to:

```text
/var/log/user-<username>.password
```

* Dropbox data persists between container recreations through the Docker volume.
=======
Run the Dropbox desktop client inside a Docker container with persistent storage and isolated dependencies.

This project provides a lightweight Docker image that allows Dropbox to run without installing it directly on the host system. It is useful for servers, NAS systems, and environments where you want Dropbox synchronization separated from the base OS.

## Features

* Run Dropbox inside Docker
* Persistent Dropbox configuration and synced files
* Host directory mounting support
* User/group mapping for file permission compatibility
* Keeps Dropbox isolated from the host system
* Suitable for self-hosted or headless environments

## Quick Start

Build the image:

```bash
docker build -t dropbox-docker .
```

Run the container:

```bash
docker run -d \
  --name dropbox \
  -v ./Dropbox:/home/dropbox/Dropbox \
  -v ./config:/home/dropbox/.dropbox \
  --restart unless-stopped \
  dropbox-docker
```

On first startup, obtain the authentication URL:

```bash
docker logs dropbox
```

Open the generated Dropbox authorization link in a browser and authorize the device.

## Notes

* The Dropbox client may require a supported Linux filesystem for synchronization.
* File permissions may require UID/GID adjustments depending on the host setup.
* Initial synchronization time depends on the amount of data in the account.
>>>>>>> 4d585d96fc0269ce1f0752b43f25197085ad4da7

## TODO

* Mount paths and custom volumes

<<<<<<< HEAD
## License

This project is licensed under the Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0).

https://creativecommons.org/licenses/by-nc-sa/4.0/
=======
## Why use this?

Running Dropbox in Docker keeps the host system clean while allowing Dropbox synchronization for backups, shared folders, or automation workflows.

## License

This project is licensed under the Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0) license.

You are free to share and adapt this work under the terms of the license, provided you give appropriate credit, do not use it for commercial purposes, and distribute derivatives under the same license.
>>>>>>> 4d585d96fc0269ce1f0752b43f25197085ad4da7
