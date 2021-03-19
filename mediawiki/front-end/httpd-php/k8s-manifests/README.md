Front End deployment
--------------------
- Pre-requisite:
    - Requires storage infrastructure on AWS to be provisioned to support Dynamic PV provisioning.
    - Requires External Load Balancer on AWS to be provisioned
    - Cluster must be configured to launch External Layer 7 LoadBalancer on AWS

```
kubectl create -f k8s-manifests/
```
- Post Deployment:
    - Line #40 of ConfigMap "mediawiki-php-config" must be updated to point to External IP of the Loadbalancer


Note: Solution works on kubernetes v1.20
