Automation of Deployment of Distributed Mediawiki & MariaDB deployment on Kubernetes using Helm charts
------------------------------------------------------------------------------------------------------

- Pre-requisite:
    - Requires storage infrastructure on AWS to be provisioned to support Dynamic PV provisioning.
    - Requires External Load Balancer on AWS to be provisioned.
    - Cluster must be configured to launch External Layer 7 LoadBalancer on AWS.

Order of deployment
-------------------

1. helm install mariadb ./back-end/mariadb/Helm-charts/mariadb55-0.1.0.tgz
2. helm install mediawiki-fe ./front-end/httpd-php/Helm-charts/mediawiki-fe-0.1.0.tgz

- Post-deployment:
    - Line #40 of ConfigMap "mediawiki-php-config" must be updated to point to External IP of the Loadbalancer.

- Limitation:
    - Mariadb deployment isn't scalable due to race condition on Persistent volume usage. Requires statefulset deployment of Mariadb.


Note: Solution works & tested on kubernetes v1.20