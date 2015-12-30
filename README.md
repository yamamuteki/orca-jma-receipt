# Dockerfile for the ORCA server

## Supported tags and respective `Dockerfile` links
* [`4.5` (4.5/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/4.5/Dockerfile)
* [`4.6` (4.6/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/4.6/Dockerfile)
* [`4.7` (4.7/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/4.7/Dockerfile)
* [`4.8`, `latest` (4.8/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/4.8/Dockerfile)

## About this Repo

This is the Git repo of the Docker image for the ORCA (Online Receipt Computer Advantage) server in a development environment. See the Docker Hub page for the full readme on how to use this Docker image and for information regarding contributing and issues.

* Reference: <http://www.orca.med.or.jp/receipt/>
* GitHub page: <https://github.com/yamamuteki/orca-jma-receipt>
* Docker Hub page: <https://hub.docker.com/r/yamamuteki/orca-jma-receipt/>

## How to use this Dockerfile (only build image on yourself)

```bash
git clone https://github.com/yamamuteki/orca-jma-receipt
cd orca-jma-receipt/4.8
docker build -t yamamuteki/orca-jma-receipt:4.8
```

## How to use the Docker image (recommend: use the Docker hub image)

Use services:

```bash
docker run -d -p 8000:8000 yamamuteki/orca-jma-receipt
```

Explore in this docker image: (NOTICE: Not starting services)

```bash
docker run -it -p 8000:8000 yamamuteki/orca-jma-receipt /bin/bash
```

Explore in running docker container already:

```bash
docker exec -it <CONTAINER_ID> /bin/bash
```

## How to connect your ORCA client

1. Download and install Java SE 7 or later
2. Download and install Monsiaj
3. Input your docker-machine ip and your binding port (ex. 8000)

Reference: <http://www.orca.med.or.jp/receipt/download/java-client/>

