Docker image for transmission-daemon
# Works well with
- [dylanlindgren/docker-couchpotato](https://github.com/dylanlindgren/docker-couchpotato)
- [dylanlindgren/docker-sickbeard](https://github.com/dylanlindgren/docker-sickbeard)

# Preparation
1. Pull the image by running `docker pull dylanlindgren/docker-transmission`
2. Create a folder to keep torrent files and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/torrents`)
3. Create a folder to keep resume files and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/resume`)
4. Create a folder to keep downloads and and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/downloads`)

# Running the container
```
docker run -d --rm --privileged=true --name transmission -p 9091:9091 -p 51413:5143 -p 51413:51413/udp -v /data/torrents:/data/transmission/torrents:rw -v /data/resume:/data/transmission/resume:rw -v /data/downloads:/data/transmission/downloads:rw dylanlindgren/docker-transmission
```

## Starting automatically
To run this container as a service on a [Systemd](http://www.freedesktop.org/wiki/Software/systemd/) based distro (e.g. CentOS 7), create a unit file under `/etc/systemd/system` called `transmission.service` with the below contents (making sure you update the command in the line starting with `ExecStart` to the one you used above).

```bash
[Unit]
Description=Transmission-daemon docker container (dylanlindgren/docker-transmission)
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker stop transmission
ExecStartPre=-/usr/bin/docker rm transmission
ExecStartPre=-/usr/bin/docker pull dylanlindgren/docker-transmission
ExecStart=/usr/bin/docker run -d --rm --privileged=true --name transmission -p 9091:9091 -p 51413:5143 -p 51413:51413/udp -v /data/torrents:/data/transmission/torrents:rw -v /data/resume:/data/transmission/resume:rw -v /data/downloads:/data/transmission/downloads:rw dylanlindgren/docker-transmission dylanlindgren/docker-couchpotato
ExecStop=/usr/bin/docker stop transmission

[Install]
WantedBy=multi-user.target
```

Then you can start/stop/restart the container with the regular Systemd commands e.g. `systemctl start transmission.service`.

To automatically start the container when you restart enable the unit file with the command `systemctl enable transmission.service`
