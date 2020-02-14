# Copied from https://github.com/soup-bowl/docker-wordpress-mssql
FROM wordpress:php7.2

# Obtain WP-CLI.
RUN curl https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar --output /usr/bin/wp \
	&& chmod +X /usr/bin/wp \
	&& chmod 766 /usr/bin/wp

# Self-signed SSL
RUN apt-get update \
    && apt-get install -y --no-install-recommends ssl-cert \
	&& rm -r /var/lib/apt/lists/* \
	&& a2enmod ssl \
	&& a2ensite default-ssl

# Run 'service apache2 reload' to load up.

# Email
COPY msmtprc /etc/msmtprc
RUN chmod 777 /etc/msmtprc
RUN export DEBIAN_FRONTEND=noninteractive \
	&& apt-get update \
	&& apt-get install -yq ca-certificates msmtp
RUN echo "sendmail_path = \"/usr/bin/msmtp -t\"" >> /usr/local/etc/php/conf.d/sendmail.ini

EXPOSE 80
EXPOSE 443