FROM	nginx:1.28.0-alpine3.21
RUN	apk add certbot certbot-nginx && \
	echo '0 0 * * Mon certbot renew; kill 1 # Auto renew & reload nginx'	> /var/spool/cron/crontabs/root && \
	echo '* * * * * date +\%Y\%m\%d\%H > /healthcheck'			>> /var/spool/cron/crontabs/root
RUN	echo '#!/bin/sh'							>> /usr/local/bin/entrypoint.sh && \
	echo 'crond -d 8 -L /var/log/crond.log'					>> /usr/local/bin/entrypoint.sh && \
	echo 'if [ -z "`ls /etc/letsencrypt`" ]; then'				>> /usr/local/bin/entrypoint.sh && \
        echo '  echo No certificates exist!'					>> /usr/local/bin/entrypoint.sh && \
	echo '  echo Run \"certbot certonly --standalone --agree-tos -m YOUR_EMAIL_ADDRESS -d YOUR_PBX_DOMAIN --elliptic-curve secp384r1\" to get certificates' >> /usr/local/bin/entrypoint.sh && \
	echo '  echo "     or"'		>> /usr/local/bin/entrypoint.sh && \
	echo '  echo Run \"touch /etc/letsencrypt\" to supress this message.'	>> /usr/local/bin/entrypoint.sh && \
        echo '  exec sleep 15m'	>> /usr/local/bin/entrypoint.sh && \
	echo 'else'			>> /usr/local/bin/entrypoint.sh && \
	echo '  exec "$@"'		>> /usr/local/bin/entrypoint.sh && \
	echo 'fi'			>> /usr/local/bin/entrypoint.sh && \
	chmod 755 /usr/local/bin/entrypoint.sh && \
	mkdir -p /var/www/html && \
	echo '### Set ChaCha20-Poly1305 as Default Cipher Suite ###' && \
	sed -i '1iopenssl_conf = default_conf'		/etc/ssl/openssl.cnf && \
	sed -i '2i'					/etc/ssl/openssl.cnf && \
	sed -i '3i[default_conf]'			/etc/ssl/openssl.cnf && \
	sed -i '4issl_conf = ssl_sect'			/etc/ssl/openssl.cnf && \
	sed -i '5i'					/etc/ssl/openssl.cnf && \
	sed -i '6i[ssl_sect]'				/etc/ssl/openssl.cnf && \
	sed -i '7isystem_default = system_default_sect'	/etc/ssl/openssl.cnf && \
	sed -i '8i'					/etc/ssl/openssl.cnf && \
	sed -i '9i[system_default_sect]'		/etc/ssl/openssl.cnf && \
	sed -i '10iCiphersuites = TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384' /etc/ssl/openssl.cnf && \
	sed -i '11iOptions = ServerPreference' /etc/ssl/openssl.cnf

HEALTHCHECK --interval=120s --timeout=30s --start-period=5s --retries=3 CMD ["sh", "-c", "curl -sSo /dev/null ${HEALTHCHECK_URL} && grep $(date +%Y%m%d%H) /healthcheck"]
ENTRYPOINT	["entrypoint.sh"]
CMD	["nginx", "-g", "daemon off;"]
