# Learning jenkins for automate deployment

## Installing jenkins

Creating a custom image of jenkins following the official documentation on how to install with docker.
[Click here](https://www.jenkins.io/doc/book/installing/docker/)

### Build custom image

```
docker build -t myjenkins-blueocean:2.387 .
```

### Create jenkins network

```
docker network create jenkins
docker network ls
```

### Run the container

```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.387
```

### Sign in

```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## Deploy typescript project

Installing nodejs and npm for automate deployment of the typescript project.

### Connect to container with root user and install npm

```
docker exec -u 0 -it jenkins-blueocean bash
curl -fsSL https://deb.nodesource.com/setup_19.x | bash -
apt install -y nodejs
```
