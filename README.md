# docker-nginx_letsencrypt
Nginx with Let's Encrypt client (certbot) and crond for auto renew.

# Usage
## Launch server
Just run

`docker-compose up -d`

## Get new certificate (or add domains)
Run

`docker-compose exec reverse-proxy sh`

Then you will get into container's prompt.

And run

`certbot certonly --webroot -w /var/www/html -d YOUR_DOMAIN`

to get certificate.

## Renew certificate (Auto!!)
Renew job will be automatically run by crond on every Monday 0:00.

So you don't need to run manually.

Renew job will reload nginx to apply new certificates.

If it fails renewing, you can run `certbot renew` to investigate.
