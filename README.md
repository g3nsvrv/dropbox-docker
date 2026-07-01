# Dropbox Docker

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

## TODO

* Mount paths and custom volumes

## Why use this?

Running Dropbox in Docker keeps the host system clean while allowing Dropbox synchronization for backups, shared folders, or automation workflows.

## License

This project is licensed under the Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International (CC BY-NC-SA 4.0) license.

You are free to share and adapt this work under the terms of the license, provided you give appropriate credit, do not use it for commercial purposes, and distribute derivatives under the same license.