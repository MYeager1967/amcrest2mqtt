# amcrest2mqtt

Original work done by D Chesterton. I have done a little work so far and updated most of the libraries so that they are current.

A simple app to expose all events generated by an Amcrest device to MQTT using the
[`python-amcrest`](https://github.com/tchellomello/python-amcrest) library.

It supports the following environment variables:

-   `AMCREST_HOST` (required)
-   `AMCREST_PORT` (optional, default = 80)
-   `AMCREST_USERNAME` (optional, default = admin)
-   `AMCREST_PASSWORD` (required)
-   `MQTT_USERNAME` (required)
-   `MQTT_PASSWORD` (optional, default = empty password)
-   `MQTT_HOST` (optional, default = 'localhost')
-   `MQTT_QOS` (optional, default = 0)
-   `MQTT_PORT` (optional, default = 1883)
-   `MQTT_TLS_ENABLED` (required if using TLS) - set to `true` to enable
-   `MQTT_TLS_CA_CERT` (required if using TLS) - path to the ca certs
-   `MQTT_TLS_CERT` (required if using TLS) - path to the private cert
-   `MQTT_TLS_KEY` (required if using TLS) - path to the private key
-   `HOME_ASSISTANT` (optional, default = false)
-   `HOME_ASSISTANT_PREFIX` (optional, default = 'homeassistant')
-   `STORAGE_POLL_INTERVAL` (optional, default = 3600) - how often to fetch storage data (in seconds) (set to 0 to disable functionality)
-   `DEVICE_NAME` (optional) - override the default device name used in the Amcrest app

It exposes events to the following topics:

-   `amcrest2mqtt/[SERIAL_NUMBER]/event` - all events
-   `amcrest2mqtt/[SERIAL_NUMBER]/doorbell` - doorbell status (if AD110 or AD410)
-   `amcrest2mqtt/[SERIAL_NUMBER]/human` - human detection (if AD410)
-   `amcrest2mqtt/[SERIAL_NUMBER]/motion` - motion events (if supported)
-   `amcrest2mqtt/[SERIAL_NUMBER]/config` - device configuration information
-   `amcrest2mqtt/[SERIAL_NUMBER]/ivs/[RuleName]` - status of IVS rules as defined on camera (if used)

## Device Support

The app supports events for any Amcrest device supported by [`python-amcrest`](https://github.com/tchellomello/python-amcrest).

## Home Assistant

The app has built-in support for Home Assistant discovery. Set the `HOME_ASSISTANT` environment variable to `true` to enable support.
If you are using a different MQTT prefix to the default, you will need to set the `HOME_ASSISTANT_PREFIX` environment variable.

## Running the app

The easiest way to run the app is via Docker Compose, e.g.

```yaml
version: "3"
services:
    amcrest2mqtt:
        container_name: amcrest2mqtt
        image: dchesterton/amcrest2mqtt:latest
        restart: unless-stopped
        environment:
            AMCREST_HOST: 192.168.0.1
            AMCREST_PASSWORD: password
            MQTT_HOST: 192.168.0.2
            MQTT_USERNAME: admin
            MQTT_PASSWORD: password
            HOME_ASSISTANT: "true"
```

## Out of Scope

### Multiple Devices

The app will not support multiple devices. You can run multiple instances of the app if you need to expose events for multiple devices.

### Non-Docker Environments

Docker is the only supported way of deploying the application. The app should run directly via Python but this is not supported.

### Home Assistant Addons

There are a couple of Home Assistant Addons that use my code to be able to port this software into Supervised versions of Home Assistant. I do not specifically support the add-ons themselves, only the base software in the original docker format. Please contact the authors of those add-ons for support if using that method.

https://github.com/ikifar2012/amcrest2mqtt-addon/blob/master/README.md
[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fikifar2012%2Fha-addons)

https://github.com/robsonke/hassio-addons/blob/master/amcrest2mqtt/README.md
[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Frobsonke%2Fhassio-addons)
