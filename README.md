Docker Registry + Joxit Docker Registry UI
===========================================
Here is simple example how to bind Docker Registry and Joxit Docker Registry UI

Goals
------
Configure own Docker Registry with 3rd party UI manager: [https://github.com/Joxit/docker-registry-ui](https://github.com/Joxit/docker-registry-ui)

Requirements
------------
- GNU Linux OS (recommended)
- Docker - [Install](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- Docker Compose - [Install](https://docs.docker.com/compose/install/#install-compose)

Commands
--------
- Run Docker compose: `docker-compose up -d`
- Stop Docker compose: `docker-compose down`

Configure custom registry
-------------------------

- Edit Docker config
	`/etc/docker/daemon.json`

- Modify file `docker.json`
```
{
  "insecure-registries" : ["localhost:5000"]
}
```
- Restat Docker `sudo service restart docker`

Test publish to Docker Registry
-------------------------------
- Pull example image: `docker pull busybox`
- Tag example image with `localhost:5000` which represents URL of custom Docker Registry: `docker tag busybox localhost:5000/busybox`
- Remove old image: `docker rmi busybox`
- Publish new image to custom Docker Registry `docker push localhost:5000/busybox`

Test pull from Docker Registry
-------------------------------
- Remove pushed image: `docker rmi localhost:5000/busybox`
- Pull pushed image: `docker pull localhost:5000/busybox`

Manage Docker Registry
-------------------------------
- List images via REST API: `http://localhost:5000/v2/_catalog`
- User Web UI: `http://localhost:8080/`
- Admin Web UI (allows delete images): `http://localhost:5050/`

Delete image
-------------------------------
- Delete via UI: `http://localhost:8080/`
- Run GC command: `docker exec registry-srv bin/registry garbage-collect /etc/docker/registry/config.yml`
