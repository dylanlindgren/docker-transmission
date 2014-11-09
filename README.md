Docker image for transmission-daemon
# Works well with
- [dylanlindgren/docker-couchpotato](https://github.com/dylanlindgren/docker-couchpotato)
- [dylanlindgren/docker-sickbeard](https://github.com/dylanlindgren/docker-sickbeard)

# Preparing
1. Pull the image by running `docker pull dylanlindgren/docker-transmission`
2. Create a folder to keep torrent files and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/torrents`)
3. Create a folder to keep resume files and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/resume`)
4. Create a folder to keep downloads and and chown it to be owned by user ID 1000 (e.g. `sudo chown -R 1000 /data/downloads`)

# Running
```
docker run -d --rm --privileged=true -p 9091:9091 -p 51413:5143 -p 51413:51413/udp -v /data/torrents:/data/transmission/torrents:rw -v /data/resume:/data/transmission/resume:rw -v /data/downloads:/data/transmission/downloads:rw dylanlindgren/docker-transmission
```
