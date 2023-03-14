[![GitHub license](https://img.shields.io/github/license/fboaventura/dckr-cupsd)](https://github.com/fboaventura/dckr-cupsd/blob/master/LICENSE)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Ffboaventura%2Fdckr-cupsd.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Ffboaventura%2Fdckr-cupsd?ref=badge_shield)
[![DockerPulls](https://img.shields.io/docker/pulls/fboaventura/dckr-cupsd.svg)](https://hub.docker.com/r/fboaventura/dckr-cupsd)
[![DockerPulls](https://img.shields.io/docker/stars/fboaventura/dckr-cupsd.svg)](https://hub.docker.com/r/fboaventura/dckr-cupsd)
[![GitHub forks](https://img.shields.io/github/forks/fboaventura/dckr-cupsd)](https://github.com/fboaventura/dckr-cupsd/network)
[![GitHub stars](https://img.shields.io/github/stars/fboaventura/dckr-cupsd)](https://github.com/fboaventura/dckr-cupsd/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/fboaventura/dckr-cupsd)](https://github.com/fboaventura/dckr-cupsd/issues)
[![dockeri.co](https://dockeri.co/image/fboaventura/dckr-httpd)](https://hub.docker.com/r/fboaventura/dckr-httpd)

# dckr-cupsd

I needed a CUPS server to share my printer on my Home Network. Setting up old printers in new Macs posed a good challenge, and I don't have that much time to lose.

I already have a Kubernetes cluster running on my Raspberry Pi 4, so I decided to run CUPS in a container. I found this [quick tutorial](https://gist.github.com/svanellewee/ec25c234b61213710771bf25d1cc45d0) 
and copied the [Dockerfile](https://gist.github.com/svanellewee/ec25c234b61213710771bf25d1cc45d0#file-dockerfile) here to have the image in a place I can control.

## Usage

### Build the image

```bash
docker build -t dckr-cupsd .
```

### Run the container

```bash
docker run -d --name cupsd -p 631:631 -v /var/run/dbus:/var/run/dbus -v /var/run/cups:/var/run/cups -v /var/log/cups:/var/log/cups -v /etc/cups:/etc/cups -v /etc/avahi/services:/etc/avahi/services dckr-cupsd
```

### Add a printer

```bash
docker exec cupsd lpadmin -p <printer_name> -E -v <printer_uri> -m everywhere
```

### Remove a printer

```bash
docker exec cupsd lpadmin -x <printer_name>
```

### List printers

```bash
docker exec cupsd lpstat -p -d
```

### Print a test page

```bash
docker exec cupsd lp -d <printer_name> /usr/share/cups/data/testprint
```

### Print a file

```bash
docker exec cupsd lp -d <printer_name> <file>
```

### Print a file with options

```bash
docker exec cupsd lp -d <printer_name> -o <option>=<value> <file>
```

# Changelog

## v1.0.0 - 2023.03.14

- Initial release
- CUPS 2.3.3
- 
