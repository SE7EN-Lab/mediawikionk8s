Mediawiki Dockerfile
==================

Based on CentOS7, mediawiki v1.35.1

Launching Mediawiki
-------------------
- Ensure DB container is launched first before executing the below command line
- LocalSettings.php must be mounted to the container post intial setup of mediawiki

```
docker run -d --name=<container-name> devopsworkslab/mediawiki-fe:1.0
```