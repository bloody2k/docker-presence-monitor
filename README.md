# docker-presence-monitor

Docker container image for the bluetooth presence monitor found [here](https://github.com/andrewjfreyer/monitor)

## Usage

```bash
docker run -d \
    --name monitor \
    --net host \
    --privileged \
    --volume /path/to/your/config:/config \
    mashupmill/presence-monitor
```

You can also pass additional options (i.e. `-b -tad`) to the end of the command. For example...

```bash
docker run -d \
    --name monitor \
    --net host \
    --privileged \
    --volume /path/to/your/config:/config \
    mashupmill/presence-monitor \
    -b -tad
```

Depending on your system architecture, you may need run a different tag. It was attempted to mitigate this by using the docker manifest functionality, however, currently when docker pulls images it doesn't do a good job distinguishing arm v6 from arm v7. So for example, on a Raspberry Pi Zero W which uses arm v6, docker pulls one of the images marked as arm, but ignores the variant. It's been an [issue](https://github.com/moby/moby/issues/34875) for a couple years :\ 

There are various tags available...

* `latest` ... which is the manifest which docker is supposed to auto detect the architecture and then subsequently pull the correct image for your architecture
* `latest-amd64`
* `latest-armv6`
* `latest-armv7`
* `latest-arm64`

There are also version specific tags (where the version corresponds with the version of monitor.sh), for example...

* `0.2.197` ... which is the manifest which docker is supposed to auto detect the architecture and then subsequently pull the correct image for your architecture
* `0.2.197-amd64`
* `0.2.197-armv6`
* `0.2.197-armv7`
* `0.2.197-arm64`

There are also beta version specific tags, for example...

* `beta` ... which is the manifest which docker is supposed to auto detect the architecture and then subsequently pull the correct image for your architecture
* `beta-amd64`
* `beta-armv6`
* `beta-armv7`
* `beta-arm64`


### Configuration

You can either mount your own configuration files into the `/config` directory. Any config file that is missing will get created it will be populated with data from the environment variables.

Variables that you normally would have put in the `behavior_preferences` file can be set as regular docker environment variables. The MQTT preferences can also be set via environments variables like: `MQTT_ADDRESS`, `MQTT_USER`, `MQTT_PASSWORD`, `MQTT_TOPICPATH`, `MQTT_PUBLISHER_IDENTITY`, `MQTT_PORT`, `MQTT_CERTIFICATE_PATH` and `MQTT_VERSION`. The address

You can set the `ADDRESS_BLACKLIST`, `KNOWN_BEACON_ADDRESSES`, and `KNOWN_STATIC_ADDRESSES` all in a similar manner. For example, you can pass the environment like this...

```bash
--env 'KNOWN_BEACON_ADDRESSES=00:00:00:00:00:00 Nickname1
00:00:00:00:00:00 Nickname2
00:00:00:00:00:00 Nickname3'
```