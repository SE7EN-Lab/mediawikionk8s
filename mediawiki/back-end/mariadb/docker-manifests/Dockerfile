FROM centos:centos7

LABEL Version=5.5.41

RUN yum -y install epel-release && \ 
    yum -y install mariadb-server bind-utils pwgen psmisc hostname net-tools iproute which wget && \ 
    yum -y update && yum clean all

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

VOLUME /var/lib/mysql

EXPOSE 3306
CMD ["mysqld_safe"]