# MegaCMD Docker image

[![Docker Automated build](https://img.shields.io/docker/automated/ruslanys/megacmd.svg?style=flat-square&colorB=007EC6)](https://hub.docker.com/r/ruslanys/megacmd/) [![Docker Pulls](https://img.shields.io/docker/pulls/ruslanys/megacmd.svg?style=flat-square&colorB=007EC6)](https://hub.docker.com/r/ruslanys/megacmd/)

## How it works

1. Run container with Mega CMD server and mounted configuration and data directories.
2. Obtain session using your login and password.
3. Execute necessary commands.

## Start a Daemon

Because of the issue [#623](https://github.com/meganz/MEGAcmd/issues/623)
we need to have an unique UUID at `/etc/machine-id`. Otherwise, `mega-sync` will not work.

We have 2 options: 

1. Bypass the `machine-id` from the host.

```
docker run -d --name megacmd --restart always \
    -v /etc/machine-id:/etc/machine-id:ro \
    -v /opt/MEGA/config:/root/.megaCmd \
    -v /opt/MEGA/data:/root/MEGA \
    ruslanys/megacmd
```

2. Generate a new UUID for the container.

```
docker run -d --name megacmd --restart always \
    -v /opt/MEGA/config:/root/.megaCmd \
    -v /opt/MEGA/data:/root/MEGA \
    ruslanys/megacmd
```

```
docker exec -it megacmd bash
cat /proc/sys/kernel/random/uuid > /etc/machine-id
```

## Commands Execution

```
docker exec -it megacmd mega-login $username $password
docker exec -it megacmd mega-sync /root/MEGA /
```
