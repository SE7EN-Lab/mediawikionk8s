Automation of Deployment of Distributed Mediawiki & MariaDB deployment on Kubernetes using Helm charts
------------------------------------------------------------------------------------------------------

Tech Spec:
 - OS: CentOS 7
 - Back-end:
    - mariadb: v5.5.41
 - Front-end:
    - httpd: v2.4
    - PHP: v7.3

- Pre-requisite:
    - Requires storage infrastructure on AWS to be provisioned to support Dynamic PV provisioning.
    - Requires External Load Balancer on AWS to be provisioned.
    - Cluster must be configured to launch External LoadBalancer on AWS.
    - Make sure Helm charts are packaged under Helm-charts directories.

Order of deployment
-------------------

1. Deploy back-end helm chart
    ```
    helm install mariadb ./back-end/mariadb/Helm-charts/mariadb55-0.1.0.tgz
    ```
2. Deploy front-end chart
    ``` 
    helm install mediawiki-fe ./front-end/httpd-php/Helm-charts/mediawiki-fe-0.1.0.tgz
    ```

- Mediawiki Application can be reached at 
    ```
    http://<EXTERNAL-IP-OF-LB>:8080
    ```
- Post-deployment:(One time action)
    - After completion of initial setup on Web - Download LocalSettings.php file.
        - Front-end deployment to be updated to add the following definition for mounting LocalSettings.php
        - .spec.volumeMounts.php-config
        ```
        volumeMounts:
         - name: php-config
           mountPath: /var/www/mediawiki/LocalSettings.php
           subPath: LocalSettings.php
        ```
        - .spec.volumes.php-config definition
        ```
        volumes:
        - name: php-config
          configMap:
            name: mediawiki-php-config
        ```
    - All pods of the deployment will re-started automatically.


- Limitation:
    - Mariadb deployment isn't scalable due to race condition on Persistent volume usage. Requires statefulset deployment of Mariadb.


Note: Solution works & tested on kubernetes v1.20