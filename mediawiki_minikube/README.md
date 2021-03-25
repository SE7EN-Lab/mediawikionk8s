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
    - minikube v1.17.1
    - Local host volume infrastructure provisioned.
    - Make sure Helm charts are packaged under Helm-charts directories.

Order of deployment
-------------------
1. Start minikube
    ```
    minikube start --host-only-cidr 192.168.99.1/24
    ```
2. Provision local host volume directories on minikube VM
    ```
    minikube ssh

    sudo mkdir -p /var/lib/mysql
    sudo chmod -R 777 /var/lib/mysql
    sudo mkdir -p /var/www/mediawiki/images
    sudo chmod -R 777 /var/www/mediawiki/images
    ls -ltr /var/lib/mysql /var/www/mediawiki/images
    exit
    ```
3. Deploy back-end helm chart
    ```
    helm install mariadb ./back-end/mariadb/Helm-charts/mariadb55-0.1.0.tgz
    ```
4. Deploy front-end chart
    ``` 
    helm install mediawiki-fe ./front-end/httpd-php/Helm-charts/mediawiki-fe-0.1.0.tgz
    ```
5. Expose Front-end service. From a seperate terminal window.
    ```
    minikube tunnel --cleanup
    ```
6. Mediawiki Application can be reached at 
    ```
    http://<EXTERNAL-IP-OF-LB>:8080
    ```
7. Post-deployment: (One time action)
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