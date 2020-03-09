# Docker media server

## Containers

- Traefik - reverse proxy
- Jellyfin - media server
- Sonnarr - tv downloader
- Radarr - movie downloader
- Ombi - requester for radarr & sonarr
- Deluge - torrent client, required for downloaders to work
- Jackett - Torrent tracker api, required for downloaders to work


## Setup

These instructions all assume that you're on a linux machine that already has docker and docker compose setup.

After cloning this repo...

1. Copy the example.env and give it your own variables

```bash
cp example.env .env
nano .env
```

2. Create the folders needed for the setup, replace the vars with actual directories unless you've put them in the bash environment

```bash
mkdir -p jellyfin/config jellyfin/cache sonarr radarr ombi deluge jackett ${DLDIR}/completed ${DLDIR}/incomplete ${MOVIESDIR} ${TVDIR}
```

(I've seen that some people need to change the permissions on the content folder, if you do `chmod -R 0777 content/` should work, although changing folder ownership is probs a better option)

3. Setup your DNS so that all \*.yourdomain go to the media server. Your domain doesn't have to be registered since it will only be used internally

4. Fire up the docker containers

```bash
# Make sure you're running this command from inside the docker-media-server folder and that the .env file is inside the same folder
sudo docker-compose up -d
# The command will take a bit, after it runs you can run the following to check all the containers are running
sudo docker-compose ps
```

If this is all setup correctly visiting your domain or the ip address of your machine should open Heimdall, and `<your ip>:8112` should open the Deluge web ui

5. Setup all the containers

   - Deluge needs to be setup to download to `/downloads/incomplete` and move completed downloads to `/downloads/completed`
   - You need to add some torrent trackers to Jackett
   - For sonarr and radarr you need to connect them to deluge and to the trackers you set up on Jackett. You may also need to click add movie/add series and setup the path to be `/movies` and `/tv` respectively
   - Ombi needs to be connected to sonarr, radarr and jellyfin. You could also set up passwordless login if you're only going to be using it on your network
   - IIRC you just need to go through the setup wizard for Jellyfin
   - Add links to everything on Heimdall
