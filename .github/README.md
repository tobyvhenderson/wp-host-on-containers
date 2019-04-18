[![Build status](https://dev.azure.com/collinbarrett/wp-host-on-containers/_apis/build/status/wp-host-on-containers-CI)](https://dev.azure.com/collinbarrett/wp-host-on-containers/_build/latest?definitionId=4)

# wp-host-on-containers

Continuously deploying WordPress sites in docker containers to a [$5/mo DigitalOcean Ubuntu VPS](https://m.do.co/c/fea63c0a77d1 "DigitalOcean Affiliate Link") via Azure Pipelines

A modern, containerized `v2` of [wp-vps-build-guide](https://github.com/collinbarrett/wp-vps-build-guide).

Note: This repo is primarily for my personal DevOps process and not meant to be directly re-usable by anyone. However, it could potentially serve as a reference for anyone trying to do something similar.

# The Stack

- GitHub
- Azure Pipelines
- [DigitalOcean](https://m.do.co/c/fea63c0a77d1 "DigitalOcean Affiliate Link")
- Ubuntu LTS
- Docker CE
- Docker Compose
- MariaDB
- php-fpm
- WordPress
- nginx
- Cloudflare

# Deployment Target Setup

## Initial Setup

1. Create a minimum-size Standard Droplet with the latest Ubuntu LTS.
    - Add backups.
    - Enable Monitoring.
    - Include a pre-configured SSH key.
2. Follow the DigitalOcean guide for [Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).
3. Set `PermitRootLogin` to `no` in `/etc/ssh/sshd_config`.
4. `sudo apt install fail2ban`
5. Enable [Ubuntu automatic updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html.en).

## Register with Azure DevOps

TBD

## Install Docker

1. See the [official docs](https://docs.docker.com/install/linux/docker-ce/ubuntu/) for installing on Ubuntu.
2. Complete the desired [linux postinstall procedures](https://docs.docker.com/install/linux/linux-postinstall/).

## Install Docker Compose

See the [official docs](https://docs.docker.com/compose/install/) for installing on linux.

## TLS Certificates

I use [Cloudflare's free ssl certificates](https://www.cloudflare.com/ssl/).

1. SFTP the following to `~/cert`:
    - domain-specific key `.pem` files
    - domain-specific cert `.pem` files
    - [Cloudflare Authenticated Origin Pull cert](https://support.cloudflare.com/hc/en-us/article_attachments/201243967/origin-pull-ca.pem)
2. `sudo openssl dhparam -out ~/cert/dhparam.pem 2048`
3. `sudo chmod -R 600 ~/cert`
4. `sudo chown root:root ~/cert`

# Azure Pipelines Setup

TBD

- Add a `.env` Secure File and corresponding task for downloading the MariaDB/WordPress secure environment variables.

# TODO List

- [X] Use secrets for database configurations.
- [ ] Limit permissions of WordPress database user.
- [ ] Implement backups of databases and files.
- [ ] Implement auto-updates of WordPress, plugins, and themes via [wp-cli](https://wp-cli.org/).
- [ ] Implement fastCGI page caching.
- [ ] Implement redis object caching.
- [ ] Implement miscellaneous nginx best practices for speed and security.
    - [h5bp](https://github.com/h5bp/server-configs-nginx)
- [ ] Implement [wp-sweep](https://github.com/lesterchan/wp-sweep) via [wp-cli](https://wp-cli.org/).
- [ ] Tune MariaDB instances for WordPress performance.