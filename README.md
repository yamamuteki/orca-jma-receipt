# Dockerfile for the ORCA server

## About this Repo

This is the Git repo of the Docker image for the ORCA(Online Receipt Computer omputer Advantage dvantage) server in a development environment.

Reference: [http://www.orca.med.or.jp/receipt/download/trusty/]

## How to use this Dockerfile (only build image on yourself)

```bash
git clone https://github.com/yamamuteki/orca-jma-receipt
cd orca-jma-receipt
docker build -t yamamuteki/orca-jma-receipt 4.8
```

## How to use the Docker image (recommend: use Docker hub)

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

1. Download and Install the Java SE 7 or later
2. Download and Install the Monsiaj
3. Input your docker-machine ip and your binding port (ex. 8000)

Reference: [http://www.orca.med.or.jp/receipt/download/java-client/]

