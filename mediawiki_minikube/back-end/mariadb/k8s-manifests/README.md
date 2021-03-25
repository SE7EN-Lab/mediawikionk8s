Back End deployment
--------------------
- Pre-requisite:
    - Requires storage infrastructure on AWS to be provisioned to support Dynamic PV provisioning.

```
kubectl create -f k8s-manifests/
```
- Limitation:
    - Mariadb deployment isn't scalable due to race condition on Persistent volume usage. Requires statefulset deployment of Mariadb.

Note: Solution works on kubernetes v1.20