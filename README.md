# CoolAcid's MISP Docker images

[![Build Status](https://travis-ci.org/coolacid/docker-misp.svg?branch=master)](https://travis-ci.org/coolacid/docker-misp)

A (nearly) production ready Dockered MISP

This is based on some of the work from the DSCO docker build, nearly all of the details have been rewritten.

- Components are split out where possible, currently this is only the MISP modules
- Over writable configuration files
- Allows volumes for file store
- Cron job runs updates, pushes, and pulls - Logs go to docker logs
- Docker-Compose uses off the shelf images for Redis and MySQL
- Images directly from docker hub, no build required
- Slimmed down images by using build stages and slim parent image, removes unnecessary files from images

# Docker Tags

[Docker hub](https://hub.docker.com/r/coolacid/docker-misp) builds the images automatically based on git tags. I try and tag using the following details

***v[MISP Version][Our build version]***

- MISP version is the MISP tag we're building
- Our build version is the iteration for our changes with the same MISP version

# Getting Started

## Development/Test

- Pull the repository
- Copy the "default" configs removing "default" and edit the files in `server-configs`
  - Note: A dry run without this step will try and make a sane DEV build for docker-compose
- Run `generate.sh` in `./ssl` to generate some fake certs
- `docker-compose up --build`
- Login with
  - User: admin@admin.test
  - Password: admin

## Production
- Use docker-compose, or some other config management tool
- Directory volume mount SSL Certs /etc/apache2/ssl/
  - DH Parameters: dhparams.pem
  - Certificate File: cert.pem
  - Certificate Key File: key.pem
  - Certificate Chain File: chain.pem
- Directory volume mount and create configs: /var/www/MISP/app/Config/
- Additional directory volume mounts:
  - /var/www/MISP/app/files
  - /var/www/MISP/.gnupg
  - /var/www/MISP/.smime

# Image file sizes

- Core server(Saved: 2.5GB)
  - Original Image: 3.17GB
  - First attempt: 2.24GB
  - Remove chown: 1.56GB
  - PreBuild python modules, and only pull submodules we need: 800MB
  - PreBuild PHP modules: 664MB


- Modules (Saved: 640MB)
  - Original: 1.36GB
  - Pre-build modules: 750MB
