dump1090mqtt
============

Publish [dump1090](http://www.satsignal.eu/raspberry-pi/dump1090.html) output to MQTT

The `topic` is `/adsb/RADAR/ICAOADDR` where 

* `RADAR` is specifed by `--radar` option which is your radar's ID
* `ICAOADDR` is [ICAO 24-bit Aircraft Address](http://en.wikipedia.org/wiki/Aviation_transponder_interrogation_modes#ICAO_24-bit_address)

usage
------

```
$ ./dump1090pub.py --help
Usage: dump1090pub.py [options]

Options:
  -h, --help            show this help message and exit
  -m MQTT_HOST, --mqtt-host=MQTT_HOST
                        MQTT broker hostname
  -p MQTT_PORT, --mqtt-port=MQTT_PORT
                        MQTT broker port number
  -u MQTT_USER, --mqtt-user=MQTT_USER
                        MQTT broker connect user
  -a MQTT_PASSWORD, --mqtt-password=MQTT_PASSWORD
                        MQTT broker connert password
  -H DUMP1090_HOST, --dump1090-host=DUMP1090_HOST
                        dump1090 hostname
  -P DUMP1090_PORT, --dump1090-port=DUMP1090_PORT
                        dump1090 port number
  -r RADAR, --radar-name=RADAR
                        name of radar. used as topic string
                        /adsb/RADAR/icaoaddr
  -c, --console         write out topic and payload to console
  ```

example
---------

```
./dump1090pub.py --radar T-RJTT36
```

output
-------

|Topic| Payload |
|:--- |:--- |
|`/adsb/RADAR/ICAOADDR` | Same as port 30003 of [dump1090](https://github.com/antirez/dump1090), CSV in [the format of SBS BaseStation](http://www.homepages.mcb.net/bones/SBS/Article/Barebones42_Socket_Data.htm).


building docker container
--------------------------

```
docker build -t fabriziofiorucci/dump1090mqtt:1.0 .
docker tag fabriziofiorucci/dump1090mqtt:1.0 my-registry:5000/fabriziofiorucci/dump1090mqtt:1.0
docker push my-registry:5000/fabriziofiorucci/dump1090mqtt:1.0
```

sample docker-compose.yml
--------------------------

```
version: "3"

services:

  dump1090mqtt:
    image: my-registry:5000/fabriziofiorucci/dump1090mqtt:1.0
    container_name: dump1090mqtt
    restart: always
    environment:
      - MQTT_HOST=mqtt-broker.fqdn.tld
      - MQTT_PORT=1883
      - MQTT_USER=mqtt-username
      - MQTT_PASS=mqtt-password
      - DUMP1090_HOST=dump1090.fqdn.tld
      - DUMP1090_PORT=30003
      - RADARNAME=my-radar-name
```
