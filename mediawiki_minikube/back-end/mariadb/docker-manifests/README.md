MariaDB Dockerfile
==================

Based on CentOS7, mariadb v55

Launching MariaDB
-----------------
```
docker run -d --name=<container-name> -p 3306:3306 -v <path/to/host/dir>:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=<root-password> -e MYSQL_DATABASE=<database-name> -e MYSQL_USER=<user-name> -e MYSQL_PASSWORD=<user-password> devopsworkslab/mariadb:1.0
```
