# docker-compose-radarr-sonarr-jackett-transmission
Quick setup for Radarr, Sonarr, Jackett and Transmission

## Requirements:

* Docker: https://docs.docker.com/install/linux/docker-ce/ubuntu/
* Docker-compose: https://docs.docker.com/compose/install/
* A HDD or USB mounted at /media, of the mountpath is different update the patch in docker-compose.yml

## Get started

Get a copy of the docker-compose.yml, could be copy pasting or cloning the repository with git:
```
git clone https://github.com/wilmardo/docker-compose-radarr-sonarr-jackett-transmission.git
cd docker-compose-radarr-sonarr-jackett-transmission
```


Then it is time to start the containers
* --compatibility is to convert deploy keys in v3 files to their non-Swarm equivalent
* --detach is for starting in the background)
```
docker-compose --compatibility up --detach
```

It should start pulling the containers like so:
```
Pulling transmission (haugene/transmission-openvpn:2.7)...
2.7: Pulling from haugene/transmission-openvpn
34667c7e4631: Downloading [===================>                               ]  16.84MB/43.56MB
d18d76a881a4: Download complete
119c7358fbfc: Download complete
2aaf13f3eff0: Download complete
753fbbfcaf8d: Downloading [=======>                                           ]  14.47MB/101.6MB
d5d1df01ebc2: Download complete
9a8d60353528: Download complete
650436cd178e: Download complete
b01b4d951636: Download complete
```

After this command completes like so:

```
Creating radarr               ... done
Creating sonarr               ... done
Creating transmission-openvpn ... done
Creating jackett              ... done
Creating samba                ... done
```

The services are available on the localhost addresses:

* Radarr: http://127.0.0.1:7878/
* Sonarr: http://127.0.0.1:8989/
* Jackett: http://127.0.0.1:9117/

When you are accessing the server from outside the server (over SSH for example) replace 127.0.0.1 with the IP of your server.

The SMB share is available on the same ip address without credentials. When credentials are needed look at these options:
https://github.com/dperson/samba#configuration

## Some usefull links and tips:

* Check the running containers with `docker ps`
* Check logs of the containers with `docker logs <container id>` container id is visible in the docker ps output.
  The name of the container can be used to `docker logs radarr`
* Setup all indexers at once in Jackett: https://gist.github.com/wilmardo/cffb41d694edd069c28d585d2e20e0fc

## Final note

Feel free to contribute to this repo! Create an issue or fork the repo and create a pull request with the changes:
https://www.digitalocean.com/community/tutorials/how-to-create-a-pull-request-on-github