# Docker Container Commands Cheat Sheet

## How to create a container?

### docker create

Creates a container but **does not start** it.

```
docker create -p 8080:8080 --name javawebapp kkeducation12345/java-web-app
docker start javawebapp
```

### docker run

Creates and **starts** the container.

```
docker run -d -p 8080:8080 --name javawebapp1 kkeducation12345/java-web-app
```

---

## How to list running containers?

Running containers:

```
docker ps
docker container ls
```

All containers:

```
docker ps -a
docker container ls -a
```

---

## How to start and stop containers?

```
docker start <container_id>
docker stop <container_id>
```

---

## How to delete containers?

Stopped container:

```
docker rm <container_id>
```

Running container:

```
docker rm -f <container_id>
```

Delete multiple containers:

```
docker rm -f <cid> <cid>
```

Delete all running containers:

```
docker rm -f $(docker ps -q)
```

Delete all containers:

```
docker rm -f $(docker ps -aq)
```

---

## How to remove stopped containers?

```
docker container prune
```

OR

```
docker rm -f $(docker ps --filter status="exited" -aq)
```

---

## How to rename a container?

```
docker rename <old_name> <new_name>
```

---

## How to restart a container?

```
docker restart <container_id>
```

---

## Interview Question

### Difference between docker create and docker run?

* `docker create` → creates container only
* `docker run` → creates + starts container

---

## What is port publishing / port mapping?

Example:

```
docker run -p 8080:8080 image_name
```

Maps:

```
host_port : container_port
```

Example:

```
localhost:8080 → container:8080
```

---

## Difference between docker stop and docker kill?

### docker stop

* Sends `SIGTERM`
* Waits graceful shutdown
* Then sends `SIGKILL`

### docker kill

* Sends `SIGKILL` immediately
* No graceful shutdown

---

## How to pause container process?

```
docker pause <container_id>
docker unpause <container_id>
```

---

## Delete only paused containers

```
docker rm -f $(docker ps --filter status="paused")
```

---

## Example container lifecycle

```
docker run --name ubuntucontainer ubuntu
docker ps -a
docker start ubuntucontainer
```

---

## How to create container in interactive mode?

```
docker run -it --name test ubuntu /bin/bash
```

---

## Troubleshooting container applications

View logs:

```
docker logs <container_id>
```

Last few lines:

```
docker logs --tail 50 <container_id>
```

Live logs:

```
docker logs -f <container_id>
```

---

## How to enter inside container?

```
docker exec -it <container_id> /bin/bash
```

Run command inside container:

```
docker exec <container_id> ls
```

---

## Display running processes inside container

```
docker top <container_id>
```

---

## Display CPU and memory usage

```
docker stats <container_id>
```

---

## Interview Question

### Can we allocate custom memory to container?

Yes

Example:

```
docker run --memory="512m" -d -p 8082:8080 --name javawebapp2 kkeducation12345/java-web-app:1
```

If memory too low:

```
docker run --memory="64m" ...
```

Check status:

```
docker inspect <container_id>
```

Look for:

```
OOMKilled
```

---

## Copy files between system and container

Container → system

```
docker cp five:/usr/local/tomcat/logs/catalina.log catalina.log
```

System → container

```
docker cp demo.txt javawebapp:/usr/local/tomcat/
```

---

## Interview Question

### Difference between COPY and docker cp?

COPY:

* Dockerfile instruction
* Copies files while building image

docker cp:

* CLI command
* Copies files between container and host

---

## What is docker diff?

Shows file changes inside container

```
docker diff <container_id>
```

---

## What is docker commit?

Creates new image from container

```
docker commit <container_id> kkeducation12345/java-web-app:24
```

---

## How to inspect container?

```
docker inspect <container_id>
```

Get container IP:

```
docker inspect --format "{{.NetworkSettings.IPAddress}}" mavenapp
```

Get container PID:

```
docker inspect --format "{{.State.Pid}}" <container_id>
```

Get container IP and status:

```
docker inspect --format "{{.NetworkSettings.IPAddress}},{{.State.Status}}" <container_id>
```

Bridge network IP + container name:

```
docker inspect --format "IP Address: {{.NetworkSettings.Networks.bridge.IPAddress}}, Name: {{.Name}}" mavenapp
```
