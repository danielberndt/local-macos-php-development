# What does it do?

Assume all your wordpress/php projects are located within a `_Sites` folder.
Put this repo into the parent directory and execute the steps below. You'll now be able to open http://[SUBDIRNAME].test into your browser and see the contents of `_Sites/SUBDIRNAME` rendered by php.

# SETUP

## Start the docker container

- `docker-compose up`
  this exposes php-apache via port 8056, the db contents are stored within the ./db folder. This makes it very easy to migrate the database from one machine to another by just copying file contents.

## Setup dnsmasq

- `brew install dnsmasq`
- `sudo mkdir /etc/resolver`
- `sudo sh -c 'echo "nameserver 127.0.0.1" >> /etc/resolver/test'`
- `sudo brew services start dnsmasq`
- `cd $(brew --prefix)`
- `echo 'address=/test/127.0.0.1' >> etc/dnsmasq.conf`
- `sudo brew services start dnsmasq`

- Restart the machine 😒

Now `[anydomain].test` will be forwarded to localhost.

## Setup caddy

- `brew install caddy`
- create a `/usr/local/etc/Caddyfile` with these contents
  ```
  http://*.test {
    reverse_proxy 127.0.0.1:8056
  }
  ```
- `brew services start caddy`

Now `[anydomain].test` will be forwarded to `[anydomain].test:8056` where the docker container above will be awaiting.
