# Docker image for TURN server

A Docker container with the [Coturn TURN server](https://github.com/coturn/coturn).

* hub.docker.com (Docker image): [boldt/coturn](https://hub.docker.com/r/boldt/coturn/)
* github.com (Repo): [boldt/turn-server-docker-image](https://github.com/boldt/turn-server-docker-image)

# Run the container

```bash
docker run \
  -d \
  -p 3478:3478 \
  -p 3478:3478/udp \
  -p 65435-65535:65435-65535/udp \
  --restart=always \
  --name coturn \
  boldt/coturn
```

## Environment variables

This image supports some environment variables:

* `USERNAME`: Username needed for turn. Defaults to `username`
* `PASSWORD`: Password needed for turn. Defaults ro `password`
* `REALM`: Realm needed for turn. Defaults to `realm`
* `MIN_PORT`: This defines the min-port for the range used by turn. Defaults to `65435`
* `MAX_PORT`: This defines the max-port for the range used by turn. Defaults to `65535`

An example:

```bash
# This makes sure, that the min- and max-port is the same for all environment variables
export MIN_PORT=50000
export MAX_PORT=50010
docker run \
  -d \
  -p 3478:3478 \
  -p 3478:3478/udp \
  -p ${MIN_PORT}-${MAX_PORT}:${MIN_PORT}-${MAX_PORT}/udp \
  -e USERNAME=another_user \
  -e PASSWORD=another_password \
  -e REALM=another_realm \
  -e MIN_PORT=${MIN_PORT} \
  -e MAX_PORT=${MAX_PORT} \
  --restart=always \
  --name coturn \
  boldt/coturn
```

## Certificates

Store the cert under `/opt/cert.pem` and the key under `/opt/pkey.pem` and mount them as volumes:

```bash
docker run \
  -d \
  -p 3478:3478 \
  -p 3478:3478/udp \
  -p 65435-65535:65435-65535/udp \
  --volume /opt/cert.pem:/etc/ssl/turn_server_cert.pem \
  --volume /opt/pkey.pem:/etc/ssl/turn_server_pkey.pem \
  --restart=always \
  --name coturn \
  boldt/coturn
```

## Debugging

```bash
docker logs coturn
docker exec -it coturn /bin/bash
```

# Build and push the container

(For own documentation)

```bash
# Clone
git clone git clone git@github.com:boldt/turn-server-docker-image.git

# Build
docker build -t boldt/coturn .

# Tag
VERSION=0.0.2
docker tag boldt/coturn boldt/coturn:$VERSION

# Push
docker push boldt/coturn:latest
docker push boldt/coturn:$VERSION
```

# Thanks

The initial image of this image was created by [boldt/turn-server-docker-image](https://github.com/boldt/turn-server-docker-image) and
The initial image of this image was created by [anastasiia-zolochevska/turn-server-docker-image](https://github.com/anastasiia-zolochevska/turn-server-docker-image)
