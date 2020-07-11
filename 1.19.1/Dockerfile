FROM	nginx:1.19.1-alpine
RUN	apk add certbot certbot-apache && \
	echo '0 0 * * Mon certbot renew; kill 1 # Auto renew & reload nginx' > /var/spool/cron/crontabs/root && \
	echo '#!/bin/sh' >> /usr/local/bin/entrypoint.sh && \
	echo 'crond -d 8 -L /var/log/crond.log' >> /usr/local/bin/entrypoint.sh && \
	echo 'exec "$@"' >> /usr/local/bin/entrypoint.sh && \
	chmod 755 /usr/local/bin/entrypoint.sh && \
	mkdir -p /var/www/html

ENTRYPOINT	["entrypoint.sh"]
CMD	["nginx", "-g", "daemon off;"]
