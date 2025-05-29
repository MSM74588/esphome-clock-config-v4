# esphome-clock-config-v4
esphome configuration for max7219 matrix clock.

## Parametes set: (in the yaml)
- Log level: `ERROR`
- web server version: `v3`
- web server: local -> `TRUE` (higher ram usage, but totally local)
- time server: using `hassio`, for true standalone use, use a `ntp` server.
- platform: `esp-idf`

## How to use the web server API?
Turn off clock display: `curl -X POST 'http://192.168.0.100/switch/keep_clock_on/turn_off'`
Turn on clock display: `curl -X POST 'http://192.168.0.100/switch/keep_clock_on/turn_on'`
Set Brightness level: `curl -X GET 'http://192.168.0.100/switch/show_display_text/'`

Note:
- only adding the `domain` will not work, `entity id` has to be also added.
- make a `GET` request to get the entity state and values, add `?detail=all` to get more details
- authentication can be added: simple `digest auth` kind

more documentation: [esphome webserver web api](https://esphome.io/web-api/index.html)

note: - see the max suffix, so one single firmware can be used for all devices
```yaml
esphome:
  name: "${name}"
  # Friendly names are used where appropriate in Home Assistant
  friendly_name: "${friendly_name}"
  # Automatically add the mac address to the name
  # so you can use a single firmware for all devices
  name_add_mac_suffix: true

  # This will allow for (future) project identification,
  # configuration and updates.
  project:
    name: esphome.project-template
    version: "1.0"
```

- added `dashboard_import` to make the config shareable


> [!NOTE]
> When you connect to the fallback network, the web interface should open automatically (see also login to network notifications).
> If that does not work, you can also navigate to http://192.168.4.1/ manually in your browser.

## Instruction to connect to a new HA instance
For freshly installed device:

### Via Wifi
- Connect to the Fallback Network
- Go to `192.168.4.1` -> give wifi credentials
- Go to `esphome` dashboard > Discovered device > configure the device > Copy/save the `API key` > install (will happen over wifi)
- Go to `HA > Devices` > Reconfigure > Done.

### Via Serial ([esphome web](https://web.esphome.io/))
- Connect via USB-Serial (via a chrome based browser)
- On linux may need to give permission to the usb device, by adding the `user` to `uucp` group (generally /usb/ttyACM0)
- Connect > then 3 dots > `Configure Wi-fi`