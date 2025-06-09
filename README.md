# docker-mediathekview
[![GitHub-Issues](https://img.shields.io/github/issues/Minegamer06/docker-mediathekview-webinterface)](https://github.com/Minegamer06/docker-mediathekview-webinterface/issues)
[![GitHub-Releases](https://img.shields.io/github/tag/Minegamer06/docker-mediathekview-webinterface.svg)](https://github.com/Minegamer06/docker-mediathekview-webinterface/releases)

X11rdp Version of Mediathekview
## About
Using this container allows you to run Mediathekview as a service and control it via webbrowser like Firefox or Chrome.
The X11rdp feature is inherited from [https://github.com/jlesage/docker-baseimage-gui](https://github.com/jlesage/docker-baseimage-gui).

[MediathekView Releases GitHub](https://github.com/mediathekview/MediathekView/tags)

[MediathekView Changelog](https://mediathekview.de/tags/changelog/)

## Installation
### Manual

1. Download Dockerfile or clone the repository.
2. Run `docker build .` to create the docker image.
3. Wait until build process is finished.

### Pre-build
The Github repository is automatically build by Github Actions.
You can pull it from Docker Hub:
```
docker pull Minegamer06/mediathekview-webinterface:latest
```
Some older versions can also be acquired by using e.g.
```
docker pull Minegamer06/mediathekview-webinterface:14.2.0
```

## Running it
For additional configuration options, have a look at the [available environment-variables](https://github.com/jlesage/docker-baseimage-gui#environment-variables).
For basic usage, just use
```yaml
services:
  mediathekview-webinterface:
    image: ghcr.io/minegamer06/docker-mediathekview-webinterface:v14.3.1
    container_name: mediathekview-webinterface
    environment:
      - USER_ID=568 # Optional: Set to match Docker App user
      - GROUP_ID=568
    volumes:
      - ./config:/config
      - ./output:/output
    restart: unless-stopped
    mem_limit: 2500m # Optional: limit RAM usage
    cpus: 0.5 # Optional: limit CPU
  restart-cron: # Optional: Restarts the container regularly to trigger auto-downloads ("Abos") and a library scan on startup (auto-download must be enabled in the settings)
    image: docker:stable
    container_name: restart_cron
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock # ⚠️ Use with caution
    # Adjust the restart schedule for MediathekView (e.g. weekly on Monday at 10:00) to trigger auto-downloads and library refresh on container start
    command: >
      sh -c "echo '0 10 * * 1 docker restart mediathekview-webinterface' >
      /tmp/crontab &&
              crontab /tmp/crontab &&
              crond -f -l 8"
    restart: unless-stopped
```
