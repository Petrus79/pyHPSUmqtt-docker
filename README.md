# pyHPSUmqtt-docker
Docker container for pyHPSU with MQTT daemon, available on Docker Hub at https://hub.docker.com/r/nerdix/pyhpsu_mqtt

```
docker pull nerdix/pyhpsu_mqtt
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
