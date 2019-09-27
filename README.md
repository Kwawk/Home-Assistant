# How to setup my Hassbian.

## Global Setup.

* Install hassbian on a micro SD card: https://www.home-assistant.io/docs/installation/hassbian/installation/
* Put the micro SD card in your rasbperry
* On your browser, http://hassbian.local:8123 should show Home assistant.
* SSH on your raspberry: `ssh pi@192.168.1.30` (Password is `rapsberry` by default)
* Copy/paste all yaml files of this repository to /home/homeassistant/.homeassistant.
* Reboot: `su pi & sudo systemctl reboot`


---

## Devices to add using Home Assistant discovery.

In fact, I am not sure `configuration.yaml` is enough for thoses devices:
* Google cast
* Meteo
* iOS Home assistant
* Philips Hue bridge
* Deconz

---

## Raspbee setup (with Deconz).

### Sources:
* https://phoscon.de/en/raspbee/install
* https://github.com/marthoc/docker-deconz
* https://hub.docker.com/r/marthoc/deconz
* https://www.home-assistant.io/components/deconz/

### Steps:

* Install Raspbee in Raspberry pi (See first link above).
* SSH on your raspberry: `ssh pi@192.168.1.30` (Password is `rapsberry` by default)
* add your Linux user to the dialout group, which allows the user access to serial devices (i.e. Conbee/Conbee II/RaspBee): `sudo usermod -a -G dialout $USER`
* Download and install docker: `curl -sSL get.docker.com | sh`
* Enable serial port hardware. To do so: `sudo raspi-config`
```
In the menu, select "Interfacing Options".
Select "P6 Serial".
“Would you like a login shell to be accessible over serial?” Select No.
“Would you like the serial port hardware to be enabled?” Select Yes.
Exit raspi-config (escape).
```

* Reboot: `su pi & sudo systemctl reboot`
* To swap Bluetooth to /dev/S0 (moving RaspBee to /dev/ttyAMA0): `echo 'dtoverlay=pi3-miniuart-bt' | sudo tee -a /boot/config.txt`
* Reboot again: `su pi & sudo systemctl reboot`
* Pull Deconz docker: `sudo docker pull marthoc/deconz`
* Install and run it:
```
sudo docker run -d \
    --name=deconz \
    --net=host \
    --restart=always \
    -v /etc/localtime:/etc/localtime:ro \
    -v /opt/deconz:/root/.local/share/dresden-elektronik/deCONZ \
    --device=/dev/ttyAMA0 \
    marthoc/deconz
```

* In your web browser, go to http://dresden-elektronik.de/pwa/
* Add your devices. I did it via lights tab.
* They should now appear on Home assistant (Unused entities) :-)


---

## BLE Tracker setup.

### Sources:
* https://www.home-assistant.io/components/bluetooth_le_tracker/

### Steps:

* SSH on your raspberry: `ssh pi@192.168.1.30` (Password is `rapsberry` by default)
* $ sudo apt-get install libcap2-bin
* $ sudo setcap 'cap_net_raw,cap_net_admin+eip' `readlink -f \`which python3\``
* $ sudo setcap 'cap_net_raw+ep' `readlink -f \`which hcitool\``


--- 

## LG TV WebOS setup.

### Sources:
* https://www.home-assistant.io/components/webostv/
* https://www.lg.com/uk/support/product-help/CT00008334-1437131798537-others

### Steps:

A notification should be visible in the frontend’s Notification section. Follow the instructions and accept the pairing request on your TV.
Use above lg link if needed.

---

## Additional notes.

* To start it again in case of problem: `sudo docker start deconz`
* You will have to check everything is working (automations...) since some device ids could be different?
* To log as home assistant user: `sudo -u homeassistant -H -s`
* Go to Home assistant folder: `cd /home/homeassistant/.homeassistant/`

