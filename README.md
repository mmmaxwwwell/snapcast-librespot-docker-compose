# [mmmaxwwwell/snapcast-librespot-docker-compose](https://github.com/mmmaxwwwell/snapcast-librespot-docker-compose)

Docker compose stack that provides a distributed time synchronized [Spotify Connect](https://www.spotify.com/us/connect/) speaker, wrapped in ssl. 

Doesn't play audio locally, requires web or native [snapcast](https://github.com/badaix/snapcast) clients.

## Prerequisites
* docker
* docker-compose
* amd64 arch computer (multiarch support coming soon)
* Spotify premium

## Features

* Web interface wrapped in SSL, A+ on [Qualys' SSL Server Test](https://www.ssllabs.com/ssltest/)
* Automatically obtains and renews ssl certificate, assuming traffic is forwarded correctly
* Provides a [Spotify Connect Speaker](https://www.spotify.com/us/connect/) on your network
* Distributed, time synchronized audio


## Usage

1. Clone this repo
2. Copy the settings.example folder and rename it to settings
3. Go through settings/common.env and fill out anything you want to change, specifically the following
  * HOSTED_URL
  * ADMIN_EMAIL
4. Generate a dhparam.pem for nginx by running ```mkdir -p appdata/nginx && openssl dhparam -out ./appdata/nginx/dhparam.pem 2048``` in the repo's root
5. Ensure traffic is routed and your DNS resolves correctly
6. Ensure req'd ports are open and not blocked by firewall. Defaults ports required are 80, 443 and 5454.
7. Run ```./start```
8. In a Spotify client, select the speaker, "librespot-docker" with the default settings/common.env, and play some music
9. In a browser, you should be able to go to "$SNAPSERVER_SUBDOMAIN.$HOSTED_URL", or "snapcast.example.com" with the default settings/common.env
10. Press the play icon in the top right hand corner of the screen and you should hear your Spotify connect stream
11. Repeat steps 8 and 9 with other devices

## Libraries 

* [badaix/snapcast](https://github.com/badaix/snapcast)
* [librespot-org/librespot](https://github.com/librespot-org/librespot)

## Ports

* 80 nginx http - only used by nginx for getting the ssl cert, OK to face the internet
* 443 nginx ssl - nginx ssl port, snapcast web server http interface will be available here after cert is obtained
* 1704 snapcast tcp - snapcast uses this port to communicate with other snapcast clients. I wouldn't expose this to the internet
* 1705 snapcast ??? - ???, ditto?
* 1780 snapcast http - snapcast http webserver, don't expose to internet
* 5454 zeroconf - used for communication with librespot, the spotify connect provider

## Tips

* Use wireguard and set $LISTEN_IP to the ip assigned to your wireguard interface
* You can listen on your router's public IP address with port 80 to obtain a cert, while keeping port 443 traffic local to your network.