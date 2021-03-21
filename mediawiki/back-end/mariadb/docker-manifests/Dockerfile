FROM centos:centos7

LABEL Version=5.5.41

RUN yum -y install epel-release && \ 
    yum -y install mariadb-server bind-utils pwgen psmisc hostname net-tools iproute which wget && \ 
    yum -y update && yum clean all

RUN chgrp -R root /var/lib/mysql/ /var/log/mariadb/ /var/run/ && \
    chmod -R g+rw /var/lib/mysql/ /var/log/mariadb/ /var/run/ && \
    find /var/lib/mysql/ -type d -exec chmod g+x {} + && \
    find /var/log/mariadb/ -type d -exec chmod g+x {} + && \
    find /var/run/ -type d -exec chmod g+x {} +

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

VOLUME /var/lib/mysql

USER 27

EXPOSE 3306
CMD ["mysqld_safe"]