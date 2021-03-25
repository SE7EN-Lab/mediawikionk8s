FROM centos:centos7

LABEL Frontend=httpd-php

#Install system dependencies
RUN yum -y install epel-release yum-utils && \
    yum -y install net-tools iproute which wget && \
# Install httpd 2.4
    yum -y install httpd httpd-tools && \
# Install PHP 7.3
    yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    yum-config-manager --enable remi-php73 && \
    yum -y install php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqlnd php-xml php-mbstring && \
    yum -y install mariadb-server mariadb && \
    yum -y update && yum clean all

#Mediawiki setup
ENV MEDIAWIKI_MAJOR_VERSION 1.35
ENV MEDIAWIKI_VERSION 1.35.1

RUN curl -fSL "https://releases.wikimedia.org/mediawiki/${MEDIAWIKI_MAJOR_VERSION}/mediawiki-${MEDIAWIKI_VERSION}.tar.gz" -o mediawiki.tar.gz && \
    tar -zxf mediawiki.tar.gz && \
    mv "mediawiki-${MEDIAWIKI_VERSION}" mediawiki && \
    mv mediawiki /var/www/ && \
# Fix permissions
    chown -R apache:apache /var/www/mediawiki/ && chmod -R 775 /var/www/mediawiki/ && \
    rm -rf mediawiki.tar.gz

#Setup httpd config
COPY httpd.conf /etc/httpd/conf/httpd.conf
RUN chown root:root /etc/httpd/conf/httpd.conf && chmod 644 /etc/httpd/conf/httpd.conf

#Local settings for PHP
#COPY LocalSettings.php /var/www/mediawiki/
#RUN chown apache:apache /var/www/mediawiki/LocalSettings.php && chmod 775 /var/www/mediawiki/LocalSettings.php
EXPOSE 80

ENTRYPOINT ["/usr/sbin/httpd"]

CMD ["-D", "FOREGROUND"]