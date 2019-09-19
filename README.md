# Home-Assistant-configuration

## Links:

Hassbian setup:
- https://www.home-assistant.io/docs/installation/hassbian/installation/
- hassbian.local:8123/lovelace

Deconz:
- https://www.home-assistant.io/components/deconz/
- https://hub.docker.com/r/marthoc/deconz
- http://dresden-elektronik.de/pwa/

## Commands

Reboot pi:
`su pi & sudo systemctl reboot`

Create docker:
`docker run -d --name=deconz --net=host --restart=always -v /etc/localtime:/etc/localtime:ro -v /opt/deconz:/root/.local/share/dresden-elektronik/deCONZ --device=/dev/ttyAMA0 marthoc/deconz`

Start docker:
`docker start deconz`

Log as home assistant user:
`sudo -u homeassistant -H -s`

Go to home assistant folder:
`cd /home/homeassistant/.homeassistant/`
