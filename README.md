# docker-compose-radarr-sonarr-prowlarr-transmission
Quick setup for Radarr, Sonarr, Prowlarr and Transmission made for a friend at some point and open sourced.

## Requirements:

* Docker: https://docs.docker.com/engine/install/
* Docker compose: https://docs.docker.com/compose/install/linux/#install-using-the-repository
* A HDD or USB mounted at /media, of the mountpath is different update the path in docker-compose.yml under `volumes:`
  https://raspberrypi-guide.github.io/filesharing/mounting-external-drive

TLDR: run these on a new install of **Ubuntu** to install Docker and add your user to the docker group:
```
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    git

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo usermod -aG docker $USER
```

## Get started

If you didn't use the above TLDR, make sure you did the [linux-postinstall](https://docs.docker.com/engine/install/linux-postinstall/) steps.
Make sure after the above usermod you logged in and out once (type `exit` on the terminal).

Get a copy of the docker-compose.yml, could be copy pasting or cloning the repository with git:
```
git clone https://github.com/wilmardo/docker-compose-radarr-sonarr-prowlarr-transmission.git
cd docker-compose-radarr-sonarr-prowlarr-transmission
```

In the `docker-compose.yml` you must setup the `OPENVPN_` variables under the `environment:` key.
See here for more information: https://haugene.github.io/docker-transmission-openvpn/config-options/

When completed it is time to start the containers
* --compatibility is to convert deploy keys in v3 files to their non-Swarm equivalent
* --detach is for starting in the background)
```
docker compose --compatibility up --detach
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
Creating prowlarr             ... done
Creating samba                ... done
```

The services are available on the localhost addresses:

* Radarr: http://127.0.0.1:7878/
* Sonarr: http://127.0.0.1:8989/
* Prowlarr: http://127.0.0.1:9696/

When you are accessing the server from outside the server (over SSH for example) replace 127.0.0.1 with the IP of your server.

The SMB share is available on the same ip address without credentials. When credentials are needed look at these options:
https://github.com/dperson/samba#configuration

If you didn't setup credentials it is as easy as entering `\\<ip>\Media` in the Windows explorer bar. See this link for some more information:
https://www.techrepublic.com/article/how-to-connect-to-linux-samba-shares-from-windows-10/

## Some useful links and tips:

* Check the running containers with `docker compose ps`
* Check logs of the containers with `docker compose logs <container name>` container name is visible in the docker ps output.
  For example: `docker compose logs radarr`
* Container can connect between eachother on their name in the compose file, for example `http://prowlarr:9696` as Prowlarr Server and `http://radarr:7878` as Radarr server.
* Add Sonarr and Radarr as clients in Prowlarr to automatically setup indexers in them (settings>apps)
* Update the containers to their latest version with `docker compose pull` and `docker compose up -d`

## Final note

Feel free to contribute to this repo! Create an issue or fork the repo and create a pull request with the changes:
https://www.digitalocean.com/community/tutorials/how-to-create-a-pull-request-on-github
