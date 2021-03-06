# IoT Node

Network enabled device for data exchange using MQTT protocol. This device periodically
reads data from [DHT22](http://www.aosong.com/en/products/details.asp?id=117) sensor
and publish them to network over MQTT protocol. This is part of my IoT project. You can
read more about it on my [blog](http://buben19.blogspot.com/).

## Configuration

Copy sample configuration file to create node config:

    $ cd src
    $ cp config.h.sample config.h
    $ vi config.h

Edit following values config.h configuration file:

 - `ETH_ADDR0` ... `ETH_ADDR5` - Edit those values to unique MAC address.
 - `CONFIG_IP_ADDR0` ... `CONFIG_IP_ADDR` - Edit those values to assign LAN address.
 - `CONFIG_NETMASK0` ... `CONFIG_NETMASK3` - Edit those values to assign netmask.
 - `MQTT_BROKER_IP_ADDR0` ... `MQTT_BROKER_IP_ADDR0` - Edit those values to assign
    MQTT broker IP address.
 - `MQTT_BROKER_PORT` - Configure MQTT broker port.
 - `MQTT_TOPIC_TEMPERATURE` - Configure temperature topic name.
 - `MQTT_TOPIC_HUMIDITY` - Configure humidity topic name.
 - `MQTT_PUBLISH_PERIOD` - Data publish period in seconds. DHT22 sensor requires
   at minimum 2 seconds.
 - `MQTT_KEEP_ALIVE` - MQTT keep alive interval.
 - `MQTT_CLIENT_ID` - MQTT client ID.
 - `MQTT_NODE_PRESENCE` - Set to non-zero to enable node presence messages.
 - `MQTT_NODE_PRESENCE_MSG_ONLINE` - Presence online message.
 - `MQTT_NODE_PRESENCE_MSG_ONLINE` - Presence offline message.

## Data output

Device sends humidity and temperature measurements on topic `MQTT_TOPIC_HUMIDITY` and
`MQTT_TOPIC_TEMPERATURE` respectively. When reading from sensors is successful,
payload is real positive (humidity, temperature) or negative (temperature only) number.

When reading from sensor fails, payload for each topic is appropriate error code:

 - `E_CHECKSUM` - Data checksum is incorrect.
 - `E_TIMEOUT` - Data reading timeouted.
 - `E_CONNECT` - Sensor connection was failed.
 - `E_ACK` - Error when expecting ACK signal from DHT-22 sensor.

### Node presence

When device connects to the broke, it will send presence message defined in `MQTT_NODE_PRESENCE_MSG_ONLINE` to topic `presence/<device_name>` with retain bit. It also defines last will message defined in `MQTT_NODE_PRESENCE_MSG_ONLINE` to the same topic.

## Building

After configuration is done, build IoT node software with command `make`

### Upload

To upload software into AVR use command `make avrdude`

## Development

Node has implemented code for DHCP client to dynamically assign IP address. This
feature is currently in experimental phase and it is not well tested. Future
versions should also include DNS client to obtain IP address of MQTT broker from
local DNS server.
