# pyHPSUmqtt-docker
Docker container for pyHPSU with MQTT daemon, available on Docker Hub at https://hub.docker.com/r/nerdix/pyhpsu_mqtt

```
$ mkdird pyHPSU
$ docker run \
        --name pyHPSU \
        -v /home/pi/pyHPSU/etc:/etc/pyHPSU \
        -d \
        -it \
        --network host \
	      --restart=on-failure:5o \
        nerdix/pyhpsu_mqtt:arm32v7
        #OR nerdix/pyhpsu_mqtt:amd64
```

## HOST preparation
### Raspberry Pi
Working adapter: https://www.waveshare.com/rs485-can-hat.htm

adjust /boot/config.txt:
```
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=12000000,interrupt=25,spimaxfrequency=2000000
# Might not be needed any longer
# dtoverlay=spi-bcm2835-overlay
```
