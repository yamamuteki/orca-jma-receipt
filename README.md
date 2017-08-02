# Dockerfile for the ORCA server

## Supported tags and respective `Dockerfile` links
* [`xenial50`, `5.0`, `latest` (xenial50/Dockerfile)](https://github.com/yamamuteki/orca/jma-receipt/blob/master/xenial50/Dockerfile)
* [`trusty48`, `4.8`, (trusty48/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/trusty48/Dockerfile)
* [`precise48`, (precise48/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/precise48/Dockerfile)
* [`precise47`, `4.7`, (precise47/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/precise47/Dockerfile)
* [`lucid47`, (lucid47/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/lucid47/Dockerfile)
* [`lucid46`, `4.6`, (lucid46/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/lucid46/Dockerfile)
* [`lucid45`, `4.5`, (lucid45/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/lucid45/Dockerfile)
* [`etch45`, (etch45/Dockerfile)](https://github.com/yamamuteki/orca-jma-receipt/blob/master/etch45/Dockerfile)

## About this Repo

This is the Git repo of the Docker image for the ORCA (Online Receipt Computer Advantage) server in a development environment. See the Docker Hub page for the full readme on how to use this Docker image and for information regarding contributing and issues.

* Reference: <http://www.orca.med.or.jp/receipt/>
* GitHub page: <https://github.com/yamamuteki/orca-jma-receipt>
* Docker Hub page: <https://hub.docker.com/r/yamamuteki/orca-jma-receipt/>

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
  - [for jma-receipt v5.0 or later](https://www.orca.med.or.jp/receipt/download/java-client2/)
  - [for jma-receipt prior to v5.0](https://www.orca.med.or.jp/receipt/download/java-client/)
3. Input your docker-machine ip and your binding port (ex. 8000)

Reference: <http://www.orca.med.or.jp/receipt/download/java-client/>

## How to use this Dockerfile (only build image on yourself)

```bash
git clone https://github.com/yamamuteki/orca-jma-receipt
cd orca-jma-receipt/trusty48
docker build -t yamamuteki/orca-jma-receipt:trusty48
```
