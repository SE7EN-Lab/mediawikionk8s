Automation of Deployment of Distributed Mediawiki & MariaDB deployment on Kubernetes using Helm charts
------------------------------------------------------------------------------------------------------

Order of deployment
-------------------

1. helm install mariadb ./back-end/mariadb/Helm-charts/mariadb55-0.1.0.tgz
2. helm install mediawiki-fe ./front-end/httpd-php/Helm-charts/mediawiki-fe-0.1.0.tgz