FROM php:7.2-apache

RUN apt-get update > /dev/null && apt-get install -y git openssh-client openssh-server zip unzip build-essential
RUN docker-php-ext-install pdo pdo_mysql
RUN curl -s -f -L -o /tmp/installer.php https://raw.githubusercontent.com/composer/getcomposer.org/b107d959a5924af895807021fcef4ffec5a76aa9/web/installer \
    && php -r " \
    \$signature = '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061'; \
    \$hash = hash('SHA384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
        unlink('/tmp/installer.php'); \
        echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
        exit(1); \
    }" \
    && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer
    
RUN mkdir /var/www/html/server
RUN mkdir /var/www/html/client

RUN apt-get update && \
    apt-get install -y wkhtmltopdf xvfb xauth xfonts-base xfonts-75dpi fontconfig  && \
    echo 'xvfb-run --server-args="-screen 0, 1024x768x24" /usr/bin/wkhtmltopdf $*' > /usr/bin/wkhtmltopdf.sh && \
    chmod a+rx /usr/bin/wkhtmltopdf.sh && \
    ln -s /usr/bin/wkhtmltopdf.sh /usr/local/sbin/wkhtmltopdf

RUN a2enmod rewrite

VOLUME [ "/var/www/html/server" ]
VOLUME [ "/var/www/html/client" ]
VOLUME [ "/etc/apache2/sites-available" ]

WORKDIR /var/www/html/server

CMD ["apache2-foreground"]