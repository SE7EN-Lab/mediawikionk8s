FROM centos:centos7

LABEL Version=5.5.41

RUN yum -y install epel-release && \ 
    yum -y install mariadb-server bind-utils pwgen psmisc hostname net-tools iproute which wget && \ 
    yum -y update && yum clean all


# Fix permissions
RUN chgrp -R 0 /var/lib/mysql/ && chmod -R 775 /var/lib/mysql/ && \
    chgrp -R 0 /var/log/mariadb/ && chmod -R 775 /var/log/mariadb/ && \
    chgrp -R 0 /var/run/ && chmod -R 775 /var/run/

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

VOLUME /var/lib/mysql

USER 27

EXPOSE 3306
CMD ["mysqld_safe"]