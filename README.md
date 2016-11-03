# Sentry On-Premise

Official bootstrap for running your own [Sentry](https://sentry.io/) with [Docker](https://www.docker.com/).

## RCG AWS Instance Setup Information

EC2 Instance details:

* t2.small -> 2 GB RAM, 64 GB SSD HDD
* login user = `ubuntu`
* connect via `ssh ubuntu@sentry.rcgproduct.com`

Sentry Application Code:

* Lives at `/home/ubuntu/sentry-onpremise`
* Run via: `docker-compose up -d` (-d is for detached)
* Stop via: `docker-compose down`

Updating configuration values:

* environment variables can be set in the `docker-compose.yml` file and will be used by containers.
* If needing to set a new value in `sentry.conf.py`, you will need to:
    * `docker rmi sentryonpremise_web`
    * `docker-compose run --rm web upgrade`
    * `docker-compose down` (containers will get automatically restarted by the service)

Auto-start / Service:

* a systemd script was added at `/etc/systemd/system/sentry-docker.service` that uses the `up` and `down` commands listed above.

## Requirements

 * Docker 1.10.0+
 * Compose 1.6.0+ _(optional)_

## Up and Running

Assuming you've just cloned this repository, the following steps
will get you up and running in no time!

1. `mkdir -p data/{sentry,postgres}` - Make our local database and sentry config directories.
    This directory is bind-mounted with postgres so you don't lose state!
2. `docker-compose run --rm web config generate-secret-key` - Generate a secret key.
    Add it to `docker-compose.yml` in `base` as `SENTRY_SECRET_KEY`.
3. `docker-compose run --rm web upgrade` - Build the database.
    Use the interactive prompts to create a user account.
4. `docker-compose up -d` - Lift all services (detached/background mode).
5. Access your instance at `localhost:9000`!

Note that as long as you have your database bind-mounted, you should
be fine stopping and removing the containers without worry.

## Securing Sentry with SSL/TLS

If you'd like to protect your Sentry install with SSL/TLS, there are
fantastic SSL/TLS proxies like [HAProxy](http://www.haproxy.org/)
and [Nginx](http://nginx.org/).

## Resources

 * [Documentation](https://docs.sentry.io/server/installation/docker/)
 * [Bug Tracker](https://github.com/getsentry/onpremise)
 * [Forums](https://forum.sentry.io/c/on-premise)
 * [IRC](irc://chat.freenode.net/sentry) (chat.freenode.net, #sentry)
