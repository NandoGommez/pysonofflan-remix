# Encryption

## When is it used?

All known Sonoff devices running Firmware 3+ have encryption implemented unless they are running in DIY mode.

DIY mode is a feature enabled on some devices, typically those branded DIY or R3. It is enabled using a jumper on the mainboard.

For devices with encryption enabled, the api_key is needed to read the device status or to operate the device using the API. The device, however, can be discovered without the key. 

## Obtaining the api_key

**Note**: the api key **IS NOT** the selfApiKey that can be found in traces.

The api_key is not designed to be captured by users and is not available in any user interfaces.

To capture the api_key there are a number of options;

* Capture with V2 firmware: If you have V2 firmware and are using the earlier version of this component/package, the api_key is visible in the debug logs at startup (part of the "user online response" message)

* Capture during pairing using a browser: You can use the method described [here](https://blog.ipsumdomus.com/sonoff-switch-complete-hack-without-firmware-upgrade-1b2d6632c01). Despite this guide being quite old and for older firmware, the early part where the api_key is uncovered still works for some devices. Unfortunately this is only visible this way during pairing

* Capture using a network trace tool. 'tcpdump' is a popular tool and can be run on an OpenWRT router. The key is visible during pairing, also sometimes it is visibile when the device starts up (not sure if this is device or firmware specific)

1. Connect to the router with ssh or telnet from your PC
2. Connect your mobile to the router SSID
3. Execute this command in the WRT router: “tcpdump -s 0 -vvv -i wlan1 -w | grep apikey” (you may need to substitute wlan1 for the correct interface)
4. Register the sonoff in the eWeLink App
5. You will get the apikey displayed like this:
{“type”:1,“deviceid”:“xxxxxxxxxx”,“apikey”:"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",“rptInfo”:{“code”:3100,“arg”:"{“rstReason”:0}"},“sequence”:“0845895318697”}^/G

* Capture via Charles Proxy

This is a method which shows how to do it
https://community.home-assistant.io/t/new-custom-component-sonoff-lan-mode-local-with-stock-firmware/88132/99?u=mattsaxon

It's based on these instructions here;
https://community.hubitat.com/t/sonoff-ewelink-via-homebridge-node/3753

Thanks to @SuperMario

## Notes on the api_key
* For a given device, it is constant, it doesn't change with repairing for example
* It is different for every device
* It is not needed for a given device if it is in DIY mode (though it returns to the original key if you put it back in standard mode)
* It is not the same as the selfApiKey that can be found in traces.
